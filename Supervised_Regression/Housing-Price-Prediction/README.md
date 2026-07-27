# House Price Prediction (Linear Regression)

Predicting house prices using Linear Regression on a housing dataset with a mix of numeric and categorical features.

## What this covers

- Loading and exploring the housing dataset
- Exploratory Data Analysis (data.head, shape, info, describe, checking for null values)
- Checking the distribution of the furnishingstatus column and visualizing the distribution of house prices
- Encoding categorical columns (mainroad, guestroom, basement, hotwaterheating, airconditioning, prefarea, furnishingstatus) using one hot encoding
- Standardizing numeric columns (area, bedrooms, bathrooms, stories, parking) using StandardScaler
- Train-test split (80-20)
- Training a Linear Regression model
- Predicting on test data and evaluating using MAE, MSE, RMSE, and R2 Score
- Plotting Actual vs Predicted House Prices
- Taking custom user input and predicting the price of a new house

## Key challenges

Initially, I considered using label encoding for the categorical columns. After thinking it through, I realized these columns like mainroad, guestroom, basement, and furnishingstatus are not ordinal, there's no natural order between the categories like "yes" and "no", or between furnishing levels. Label encoding would have incorrectly implied a ranking between categories, so I switched to one hot encoding instead, which treats each category independently without implying order.

Another challenge was handling user input for prediction correctly. Since the model was trained on a specific set of columns in a specific order (after one hot encoding), I had to make sure new input followed the exact same structure. I used `pd.DataFrame` to convert the user's input into a structured format with proper feature names, since the model expects input in tabular format matching the training data. Then I used `new_house[x_train.columns]` to force the new input into the same column order as the training data. This was important because a mismatch in column order or missing columns (for example, if the user's input doesn't naturally produce every one hot encoded column) would cause a feature mismatch error during prediction.

## Results

- MAE: 970,043.40
- MSE: 1,754,318,687,330.67
- RMSE: 1,324,506.96
- R2 Score: 65.29%

## Tools used

- Python
- pandas
- numpy
- matplotlib
- scikit-learn (StandardScaler, train_test_split, LinearRegression, metrics)
- VS Code

## Files in this folder

Selva_Linear_Reggresion_HousingData.ipynb

## Output preview

![Actual vs Predicted House Prices](Actual(y_test)VsPredicted(y_pred).JPG)

### Custom prediction example

Input:

| Feature | Value |
|---|---|
| Area | 1200.0 |
| Bedrooms | 3 |
| Bathrooms | 4 |
| Stories | 2 |
| Parking | 1 |
| Mainroad | yes |
| Guestroom | no |
| Basement | yes |
| Hotwaterheating | yes |
| Airconditioning | yes |
| Prefarea | yes |
| Furnishingstatus | semi-furnished |

Predicted House Price: 8,928,361.77

## What I learned

- The difference between ordinal and nominal categorical data, and why label encoding is only appropriate for ordinal data
- Why one hot encoding is the right choice for non-ordinal categorical features
- - Why `drop_first=True` is used with one hot encoding, it removes one category column per feature (for example, dropping the "no" column and keeping only "yes") to avoid redundant columns. Since the dropped category can still be inferred (if "yes" is 0, it means "no"), this keeps the dataframe smaller and avoids feeding the model unnecessary duplicate information
- Why standardization should be applied to numeric features before training a linear model
- - Why `random_state=42` is used in `train_test_split`, it fixes the randomness in how the data gets split into training and testing sets, ensuring the same rows go into train and test every time the code is run. Without it, each run would produce a different split, leading to slightly different results each time
- How to correctly structure new input data using pd.DataFrame and column reindexing to match the training data's column order, avoiding feature mismatch errors
- How to interpret regression evaluation metrics like MAE, MSE, RMSE, and R2 Score

## Note

This is a hands-on practice exercise, not a deployed project. I haven't learned deployment yet, that's planned for later in my course.

## Dataset source

Sourced from a public dataset (housing.csv).
