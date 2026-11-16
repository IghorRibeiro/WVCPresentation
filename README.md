# Maritime Search and Rescue: An Evaluation of Small-Object Detection in UAV Imagery

**Ighor de Souza Ribeiro** (CIAA), **Rigel Procópio Fernandes** (DSAM)

WVC 2026 — XXI Workshop on Computer Vision

**[Slides (PDF)](slides/WVC2026_slides_en.pdf)** · [Slides (PPTX)](slides/WVC2026_slides_en.pptx) · [Code and parameters](https://github.com/IghorRibeiro/yolo26n-seadronessee-postprocessing)

Versão em português: [README.pt.md](README.pt.md) · [Slides (PDF)](slides/WVC2026_slides.pdf) · [Slides (PPTX)](slides/WVC2026_slides.pptx)

---

## Slide 2 · Brazil's maritime SAR responsibility

<img src="media/en/slide02_sar_responsibility.png" width="100%" alt="NAVAREA V (Brazil) between South America and Africa">

The Blue Amazon covers about 5.7 million km²; the maritime SAR area under Brazil's responsibility is about 14.5 million km², out to 10°W (the map shows NAVAREA V, cropped from the IMO/IHO NAVAREA map; the SAR area extends farther east).

## Slide 3 · Searching at sea: ship vs. UAV

<img src="media/en/slide03_ship_vs_uav.png" width="100%" alt="Expanding square search and speed of a patrol vessel versus a commercial UAV">

In an expanding square search, a commercial UAV at about 20 m/s flies the same legs 3 to 4 times faster than a patrol vessel at 10–12 knots.

## Slide 4 · The problem: targets of a few pixels

<img src="media/en/slide04_problem.png" width="100%" alt="SeaDronesSee image with labels and the same target in fewer pixels">

With the longer side at 1280 px, the median target size is 16 px; for life-saving appliances, 9.4 px.

## Slide 7 · Two heads, the same weights

<img src="media/en/slide07_heads.png" width="100%" alt="One-to-one head without NMS and one-to-many head with NMS">

The one-to-one head outputs a single prediction per object and needs no NMS; the one-to-many head produces several candidates and needs NMS.

## Slide 8 · Hard-NMS vs. Soft-NMS

<img src="media/en/slide08_nms.png" width="100%" alt="Illustrative example of Hard-NMS and linear Soft-NMS">

Hard-NMS is all or nothing at the IoU threshold; linear Soft-NMS lowers the confidence with the overlap (illustrative example).

## Slide 9 · SAHI and Flip + WBF

<img src="media/sahi_sweep.gif" width="60%" alt="Frame swept by overlapping tiles">

SAHI processes overlapping 640-px tiles at native resolution (about 39 forward passes per image; illustrative grid). Flip + WBF fuses the boxes of the image and its mirror (2 passes).

## Slide 12 · Cost vs. accuracy

<img src="media/en/slide12_cost_accuracy.png" width="100%" alt="Validation AP against latency per image for the seven 1280-px configurations">

Single pass with linear Soft-NMS: 48.50 AP at 31 ms. Flip + WBF: 46.13 AP at 59 ms. SAHI: 31.17 to 37.38 AP at about 760 ms.

## Slide 13 · Why does SAHI lose? (hypothesis)

<img src="media/en/slide13_sahi_hypothesis.png" width="100%" alt="Scale mismatch between training and SAHI tiles">

Hypothesis, not tested: in native-resolution tiles the objects appear about 3× larger than in training, and no slicing-aided fine-tuning was applied.

## Slide 15 · Official SeaDronesSee test set

<img src="media/en/slide15_official_test.png" width="100%" alt="AP and AP50 on the official server">

AP 45.90 → 50.01 and AP50 73.49 → 79.71 from Default (one-to-one head, confidence 0.25) to linear Soft-NMS; final AP75 of 52.53.

## Slide 16 · Where does the gain come from?

<img src="media/en/slide16_gain_decomposition.png" width="100%" alt="Decomposition of the validation AP gain">

On validation, of the 4.25 points: 2.54 come only from the confidence threshold (a protocol effect), 1.25 from the one-to-many head with Hard-NMS and 0.46 from Soft-NMS.
