# Room Connection and Door Position Predicting Models

This notebook contains two models:
- The first model predicts the probability that two rooms are connected
- The second model predicts the position of the door between the two rooms

Together these models can predict if two rooms are connected and where the door connecting them would be.

### Room Connection Random Forest Model
- **Model Type:** Random forest classification
- **Data/Input:** All possible room connections of rooms in the same apartment, across many apartments. Relevant information such as room type is included. Extracted using a Cypher query.
- **Hyperparameter Tuning:** GridSearch CV sufficient
- **Label Predicted:** Probably that two rooms are connected or not. 0.5 threshold, 0 - not connected, 1 - connected
- **Results:** Testing accuracy = 95.7%

### Door Position XGBoost Model
The second model is XGBoost regression model. A dataframe containing information on all rooms and their door positions and other relevant information is extracted using a Cypher query. This is the training data for the second model. The model is trained and outputs a prediction of the door position given two connected rooms. The validation R² value is 0.99 when trained on all features or when trained on only the 5 most important features.

- **Model Type:** XGBoost regression
- **Data/Input:** All doors and information about the 2 rooms they are connecting. Relevant information such as room areas and positions are included. Extracted using a Cypher query.
- **Hyperparameter Tuning:** GridSearch CV is sufficient
- **Label Predicted:** Door position (x,y)
- **Results:** R² value = 0.992 when trained on all features and 0.994 when trained on only the 5 most important features

#### Feature Importance
The majority of the features added during feature engineering and data preprocessing are not relatively important to the model. Through observing the feature importances, the most important features are location information of the rooms and the difference in area of the two rooms. All other features have little to no impact on the model and can be removed.

#### Dataset Size Requirements
XGBoost is powerful in its ability to preform well on small datasets. The number of training datapoints needed to reach a R² value of 0.5 is 19 datapoints. 57 are needed to reach 0.75, 133 for 0.9, and 1163 for 0.99.
