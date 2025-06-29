# 🌌 M67 Cluster Membership Identification using Gaia DR3 + pyUPMASK
This project implements an **unsupervised clustering pipeline** to identify probable members of the open star cluster **M67 (NGC 2682)** using data from **Gaia DR3**. The method follows the **pyUPMASK algorithm** (Solin et al. 2020), which combines clustering, spatial statistics, and probabilistic modeling.

## 📈 Overview of Method

1. **Gaia DR3 Data Extraction**
   - Queried a 1° radius around M67 using ADQL via `astroquery.gaia`.
   - Filtered based on astrometric quality (e.g., RUWE, visibility_periods).

2. **Feature Selection**
   - Used: `pmra`, `pmdec`, `parallax`, `bp_rp`, `phot_g_mean_mag`

3. **Clustering**
   - Applied **KMeans** clustering on standardized feature space.

4. **Random Field Rejection (RFR)**
   - Used **Ripley’s K function** to reject spatially uniform (field-like) clusters.

5. **Gaussian + Uniform Mixture Model (GUMM)**
   - Modeled the (RA, Dec) distribution with a 2D GMM to compute **cluster membership probabilities**.

6. **Kernel Density Estimation (KDE)**
   - Smoothed the final membership probabilities based on local density.

---

## 📊 Output

- Final DataFrame includes:
  - `gumm_prob`: membership from Gaussian mixture model
  - `kde_prob`: smoothed membership probability
- Stars with `kde_prob > 0.8` considered probable cluster members (this threshold can be varied).
