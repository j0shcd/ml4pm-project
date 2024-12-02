## Things to try to get better model

- get a way to compare between models using the F1-score
  - get a ROC curve if possible, and choose the threshold with the highest f1-score

- drop useless columns (maybe some are super correlated)
- trying to cluster according to datetime information
  - try to figure out a meaningful datetime representation
- add windowing/moving averages to the dataset
- try to somehow remove seasonality
- find outliers in the healthy data, understand if that's normal or not. -> may be causing issues 
- make sure standardization works
- try to use dataloaders, and cross validation
- get model to try and predict the reconstruction of all of the measurements
- need to group measurement types together? 
- hard-code temperature thresholds

## Next steps

- need to generate an anomaly score
  - combination of many models (ensemble learning), each getting a "vote"
- feature selection -> understand which variables are most important
- "Develop a model that automatically outputs the most probable cause of a given
anomaly."
  - can use some explicit thresholds (magnetic circuit temp, injector opening, etc)
- Auxilliary task: 
  - "Evaluate the results for the synthetic anomalies."
  - "Analyze whether the anomalies can be detected effectively in the real test data, and compare them with your previous model developed specifically for the target unit."
  - "Propose potential strategies to improve the transferability of the model. What if you had access to a limited amount of training data for the target unit?"