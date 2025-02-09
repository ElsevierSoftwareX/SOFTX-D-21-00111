![https://pypi.python.org/pypi/explainerdashboard/](https://img.shields.io/pypi/v/AMLBID.svg)

# Transparent and Auto-explainable AutoML
<p align="center">
  <img src="https://github.com/LeMGarouani/AMLBID/blob/main/media/Framework.png" width="100%" />
</p>

---
# AMLBID
AMLBID stands for **A**utomating **M**achine-**L**earning model selection and configuration with **B**ig **I**ndustrial **D**ata.

Curently, **AMLBID** is a Python-Package representing a **meta learning**-based framework for automating the process of algorithm selection, and hyper-parameter tuning in supervised machine learning. Being meta-learning based, the framework is able to simulate the role of the machine learning expert as a **decision support system**. In particular, **AMLBID** is considered the first **complete**, **transparent** and **auto-explainable** AutoML system for **recommending** the most adequate ML configuration for a problem at hand, and **explain** the rationale behind the recommendation and analyzing the predictive results in an interpretable and faithful manner through an **interactive multiviews artifact**.  

**AMLBID** is an interactive and user-guided framework for improving the utility and usability of the *AutoML* process with the following main features:
 
* The framework provides  end-users *(Industry 4.0 actors & Researchers)* with a user-friendly *control panel* that allows nontechnical users and domain experts (e.g., physicians, researchers) to overcome machine-learning predictive models building and configuring process challenges according to their own preferences.

* The first framework system that automate machine-learning predictive models building and configuration for big industrial data.

* The framework is equipped with a **recommendation engine**, that provide suggestion of the most appropriate pipelines *(classifiers with their hyperparameters configuration)* through the use of a collaborative **knowledge-base** that grows by time as more users are using our tool.

* AMLBID will automate the most tedious part of machine learning by intelligently exploring more than **3.000.000** possible pipelines to find the best one for your data in a **negligible amount of time** and **without** need to a strong **computational budget**.

* Automatically select ML algorithms and hyperparameters configurations for a given machine-learning problem more quickly than current methods with a **computational complexity near O(1)**.

* Provide a multi-level interactive visualization artifact that facilitate the models workings and performance **inspection** to address the **“black-box trusting”**.

<p align="center">
<img alt="--" src="https://github.com/LeMGarouani/AMLBID/blob/main/media/AMLBID.png" width="80%" />
</p>

---
## Usage
The `Framework` will help you with:
 - Explaining and understanding your data.
 - Automate the  Algorithm Selection and Hyper-Parameters tuning process.
 - Provide reports from analysis with details about all models (Atomatic-Explanation).
 - Interactively inspect the inner workings of the models without having to depend on a data scientist to generate tables and plots.
 - Provide a guidance, when AutoML returns unsatisfying results, to improve to predictive performances.
 - Increase the transparency, controllability, and the acceptance of AutoML.

It has two built-in modes of work:
 - **`Recommender`** mode, for recommending and building highly-tuned ML pipelines to use in production.
 - **`Recommender_Explainer`** mode, which allow users to inspect the recommended model's inner working and decision’s generation process, with many explanations levels, like feature importances, feature contributions to individual predictions, "what if" analysis, SHAP (interaction) values, visualisation of individual decision trees, Hyperameters inportance and correlation etc.

Curently,supports 08 <a href = "https://scikit-learn.org/stable/"> Scikit-Learn </a> classification algorithms, `AdaBoost`, `Support Vector Classifier`, `Extra Trees`, `Gradient Boosting`, `Decision Tree`, `Logistic Regression`, `Random Forest`, and `Stochastic Gradient Descent Classifier`. 

## Installation
AMLBID is built on top of several existing Python libraries, including:
* numpy
* shap
* jupyter_dash
* xgboost
* [dtreeviz](https://github.com/parrt/dtreeviz) **Windows users:** *See https://github.com/parrt/dtreeviz for info on how to properly install dtreeviz*.
* ...

Most of the necessary Python packages can be installed via the PyPi packages index or Anaconda Python distribution.

```python
# Install additional Python requirements
pip install -r requirements.txt
```
Finally to install AMLBID itself along with required dependencies, run the following command:
```python
# Install additional Python requirements
pip install AMLBID
```
## Examples of use
A working example is deployed in: [AMLBID](https://colab.research.google.com/drive/1zpMdccwRsoWe8dmksp_awY5qBgkVwsHd?usp=sharing)

#### <ins>Mode `Recommender`</ins>:

Below is a minimal working example of the `Recommender`mode .

```python
from AMLBID.Recommender import AMLBID_Recommender
from AMLBID.Explainer import AMLBID_Explainer
from AMLBID.loader import *

#load dataset
Data,X_train,Y_train,X_test,Y_test=load_data("TestData.csv")

#Generate the optimal configuration according to a desired predictive metric
AMLBID=AMLBID_Recommender.recommend(Data, metric="Accuracy", mode="Recommender")
AMLBID.fit(X_train, Y_train)
print("obtained score:", AMLBID.score(X_test, Y_test))
```
The corresponding Python code of the recommended pipeline should be exported to the `Recommended_pipeline.py` file and look similar to the following:<br/>
*Note that the packages import code is generated automatically and dynamically according to the recommended ML pipeline.*

```python
import numpy as np
import pandas as pd
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import classification_report
from sklearn.model_selection import train_test_split

data = pd.read_csv("Evaluation/Dataset.csv")

X = data.drop('class', axis=1)
Y = data['class']

X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.3, random_state=42)

model= DecisionTreeClassifier(criterion='entropy', max_features=0.5672564318672457,
                       min_samples_leaf=5, min_samples_split=20)
                       
model.fit(X_train, Y_train)

Y_pred = model.predict(X_test)
score = model.score(X_test, Y_test)

print(classification_report(Y_test, Y_pred))
print(' Pipeline test accuracy:  %.3f' % score)
```
---

#### <ins>Mode `Recommender_Explainer`</ins>:

Below is a minimal working example of the `Recommender_Explainer` mode.
```python
from AMLBID.Recommender import AMLBID_Recommender
from AMLBID.Explainer import AMLBID_Explainer
from AMLBID.loader import *

#load dataset
Data,X_train,Y_train,X_test,Y_test=load_data("TestData.csv")

#Generate the optimal configurations according to a desired predictive metric
AMLBID,Config=AMLBID_Recommender.recommend(Data, metric="Accuracy", mode="Recommender_Explainer")
AMLBID.fit(X_train, Y_train)

#Generate the interactive explanatory dash
Explainer = AMLBID_Explainer.explain(AMLBID,Config, X_test, Y_test)
Explainer.run()
```
Demonstration of the explanatory artifact:

![https://github.com/LeMGarouani/AMLBID/blob/main/media/Demo.gif](https://github.com/LeMGarouani/AMLBID/blob/main/media/Demo.gif)
---
**AMLBID** was developed in the [LISIC Lab](https://www-lisic.univ-littoral.fr/) at the [ULCO University](https://www.univ-littoral.fr/) with funding from the [ULCO](https://www.univ-littoral.fr/), [HESTIM](https://www.hestim.ma/), and [CNRST](https://cnrst.ma/index.php/fr/).
