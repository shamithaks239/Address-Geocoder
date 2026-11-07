# Address-Geocoder
A visit-informed geocoding system that combines field-agent GPS trails, Indian address parsing, landmark intelligence, and evidence fusion to generate more accurate location pins with calibrated confidence radii. Evaluated against a commercial geocoder, achieving 84.9 m median error compared to 376.4 m on 100 surveyed addresses.
# Address Geocoder That Learns from Field Visits

**Open IIT Data Analytics | Problem Statement 3**

A visit-informed geocoding system that combines field-agent GPS trails, Indian address parsing, landmark intelligence, and evidence fusion to generate more accurate location estimates with calibrated confidence radii.

## Problem Statement

In tier-2, tier-3 and rural areas, addresses are often expressed through local landmarks, informal directions, cross roads and building references rather than precise street addresses.

Conventional geocoders frequently return locality or pincode-level coordinates, which can be hundreds of metres or kilometres away from the actual location.

However, field agents already collect valuable location evidence during their visits through GPS check-ins, movement trails, dwell times and visit outcomes.

**Our approach:** Use this existing field-visit evidence to continuously improve address geocoding, estimate location uncertainty and distinguish between an address that is difficult to locate and one that may no longer be valid.

## Key Features

- **Multilingual Indian Address Parsing:** Handles Kannada, Devanagari and Latin-script addresses, including transliterations, abbreviations, misspellings and landmark-based references.
- **Visit-Based Location Intelligence:** Extracts useful location evidence from GPS trails, visit outcomes and dwell times rather than blindly trusting check-in coordinates.
- **Grid-Based Geocoding:** Learns spatial patterns from structured address components such as cross roads, main roads, gali numbers, wards, blocks and roads.
- **Landmark Resolution:** Identifies landmark references and associates addresses with known landmark and locality information.
- **Cross-Account Entity Resolution:** Uses shared landmarks and address patterns to transfer location intelligence between accounts referring to the same place.
- **Evidence Fusion:** Combines multiple location sources using inverse-variance weighting and outlier rejection.
- **Calibrated Confidence Radius:** Uses conformal calibration to estimate a meaningful 90% location confidence radius.
- **Comparative Evaluation:** Benchmarks the proposed approach against a supplied commercial geocoder using surveyed ground-truth locations.

## ⚙️ Methodology

The solution follows a multi-stage geocoding pipeline.

### 1. Data Analysis and Evidence Assessment

Analyses field visits to understand which outcomes and GPS observations provide reliable building-location evidence.

Key factors include:
- GPS check-in coordinates versus the final point of the visit trail.
- Visit outcomes and their relevance to building-location evidence.
- Agent dwell time and GPS trail characteristics.
- Reliability of individual visits and repeated observations.

### 2. Indian Address Parsing

A custom rule-based address parser extracts meaningful spatial information from unstructured addresses.

It handles:
- Kannada, Devanagari and Latin scripts.
- Landmark names and proximity expressions.
- Locality abbreviations and common misspellings.
- Pincode extraction and correction.
- Town-specific addressing conventions.

### 3. Spatial Grid Learning

Learns relationships between structured address components and observed visit locations.

Different addressing conventions are handled through town-specific grid models, allowing address components such as cross numbers, gali numbers and block-road combinations to contribute to location estimation.

### 4. Location Evidence Fusion

Combines multiple candidate location sources:

- Historical field-visit evidence.
- Locations of other accounts sharing the same address or landmarks.
- Learned spatial grid predictions.
- Commercial geocoder coordinates.
- Landmark coordinates.
- Locality and pincode centroids.

An inverse-variance weighted fusion approach gives greater importance to more reliable sources.

An outlier rejection mechanism prevents inconsistent location evidence from significantly distorting the final estimate.

### 5. Confidence Calibration

The system estimates uncertainty for every predicted location.

Conformal calibration is applied using held-out, high-agreement visit evidence to determine a 90% confidence radius.

This allows the system to distinguish between high-confidence locations and addresses requiring further verification.

