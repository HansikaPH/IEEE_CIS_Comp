# IEEE_CIS_Comp
This repository contains the experiments we performed for IEEE CIS Technical Challenge 2020 to compute the monthly energy consumption of 3248 homes.

# Instructions for Execution
1. ToDo: add ARIMA model script.
2. ToDo: add catboost residuals script.
3. Run "./median_ensemble/global_model_experiments.R" script. Make sure to change the "BASE_DIR" variable to your project directory before running the script. This script first runs 4 global models: Pooled Regression, Random Forest, Multilayer Perceptron Neural Network and CatBoost (with lags). The computed sub-model forecasts will be stored into "./results/sub-models" folder. Then, using the computed sub-model forecasts, it runs the median ensemble model where the medians of the sub-model forecasts are chosen as the resultant forecasts. The resultant forecasts will be stored into "results" folder with the name, "median_ensemble_forecasts.csv". This implementation requires 5 R packages: `tidyverse`, `nnet`, `catboost`, `glmnet` and `randomForest`. This script automatically install these packages, if your R environment does not include them. If you have already installed these packages, then you can comment lines 2-6.
4. Run "./gmean_ensemble/run_gmean_ensemble.sh" bash script from the project base directory. This script will first install all the required R packages including `stringr`, `dplyr`, `data.table`, `fpp3` and `expectreg`. It is assumed that the `glmnet` package is installed in the environment from the previous step. 
Then, it will run all the sub models including the Lasso Regression on CatBoost residuals, Base Lasso Regression models and the Expectile Regression models with quantiles 0.57 and 0.39. The daily level forecasts will be saved to the
directory "./results/sub-models/daily_forecasts/". Then the script will aggregate the daily forecasts to monthly level
and store them in the folder "./results/sub-models/". Next, the monthly Lasso Regression results on CatBoost residuals
will be added back to the original Catboost results to get the final Lasso Regression + CatBoost result. Finally, the script
will perform the geomean ensembling, replace the existing 0's and save the result in directory "./results/" under the name geomean_ensemble_forecasts.csv.   
5. Run "./final_ensemble.R" script. Make sure to change the "BASE_DIR" variable to your project directory before running the script. It computes the weighted average of median ensemble forecasts and geometric mean ensemble forecasts which is considered as our final submission. The final submission will be stored into the "results" folder with the name, "final_forecasts.csv".

