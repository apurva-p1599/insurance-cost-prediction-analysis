# insurance-cost-prediction-analysis

## Overview

This project aims to analyze insurance charges data and build predictive models to estimate the charges based on various factors. The project includes data visualization, hypothesis testing, and linear regression modeling. The analysis is divided into three parts:

1. **Data Reading and Visualization**
2. **Graphical Representation**
3. **Data Analysis**

## Project Structure

### Part 1: Data Reading and Visualization

- **Data Import**: Load the insurance dataset from a CSV file.
- **Data Exploration**: Display the structure, dimensions, summary statistics, and check for missing or duplicate values.

### Part 2: Graphical Representation

- **Histogram of Insurance Charges**: Visualize the distribution of insurance charges.
- **Boxplot of Charges by Smoker Status**: Compare insurance charges between smokers and non-smokers.
- **Overlay Histogram for Smokers vs. Non-Smokers**: Overlay density plots to compare the distributions of charges for smokers and non-smokers.

### Part 3: Data Analysis

- **Hypothesis Testing**: Perform t-tests to examine the effect of smoker status and sex on insurance charges.
- **Linear Regression**: Build and evaluate linear regression models using forward and backward stepwise selection.

## Installation

### Prerequisites

- R (version 4.3.2 or higher)
- Required R packages:
  - `ggplot2`
  - `MASS`
  - `MLmetrics`
  - `dplyr`
  - `caret`

### Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-repo/insurance-cost-prediction-analysis.git
   cd insurance-cost-prediction-analysis
   ```

2. **Install required packages**:
   ```R
   install.packages(c("ggplot2", "MASS", "MLmetrics", "dplyr", "caret"))
   ```
   
## Usage

### Data Reading and Visualization

1. **Load the dataset**:
   ```R
   ins <- read.csv("path_to/insurance.csv", header=TRUE, stringsAsFactors = FALSE)
   ```

2. **Explore the data**:
   ```R
   head(ins)
   str(ins)
   nrow(ins)
   ncol(ins)
   summary(ins)
   sum(is.na(ins))
   sum(duplicated(ins))
   sapply(ins, class)
   ```

### Graphical Representation

1. **Histogram of Insurance Charges**:
   ```R
   hist(ins$charges, 
        main = "Distribution of Insurance Charges", 
        xlab = "Insurance Charges", 
        ylab = "Frequency", 
        col = "lightblue", 
        border = "black")
   ```

2. **Boxplot of Charges by Smoker Status**:
   ```R
   boxplot(charges ~ smoker, data = ins,
           main = "Boxplot of Charges by Smoker Status",
           col = c("lightblue", "lightgreen"))
   ```

3. **Overlay Histogram for Smokers vs. Non-Smokers**:
   ```R
   ins$smoker <- as.factor(ins$smoker)
   ggplot(ins, aes(x = charges, fill = smoker)) +
     geom_histogram(aes(y = ..density..), position = 'identity', alpha = 0.5, bins = 30) +
     labs(title = 'Overlay Histogram of Charges for Smokers vs. Non-Smokers',
          x = 'Charges',
          y = 'Density') +
     scale_fill_manual(values = c('blue', 'red'))
   ```

### Data Analysis

1. **Hypothesis Testing**:
   - **Effect of Smoker on Charges**:
     ```R
     t.test(charges ~ smoker, data=ins)
     ```
   - **Effect of Sex on Charges**:
     ```R
     t.test(charges ~ sex, data=ins)
     ```

2. **Linear Regression**:
   - **Split Data**:
     ```R
     set.seed(123)
     split <- createDataPartition(ins$charges, p=0.7, list=FALSE)
     training <- ins[split,]
     testing <- ins[-split,]
     ```
   - **Forward Stepwise Selection**:
     ```R
     init <- lm(charges~1, data=training)
     full <- lm(charges ~ age + sex + bmi + children + smoker + region, data=training)
     forwardModel <- stepAIC(init, scope = formula(full), direction="forward", trace=FALSE)
     summary(forwardModel)
     pred_forward <- predict(forwardModel, newdata=testing)
     MAE(testing$charges, pred_forward)
     MSE(testing$charges, pred_forward)
     ```
   - **Backward Stepwise Selection**:
     ```R
     backwardModel <- stepAIC(init, scope = formula(full), direction="backward", trace=FALSE)
     summary(backwardModel)
     pred_backward <- predict(backwardModel, newdata=testing)
     MAE(testing$charges, pred_backward)
     MSE(testing$charges, pred_backward)
     ```

## Results

### Model Evaluation

- **Forward Stepwise Regression Model**:
  - Mean Absolute Error (MAE): 4350.412
  - Mean Squared Error (MSE): 40482847

- **Backward Stepwise Regression Model**:
  - Mean Absolute Error (MAE): 9329.824
  - Mean Squared Error (MSE): 136284224
