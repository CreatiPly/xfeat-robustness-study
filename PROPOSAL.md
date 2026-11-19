# Semester Project Proposal - Track A (Research)

**Robustness Evaluation of XFeat, SIFT and ORB Under Combined Image Degradation**  
Student: Haseeb Khan | Roll / Registration no.: 539657

## 1. Benchmark paper

G. Potje, F. Cadar, A. Araujo, R. Martins and E. R. Nascimento, "XFeat: Accelerated Features for Lightweight Image Matching," CVPR 2024, pp. 2682-2691, doi:10.1109/CVPR52733.2024.00259. CVPR is a CORE A\* venue.

- Paper: https://openaccess.thecvf.com/content/CVPR2024/html/Potje_XFeat_Accelerated_Features_for_Lightweight_Image_Matching_CVPR_2024_paper.html
- Code and pretrained weights: https://github.com/verlab/accelerated_features
- Dataset (HPatches): https://github.com/hpatches

## 2. Result to reproduce

Table 3 of the paper (homography estimation on HPatches): Mean Homography Accuracy (MHA) at 3, 5 and 7 px on the illumination and viewpoint splits. I will reproduce the XFeat and ORB rows with my own Python evaluation code, following the paper's setup (top 4,096 keypoints, mutual nearest neighbour matching, MAGSAC++, four-corner reprojection error) and documenting any deviation. SIFT is not in that table; it is my added classical baseline. The values below are the paper's, not results from my code.

| Method (published) | Illumination @3 / @5 / @7 | Viewpoint @3 / @5 / @7 |
| ------------------ | ------------------------- | ---------------------- |
| XFeat              | 95.0 / 98.1 / 98.8        | 68.6 / 81.1 / 86.1     |
| ORB                | 74.6 / 84.6 / 85.4        | 63.2 / 71.4 / 78.6     |

## 3. Research question

**How does MHA on HPatches change for XFeat, SIFT and ORB when the target image is affected by Gaussian blur and brightness reduction, individually and in combination, and does the relative ranking of the methods change?**

## 4. Data and experimental design

HPatches full image sequences: 116 sequences with five reference-target pairs each (580 pairs), covering illumination and viewpoint changes. The reference image stays clean; only the target image is degraded, to simulate matching a degraded camera view against a clean reference. XFeat uses the official pretrained weights; SIFT and ORB use OpenCV. No training. Everything runs on a laptop CPU or free Colab.

| Condition       | Blur sigma | Brightness multiplier |
| --------------- | ---------- | --------------------- |
| Clean           | 0          | 1.0                   |
| Mild blur       | 1          | 1.0                   |
| Severe blur     | 2          | 1.0                   |
| Mild dimming    | 0          | 0.7                   |
| Severe dimming  | 0          | 0.4                   |
| Combined mild   | 1          | 0.7                   |
| Combined severe | 2          | 0.4                   |

This is a matched-severity comparison of individual and combined effects, not a full factorial design; settings are checked in a small pilot, then fixed for all methods. Metrics: MHA at 3/5/7 px, match count, inlier ratio and runtime on one machine. A few phone photos of a planar scene may be used for a qualitative demo only.

## 5. Milestones and fallback

- **By 4 December:** Repository and environment ready; HPatches loaded; clean-image evaluation implemented; XFeat and ORB compared with Table 3; SIFT added; any reproduction gap documented.
- **By 18 December:** All seven conditions run for the three methods; results and plots saved; 4-6 page IEEE paper, documented GitHub repository and skills-profile link submitted.
- **Fallback:** If full-dataset evaluation or protocol matching takes too long, use a fixed balanced subset of about 20 sequences (10 illumination, 10 viewpoint), clearly labelled as a subset. Last resort: reproduce XFeat's MegaDepth-1500 result (Table 1) with the authors' script and study blur only, XFeat vs ORB.

Expected contribution: an independent reproduction attempt of selected HPatches results, an added SIFT comparison, and a controlled study of how blur and dimming affect local-feature homography estimation. No new method or state-of-the-art result is claimed.
