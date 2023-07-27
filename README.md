
<p align="center"><img src=".github/img/logo2.png" alt="PERMETRICS" title="PERMETRICS"/></p>


---

[![GitHub release](https://img.shields.io/badge/release-1.4.0-yellow.svg)](https://github.com/thieu1995/permetrics/releases)
[![Wheel](https://img.shields.io/pypi/wheel/gensim.svg)](https://pypi.python.org/pypi/permetrics) 
[![PyPI version](https://badge.fury.io/py/permetrics.svg)](https://badge.fury.io/py/permetrics)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/permetrics.svg)
![PyPI - Status](https://img.shields.io/pypi/status/permetrics.svg)
![PyPI - Downloads](https://img.shields.io/pypi/dm/permetrics.svg)
[![Downloads](https://pepy.tech/badge/permetrics)](https://pepy.tech/project/permetrics)
[![Tests & Publishes to PyPI](https://github.com/thieu1995/permetrics/actions/workflows/publish-package.yaml/badge.svg)](https://github.com/thieu1995/permetrics/actions/workflows/publish-package.yaml)
![GitHub Release Date](https://img.shields.io/github/release-date/thieu1995/permetrics.svg)
[![Documentation Status](https://readthedocs.org/projects/permetrics/badge/?version=latest)](https://permetrics.readthedocs.io/en/latest/?badge=latest)
[![Chat](https://img.shields.io/badge/Chat-on%20Telegram-blue)](https://t.me/+fRVCJGuGJg1mNDg1)
![GitHub contributors](https://img.shields.io/github/contributors/thieu1995/permetrics.svg)
[![GitTutorial](https://img.shields.io/badge/PR-Welcome-%23FF8300.svg?)](https://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project)
[![DOI](https://zenodo.org/badge/280617738.svg)](https://zenodo.org/badge/latestdoi/280617738)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)


PerMetrics is a python library for performance metrics of machine learning models. We aim to implement all 
performance metrics for problems such as regression, classification, clustering, ... problems. Helping users in all 
field access metrics as fast as possible

* **Free software:** GNU General Public License (GPL) V3 license
* **Total metrics**: 94 (47 regression metrics, 16 classification metrics, 31 clustering metrics)
* **Documentation:** https://permetrics.readthedocs.io/en/latest/
* **Python versions:** > 3.7.x
* **Dependencies:** numpy, scipy


# Notification

* **Currently, there is a huge misunderstanding among frameworks around the world about the notation of R, R2, and R^2.** 
* Please read the file [R-R2-Rsquared.docx](https://github.com/thieu1995/permetrics/blob/master/R-R2-Rsquared.docx) to understand the differences between them and why there is such confusion.

<p align="center"><img src=".github/img/rr2.png" alt="R" title="R"/></p>

* **My recommendation is to denote the Coefficient of Determination as COD or R2, while the squared Pearson's 
  Correlation Coefficient should be denoted as R^2 or RSQ (as in Excel software).**


# Installation

### Install with pip
Install the [current PyPI release](https://pypi.python.org/pypi/permetrics):
```sh 
$ pip install permetrics==1.4.0
```

Or installing from the source code, use:
```sh 
$ git clone https://github.com/thieu1995/permetrics.git
$ cd permetrics
$ python setup.py install
```

Or install the development version from GitHub:
```bash
pip install git+https://github.com/thieu1995/permetrics
```


# Usage

After installation, you can import Permetrics as any other Python module:

```sh
$ python
>>> import permetrics
>>> permetrics.__version__
```

**Let's go through some examples. The more complicated test case in the folder: examples**

The [documentation](https://permetrics.readthedocs.io/) includes more detailed installation instructions and explanations.


### Example with Regression metrics


```python
import numpy as np
from permetrics import RegressionMetric

## For 1-D array
y_true = [3, -0.5, 2, 7]
y_pred = [2.5, 0.0, 2, 8]

evaluator = RegressionMetric(y_true, y_pred, decimal=5)
print(evaluator.RMSE())
print(evaluator.MSE())

## For > 1-D array
y_true = np.array([[0.5, 1], [-1, 1], [7, -6]])
y_pred = np.array([[0, 2], [-1, 2], [8, -5]])

evaluator = RegressionMetric(y_true, y_pred, decimal=5)
print(evaluator.RMSE(multi_output="raw_values", decimal=5))
print(evaluator.MAE(multi_output="raw_values", decimal=5))
```


### Example with Classification metrics

```python
from permetrics import ClassificationMetric

## For integer labels or categorical labels
y_true = [0, 1, 0, 0, 1, 0]
y_pred = [0, 1, 0, 0, 0, 1]

# y_true = ["cat", "ant", "cat", "cat", "ant", "bird", "bird", "bird"]
# y_pred = ["ant", "ant", "cat", "cat", "ant", "cat", "bird", "ant"]

evaluator = ClassificationMetric(y_true, y_pred, decimal=5)

## Call specific function inside object, each function has 3 names like below

print(evaluator.f1_score())
print(evaluator.F1S(average="micro"))
print(evaluator.f1s(average="macro"))
print(evaluator.f1s(average="weighted"))

```

### Example with Clustering metrics

```python
import numpy as np
from permetrics import ClusteringMetric

# generate sample data
X = np.random.uniform(-1, 10, size=(500, 7))        # 500 examples, 7 features
y_true = np.random.randint(0, 4, size=500)          # 4 clusters
y_pred = np.random.randint(0, 4, size=500)

evaluator = ClusteringMetric(y_true=y_true, y_pred=y_pred, X=X, decimal=5)

## Call specific function inside object, each function has 2 names (fullname and short name)
##    + Internal metrics: Need X and y_pred and has suffix as index
##    + External metrics: Need y_true and y_pred and has suffix as score

print(evaluator.ball_hall_index())
print(evaluator.BHI())
```

### Metrics


| **Problem** | **STT** | **Metric** | **Metric Fullname** |                 **Characteristics**                 |
|:---:|:--:|:----------:|:---:|:---------------------------------------------------:|
| **Regression** | 1 |    EVS     | Explained Variance Score |   Greater is better (Best = 1), Range=(-inf, 1.0]   |
| **** | 2 |     ME     | Max Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 3 |    MBE     | Mean Bias Error |            Best = 0, Range=(-inf, +inf)             |
| **** | 4 |    MAE     | Mean Absolute Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 5 |    MSE     | Mean Squared Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 6 |    RMSE    | Root Mean Squared Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 7 |    MSLE    | Mean Squared Log Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 8 |   MedAE    | Median Absolute Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 9 | MRE / MRB  | Mean Relative Error / Mean Relative Bias |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 10 |    MPE     | Mean Percentage Error |            Best = 0, Range=(-inf, +inf)             |
| **** | 11 |    MAPE    | Mean Absolute Percentage Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 12 |   SMAPE    | Symmetric Mean Absolute Percentage Error |     Smaller is better (Best = 0), Range=[0, 1]      |
| **** | 13 |   MAAPE    | Mean Arctangent Absolute Percentage Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 14 |    MASE    | Mean Absolute Scaled Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 15 |    NSE     | Nash-Sutcliffe Efficiency Coefficient |    Greater is better (Best = 1), Range=(-inf, 1]    |
| **** | 16 |    NNSE    | Normalized Nash-Sutcliffe Efficiency Coefficient |     Greater is better (Best = 1), Range=[0, 1]      |
| **** | 17 |     WI     | Willmott Index |     Greater is better (Best = 1), Range=[0, 1]      |
| **** | 18 |  R / PCC   | Pearson’s Correlation Coefficient |     Greater is better (Best = 1), Range=[-1, 1]     |
| **** | 19 | AR / APCC  | Absolute Pearson's Correlation Coefficient |     Greater is better (Best = 1), Range=[-1, 1]     |
| **** | 20 |    R2s     | (Pearson’s Correlation Index) ^ 2 |     Greater is better (Best = 1), Range=[0, 1]      |
| **** | 21 |  R2 / COD  | Coefficient of Determination |    Greater is better (Best = 1), Range=(-inf, 1]    |
| **** | 22 | AR2 / ACOD | Adjusted Coefficient of Determination |    Greater is better (Best = 1), Range=(-inf, 1]    |
| **** | 23 |     CI     | Confidence Index |    Greater is better (Best = 1), Range=(-inf, 1]    |
| **** | 24 |    DRV     |  Deviation of Runoff Volume |   Smaller is better (Best = 1.0), Range=[1, +inf)   |
| **** | 25 |    KGE     | Kling-Gupta Efficiency |    Greater is better (Best = 1), Range=(-inf, 1]    |
| **** | 26 |    GINI    | Gini Coefficient |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 27 | GINI_WIKI  | Gini Coefficient on Wikipage |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 28 |    PCD     | Prediction of Change in Direction |    Greater is better (Best = 1.0), Range=[0, 1]     |
| **** | 29 |     CE     | Cross Entropy |    Range(-inf, 0], Can't give comment about this    |
| **** | 30 |    KLD     | Kullback Leibler Divergence |            Best = 0, Range=(-inf, +inf)             |
| **** | 31 |    JSD     | Jensen Shannon Divergence |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 32 |    VAF     | Variance Accounted For | Greater is better (Best = 100%), Range=(-inf, 100%] |
| **** | 33 |    RAE     | Relative Absolute Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 34 |    A10     | A10 Index |     Greater is better (Best = 1), Range=[0, 1]      |
| **** | 35 |    A20     | A20 Index |     Greater is better (Best = 1), Range=[0, 1]      |
| **** | 36 |    A30     | A30 Index |     Greater is better (Best = 1), Range=[0, 1]      |
| **** | 37 |   NRMSE    | Normalized Root Mean Square Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 38 |    RSE     | Residual Standard Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 39 |  RE / RB   | Relative Error / Relative Bias |            Best = 0, Range=(-inf, +inf)             |
| **** | 40 |     AE     | Absolute Error |            Best = 0, Range=(-inf, +inf)             |
| **** | 41 |     SE     |  Squared Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 42 |    SLE     | Squared Log Error |    Smaller is better (Best = 0), Range=[0, +inf)    |
| **** | 43 |    COV     | Covariance |    Greater is better (No best value), Range=(-inf, +inf)    |
| **** | 44 |    COR     | Correlation |    Greater is better (Best = 1), Range=[-1, +1]     |
| **** | 45 |    EC      | Efficiency Coefficient |  Greater is better (Best = 1), Range=(-inf, +1]    |
| **** | 46 |    OI      | Overall Index  |    Greater is better (Best = 1), Range=(-inf, +1]    |
| **** | 47 |  CRM       | Coefficient of Residual Mass |   Smaller is better (Best = 0), Range=(-inf, +inf)  |
| **** | 48 |         |       |             |
| **Classification** | 1 |     PS     | Precision Score |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 2 |    NPV     | Negative Predictive Value |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 3 |     RS     | Recall Score |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 4 |     AS     | Accuracy Score |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 5 |    F1S     | F1 Score |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 6 |    F2S     | F2 Score |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 7 |    FBS     | F-Beta Score |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 8 |     SS     | Specificity Score |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 9 |    MCC     | Matthews Correlation Coefficient |    Higher is better (Best = 1), Range = [-1, +1]    |
| **** | 10 |     HL     | Hamming Loss |     Higher is better (Best = 1), Range = [0, 1]     |
| **** | 11 |     CKS     | Cohen's kappa score |  Higher is better (Best = +1), Range = [-1, +1]  |
| **** | 12 |     JSI     | Jaccard Similarity Coefficient |  Higher is better (Best = +1), Range = [0, +1]  |
| **** | 13 | GMS | Geometric Mean Score | Higher is better (Best = +1), Range = [0, +1]  |
| **** | 14 | GINI | GINI Index | Higher is better (Best = +1), Range = [0, +1]  |
| **** | 15 | ROC-AUC | ROC-AUC | Higher is better (Best = +1), Range = [0, +1]  |
| **** | 16 |     |       |             |


# References

### Official channels (questions, problems)

* Official source code repo: https://github.com/thieu1995/permetrics
* Official document: https://permetrics.readthedocs.io/
* Download releases: https://pypi.org/project/permetrics/
* Issue tracker: https://github.com/thieu1995/permetrics/issues
* Notable changes log: https://github.com/thieu1995/permetrics/blob/master/ChangeLog.md
* Official chat group: https://t.me/+fRVCJGuGJg1mNDg1

* This project also related to our another projects which are "meta-heuristics", "neural-network", and "optimization" 
  check it here
    * https://github.com/thieu1995/mealpy
    * https://github.com/thieu1995/metaheuristics
    * https://github.com/thieu1995/opfunu
    * https://github.com/thieu1995/enoppy
    * https://github.com/thieu1995/mafese
    * https://github.com/aiir-team


### Citation

If you are using permetrics in your project, we would appreciate a citations:

```code 
@software{thieu_nguyen_2020_3951205,
  author       = {Nguyen Van Thieu},
  title        = {Permetrics: A framework of performance metrics for artificial intelligence models},
  month        = jul,
  year         = 2020,
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.3951205},
  url          = {https://doi.org/10.5281/zenodo.3951205}
}
```

### Helpful links

1) https://www.debadityachakravorty.com/ai-ml/cmatrix/
2) https://neptune.ai/blog/evaluation-metrics-binary-classification
3) https://danielyang1009.github.io/model-performance-measure/
4) https://towardsdatascience.com/multi-class-metrics-made-simple-part-i-precision-and-recall-9250280bddc2
5) http://cran.nexr.com/web/packages/clusterCrit/vignettes/clusterCrit.pdf
6) https://torchmetrics.readthedocs.io/en/latest/
7) http://rasbt.github.io/mlxtend/user_guide/evaluate/lift_score/
8) https://www.baeldung.com/cs/multi-class-f1-score
9) https://kavita-ganesan.com/how-to-compute-precision-and-recall-for-a-multi-class-classification-problem/#.YoXMSqhBy3A
10) https://machinelearningmastery.com/precision-recall-and-f-measure-for-imbalanced-classification/