### 6. Evaluation and Ablation Analysis

The complete system is evaluated against surveyed ground-truth locations.

Ablation experiments examine the contribution of individual components, including:
- Commercial geocoder integration.
- Learned spatial grid models.
- Outlier rejection.
- GPS trail endpoints.
- Broader visit-outcome inclusion.
- Pincode correction.

A leakage check verifies that surveyed ground-truth coordinates do not influence the prediction pipeline.

## Results

Evaluation on 100 surveyed addresses:

| Metric | Proposed System | Commercial Geocoder |
|---|---:|---:|
| Median Error | **84.9 m** | 376.4 m |
| 90th Percentile Error | 462.3 m | 839.2 m |
| Mean Error | 265.1 m | 532.7 m |

### Key Findings

- Achieved approximately **77.4% lower median location error** than the commercial baseline.
- Outperformed the commercial geocoder on **84 out of 100 surveyed addresses**.
- Improved location estimation by combining historical field evidence with address-derived spatial information.
- Demonstrated the importance of using GPS trail endpoints instead of relying exclusively on initial check-in coordinates.
- Showed that visits where the borrower was not present can still provide useful building-location evidence.

## Tech Stack

- **Language:** Python
- **Data Analysis:** Pandas, NumPy
- **Visualization:** Matplotlib
- **Statistical Analysis:** SciPy
- **Environment:** Google Colab / Jupyter Notebook
- **Core Techniques:** Geospatial analysis, rule-based NLP, spatial grid learning, inverse-variance fusion, outlier detection and conformal calibration.

## Repository Structure

```text
address-geocoder-field-intelligence/
│
├── PS3_Address_Geocoder.ipynb
|PointWise_Address_Geocoder_PPT
└── README.md
```

The notebook contains the complete implementation, including address parsing, evidence fusion, spatial grid learning, confidence calibration and evaluation.

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

### 2. Open the notebook

Open `PS3_Address_Geocoder.ipynb` using Google Colab or Jupyter Notebook.

### 3. Prepare the dataset

The original challenge dataset is not included in this repository.

Place the provided data directory in Google Drive with the following structure:

```text
data/
│
├── shared/
│   ├── addresses.csv
│   └── field_visits.csv
│
└── ps3_geocoder/
    ├── visit_gps_points.csv
    ├── landmarks_poi.csv
    ├── localities.csv
    ├── towns.csv
    ├── baseline_geocodes.csv
    └── surveyed_addresses.csv
```

### 4. Configure the data path

Update the `DATA` variable in the notebook to point to your dataset directory.

```python
DATA = '/content/drive/MyDrive/Data/data/'
```

The notebook uses Google Drive mounting in its current configuration.

### 5. Install dependencies

```bash
pip install numpy pandas matplotlib scipy
```

### 6. Execute the notebook

Run all cells sequentially to reproduce the analysis, build the geocoder, calibrate confidence radii and evaluate the predictions.

## Limitations

- The evaluation dataset is synthetic; reported accuracy should not be interpreted as real-world deployment performance.
- Ground-truth evaluation is based on a relatively small sample of 100 surveyed addresses.
- Rural addresses outside the three towns are detected and flagged rather than accurately geocoded.
- Real-world GPS spoofing and fraudulent check-ins require further validation.
- Commercial geocoder fallback remains an area for improvement.

## Potential Future Improvements

- Integrate multilingual transformer-based address understanding.
- Incorporate real-world GPS data with stronger visit-integrity checks.
- Extend spatial grid learning to rural and irregular settlement patterns.
- Develop an offline-compatible field-agent application.
- Integrate geocoding confidence with intelligent visit planning.
- Implement continuous model updates as new field-visit evidence becomes available.

## Project

Developed as part of **Open IIT Data Analytics – Problem Statement 3: Address Geocoder That Learns from Field Visits**.

The project explores how existing field-operation data can be transformed into actionable geospatial intelligence to improve address discovery and field collection efficiency.
