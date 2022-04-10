---
title: Predicting Property Sale Values in Ames, Iowa
subtitle: Using regression, random forests, time series, and neural networks
author: Rishi Goutam, Srikar Pamidi, James Goudreault
author-url: 'https://github.com/rishigoutam/ames-iowa'
date: 'April 12, 2022'
abstract: In this article, we show how we predicted property values and evaluated the Ames housing market
keywords: Ames Housing Project; NYC Data Science Academy
description:
return-url: '..'
return-text: '← Return home'
---

# Predicting Property Sale Values in Ames, Iowa

## Introduction


## The Dataset
![](https://hackmd.io/_uploads/BJHVDyamc.png)

![](https://hackmd.io/_uploads/B18cPJTXc.png)


## Feature Engineering
![](https://hackmd.io/_uploads/S1CrD1pm9.png)

![](https://hackmd.io/_uploads/S1hIv1aX9.png)

## Notable Findings
![](https://hackmd.io/_uploads/SkHOvJTm9.png)

![](https://hackmd.io/_uploads/BJGKPyTm9.png)

![](https://hackmd.io/_uploads/S1qoPJpmc.png)

![](https://hackmd.io/_uploads/ryrnPJam9.png)

![](https://hackmd.io/_uploads/ryhawk6Xq.png)

![](https://hackmd.io/_uploads/rJhAv1p79.png)


## Predictive Models

brief overview of models selected


### Model Scoring

| Model            | R2 Train | R2 Test  | RMSE     |
| --------         | -------- | -------- | -------- |
| Elastic-Net      | 0.933    | 0.922    | 0.046    |
| Random Forest    | 0.986    | 0.915    | 0.047    |
| Gradient Boosting| 0.994    | 0.927    | 0.043    |
| SVR              | 0.926    | 0.922    | 0.045    |
| Backprop         | 0.937    | 0.895    | 0.032    |
| SARIMA           | 0.528    | 0.077    |$14,167   |

how did backprop have a worse R2 test but lower RMSE? That's a red flag


### Effect of 1-unit change on mean sale price (and other insights)

| Change                         | Expected Cost | Sale Price Increase |
| ------------------------------ | ------------- | ------------------- |
| +1 sqft living area            | N/A           | +$57                |
| +1 sqft basement area          | N/A           | +$22                |
| +1 sqft finished outdoor space | N/A           | +$11                |
| Installing a fireplace         | $2-5K         | +$4,380             |
| Installing central air         | $3-15K        | +$23,599            |
| Proximity to rail line         | N/A           | -$7,675             |

## Conclusion

## Future Development

![](https://hackmd.io/_uploads/BJHvdyaQ5.png)

## Engineering Process

```mermaid
flowchart LR
    d_r[(Ames_Real_Estate\n_Data.csv)]-->geopy
    d_TNX[(TNX)]-->tnx
    d_shapefile[(ames_school_districts_sf)]-->districts
    
    subgraph geo [New Features <various R/python>]
        direction LR
        geopy[[get coordinates\n using geopy]]
        tnx[[get interest rate\n using quandle]]
        districts[[get school district\n using shapefile]]
    end
    
    geo-->d_e

    d_h[(Ames_Housing_Price\n_Data.csv)]-->clean
    
    subgraph clean [Data Cleaning <clean.ipynb>]
        direction TB
        dupe[[remove \nduplicates]]-->outlier
        outlier[[remove \noutliers]]-->fillna
        fillna[[fill \nempty values]]-->impute
        impute[[impute \nmissing values]]
    end
    
    clean-->d_c[(cleaned.csv)]
    
    subgraph encode [Feature Engineering <engineer.ipynb>]
        direction TB
        ordinate[[convert categorical \nfeatures to ordinal]]-->indicator
        indicator[[create indicator \nfeatures]]-->new
        new[[create derived \nfeatures]]
    end
    
    d_c-->encode-->d_e[(engineered.csv)]
    
    subgraph eda [EDA <eda.ipynb, eda.R>]
        direction TB
        histograms-.-
        boxplots
        scatterplots-.-
        regplots
    end
    
    subgraph lin_regression [Multiple Linear Regression <linreg.ipynb, linreg.R>]
        direction TB
        subgraph lin_features [Select Model Features]
            direction LR
            style lin_features stroke-dasharray: 5 5, fill: khaki
            cat_encoded--->dummify[[dummify]]
            numerical---|plus|cat_encoded[categorical]---|plus|boolean
        end
        
        lin_features--->
        linear_model[[create model]]--->
        validate--->|iterate|lin_features
        
        subgraph validate [Validate and Score]
            direction LR
            style validate stroke-dasharray: 5 5, fill:khaki
            stats_significance[feature significance: p-values]
            stats_cor[multicolinearity: vif]
            stats_score[score: R^2, RMSE, AIC/BIC]
        end
        
    end
    
    subgraph algo [Additional Models]
        direction LR
        decision_tree>Decision Tree]
        arima>SARIMA]
        svr>SVR]
        backprop>Neural Net]
        
        decision_tree.->random_forest>Random Forest]
        decision_tree.->boosted>Boosting]
    end
    
    d_c-->eda
    d_e-->lin_regression
    d_e-->decision_tree
    d_e-->arima
    d_e-->svr
    d_e-->backprop
```

