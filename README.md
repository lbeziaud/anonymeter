# Anonymeter: Unified Framework for Quantifying Privacy Risk in Synthetic Data

`Anonymeter` is a unified statistical framework to jointly quantify different
types of privacy risks in synthetic tabular datasets. `Anonymeter` is equipped
with attack-based evaluations for the **Singling Out**, **Linkability**, and
**Inference** risks, which are the three key indicators of factual anonymization
according to the [Article 29 Working Party](https://ec.europa.eu/justice/article-29/documentation/opinion-recommendation/files/2014/wp216_en.pdf).

A simple explanation of how `Anonymeter` works is provided below. For more details, a throughout
description of the working of the framework and the attack algorithms can be found in the paper
[A Unified Framework for Quantifying Privacy Risk in Synthetic Data](https://arxiv.org/abs/2211.10459).
This work has been accepted at the 23rd Privacy Enhancing Technologies Symposium ([PETS 2023](https://petsymposium.org/cfp23.php)).


In `Anonymeter` each privacy risk is derived from a privacy attacker whose task is to use the synthetic dataset to come up with a set of *guesses* of the form:
- "there is only one person with attributes X, Y, and Z" (singling out)
- "records A and B belong to the same person" (linkability)
- "a person with attributes X and Y also have Z" (inference)

Each evaluation consists of running three different attacks:
- the "main" privacy attack, in which the attacker uses the synthetic data to guess information on records in the original data.
- the "control" privacy attack, in which the attacker uses the synthetic data to guess information on records in the control dataset.
- the "baseline" attack, which models a naive attacker who ignores the synthetic data and guess randomly.

Checking how many of these guesses are correct, the success rates of the different attacks are measured and used to derive an estimate of the privacy risk. In particular, the "control attack" is used to separate what the attacker learns from the *utility* of the synthetic data, and what is instead indication of privacy leaks. The "baseline attack" instead functions as a sanity check. The "main attack" attack should outperform random guessing in order for the results to be trusted.


## Setup and installation

Clone the Anonymeter repository:

```shell
git clone git@github.com:statice/anonymeter.git
```

Install  [Miniconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).
After installing Miniconda, create an environment. `Anonymeter` needs either python 3.8 or 3.9.
Here we will use the more recent version.

```shell
conda create -n anonymeter python=3.9
conda activate anonymeter
```

Once you have this basic environment set up, you can install this package
calling `pip install` from the `anonymeter` directory you have pulled from git.
Use the `-e` option to install in editable mode.

```shell
cd anonymeter  # if you are not there already
pip install -e '.[build,dev]'
```

## Basic usage pattern

For each of the three privacy risks anonymeter provide an `Evaluator` class. The high-level classes `SinglingOutEvaluator`, `LinkabilityEvaluator`, and `InferenceEvaluator` are the only thing that you need to import from `Anonymeter`.

Despite the different nature of the privacy risks they evaluate, these classes have the same interface and are used in the same way. To instantiate the evaluator you have to provide three dataframes: the original dataset `ori` which has been used to generate the synthetic data, the synthetic data `syn`, and a `control` dataset containing original records which have not been used to generate the synthetic data.

Another parameter common to all evaluators is the number of target records to attack (`n_attacks`). A higher number will reduce the statistical uncertainties on the results, at the expense of a longer computation time.

```python
evaluator = *Evaluator(ori: pd.DataFrame,
                       syn: pd.DataFrame,
                       control: pd.DataFrame,
                       n_attacks: int)
```

Once instantiated the evaluation pipeline is executed when calling the `evaluate`, and the resulting estimate of the risk can be accessed using the `risk()` method.

```python
evaluator.evaluate()
risk = evaluator.risk()
```

## Getting started

Check out the example notebook in the `notebooks` folder to start playing around
with `anonymeter`. To run this notebook you would need `jupyter`. This should
be installed as part of the `dev` dependencies. If you haven't done so, please
install jupyter by executing:

```shell
pip install jupyter
```

inside the `anonymeter` environment created for the installation.

##


## Cite this work

If you use anonymeter in your work, we would appreciate citations to the following paper:

"A Unified Framework for Quantifying Privacy Risk in Synthetic Data", M. Giomi *et al*, PoPETS 2023.

This `bibtex` entry can be used to refer to the arxiv preprint:

```text
@misc{https://doi.org/10.48550/arxiv.2211.10459,
  doi = {10.48550/ARXIV.2211.10459},
  url = {https://arxiv.org/abs/2211.10459},
  author = {Giomi, Matteo and Boenisch, Franziska and Wehmeyer, Christoph and Tasnádi, Borbála},
  title = {A Unified Framework for Quantifying Privacy Risk in Synthetic Data},
  publisher = {arXiv},
  year = {2022}
}
```
