# iLOVECLIM_HistoryMatching
Source code of [Surrogate modeling based History Matching for an Earth system model of intermediate complexity](https://www.climatechange.ai/papers/neurips2023/54), accepted paper and poster at NeurIps CCAI Workshop 2023.

# Installation
This work is in continuity with the repository [L96HistoryMatching](https://github.com/HRMES-MOPGA/L96HistoryMatching), so you need to set up the same environment. To install the Python and R environment, use conda command: 

```
conda env create -f environment.yml
```

For the rest of the setup, please read the instructions.md file. 

# Repo organisation

* The DATA folder contains the outputs from the Experiments notebooks.
* The OUT_DATA folder contains the inputs used for iLOVECLIM experiments.
* The Experiments folder contains the experiment notebooks, as well as their parameters.

To reproduce an experiment, start with the HM-TuningILC notebook. Indicate the name of the experiment you want to run, and start running.
If you want to create your own experiment, you can create a params.json file, on the model of the json in the Experiments/parameters/simulations folder. You will then need to run HM-TuningILC and Get-metrics alternatively starting with the former, as well as runnning iLOVECLIM. Don't forget to precise the name of the experiment you wanna run in the beginning of both notebooks (choose_exp variable). 
