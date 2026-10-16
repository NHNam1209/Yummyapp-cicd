# YummyApp Mobile Backend - CI/CD & Mobile Mocking Testing

[![CI Pipeline](https://github.com/NHNam1209/Yummyapp-cicd/actions/workflows/yummyapp-ci.yml/badge.svg)](https://github.com/NHNam1209/Yummyapp-cicd/actions/workflows/yummyapp-ci.yml)
[![Coverage](https://img.shields.io/badge/coverage-99%25-brightgreen.svg)](https://github.com/NHNam1209/Yummyapp-cicd)
[![Python Version](https://img.shields.io/badge/python-3.11%20%7C%203.14-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> **Software Testing Lab 07**: Mobile Mocking Testing & Continuous Integration (CI/CD)  
> **Student**: Nguyen Hoai Nam (MSSV: 2312694)  
> **Institution**: Dalat University (DLU) - Faculty of Information Technology  

---

## 📌 Project Overview

**YummyApp Mobile Backend** is a lightweight Python backend service for a food delivery mobile application. This repository demonstrates:
- **Mobile Mocking & Stubbing**: Isolating hardware peripherals (`gps_service.get_current_location`) and unstable mobile network states (`network_status="offline"` ném `ConnectionError`) using `unittest.mock.patch`.
- **Automated CI Pipeline**: Building an automated pipeline with GitHub Actions on clean `ubuntu-latest` runners.
- **Strict Quality Gates**: Enforcing zero-tolerance static code analysis (`flake8`) and strict code coverage thresholds (`coverage report --fail-under=80`).
- **Fail-Fast & Break the Build**: Early error detection and artifact archival for coverage reports.

---

## 🏗️ Architecture & CI/CD Pipeline

The Continuous Integration pipeline is configured via `.github/workflows/yummyapp-ci.yml` and triggers on every `push` and `pull_request` targeting the `main` branch.

```
+------------------+     +------------------+     +------------------+
| 1. Git Push / PR | --> | 2. Checkout Code | --> | 3. Setup Python  |
| (Branch: main)   |     | actions/checkout |     | Python 3.11      |
+------------------+     +------------------+     +------------------+
                                                           |
+------------------+     +------------------+              v
| 6. Upload HTML   | <-- | 5. Execute Tests | <-- +------------------+
|    Artifacts     |     | & Quality Gate   |     | 4. Install Deps  |
| upload-artifact  |     | Coverage >= 80%  |     | requirements.txt |
+------------------+     +------------------+     +------------------+
```

---

## 🧪 Test Matrix & Coverage Summary

### Test Suite Structure

1. **`TestEasyLevel` (Level 1 - Core Order Calculation)**
   - `test_calculate_total_for_single_item`: Valid cart calculation.
   - `test_calculate_total_for_multiple_items`: Multi-item cart calculation.
   - `test_calculate_total_for_empty_cart`: Empty cart edge-case ($0.0).
   - `test_calculate_total_with_zero_quantity`: Zero quantity item handling.
   - `test_gps_service_default`: Base hardware service test.

2. **`TestMediumLevel` (Level 2 - Promo Code & Network Validation)**
   - `test_apply_promo_online_valid`: Online discount application (10% off).
   - `test_apply_promo_offline_raises_connection_error`: Raises `ConnectionError` when device is offline.
   - `test_apply_promo_special_code_valid_os15`: OS version validation for exclusive promotions (20% off).
   - `test_apply_promo_special_code_lower_os14_raises_value_error`: Rejects outdated OS with `ValueError`.
   - `test_apply_promo_invalid_code_raises_value_error`: Invalid coupon code validation.

3. **`TestHardLevel` (Level 3 - GPS Peripheral Mocking)**
   - `test_get_nearby_restaurants_in_range`: Mocks GPS coordinates `(10.0, 105.0)` to return nearby restaurants.
   - `test_get_nearby_restaurants_out_of_range`: Mocks GPS coordinates `(20.0, 110.0)` returning empty list.
   - `test_get_nearby_restaurants_gps_signal_lost`: Mocks `gps_service.GPSSignalLostError` handling gracefully.

### Code Coverage Report

```
Name                Stmts   Miss  Cover   Missing
-------------------------------------------------
gps_service.py          4      0   100%
test_yummy_app.py      71      1    99%   112
yummy_app.py           30      0   100%
-------------------------------------------------
TOTAL                 105      1    99%
```
> **Quality Gate Status**: **PASSED** (99% $\ge$ 80% threshold).

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/NHNam1209/Yummyapp-cicd.git
cd Yummyapp-cicd
```

### 2. Install dependencies (Version-pinned)
```bash
pip install -r requirements.txt
```

### 3. Run Static Code Analysis (Linter)
```bash
flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
```

### 4. Run Unit Tests & Measure Coverage
```bash
coverage run -m unittest discover -s . -p "test_*.py"
coverage report -m
coverage report --fail-under=80
```

### 5. Generate HTML Coverage Report
```bash
coverage html -d coverage_html
```

---

## 📁 Repository Structure

```
├── .github/
│   └── workflows/
│       └── yummyapp-ci.yml           # GitHub Actions CI workflow
├── logs/
│   ├── local_test.log                # Local execution logs & coverage output
│   ├── git_history.log               # Git commit history log
│   └── github_actions_run.log        # Raw log from Ubuntu CI runner
├── BaoCao_KetQua_Lab7.pdf            # Formal laboratory report (PDF)
├── MinhChung_VietTay_MSSV.pdf        # Handwritten-style verification evidence (PDF)
├── gps_service.py                    # GPS service hardware mock target
├── requirements.txt                  # Pinned dependencies
├── test_yummy_app.py                 # Comprehensive unit test suite with mocking
├── yummy_app.py                      # Backend service business logic
└── README.md                         # Project documentation
```

---

## 👤 Author

- **Student**: Nguyen Hoai Nam (Nguyễn Hoài Nam)
- **ID (MSSV)**: 2312694
- **Email**: 2312694@dlu.edu.vn
- **GitHub**: [@NHNam1209](https://github.com/NHNam1209)
