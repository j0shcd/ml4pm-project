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

## TA suggestions

- try to use simple models to check feature importance -> quick iteration using simple models (sugggested additive models)
- Timestamp info -> incorporate it by looking at data and finding some frequency that makes sense (maybe data is somewhat 24h periodic, or weekly periodic, etc.) -> feed this into a "sine encoding" with a certain frequency
- also can look if difference every weekend, or something and use boolean (isWeekend, etc)
- latent space probably better to compare similarity between healthy and anomaly
- use latent space features for anomaly detection algo
- commit to one or few model (eg. regression with CNN, autoencoder-based -> either residual based or latent features into anomaly detection algo like isolation forest)
- start writing report now
- explain that we did all the basic things (data cleaning, scaling, pre-processing, hyperparam tuning, EDA, ... josh's favorite slide)
  - approach by working on a single pipeline (one machine, one sensor target), finding what works and applying the pipeline elsewhere. preiodic sanity checks on other targets. 
  - data preproc -> naive baseline model -> 2 approaches : regression (show RF + CNN), unsupervised (autoencoder + anomaly detection), + really show all that we did, our assumptions, the potential pitfalls and what we could have done better, what we would do if we had more time, bullshit something for all points in pipeline. 
- for detecting root cause: look at what sensor has biggest reconstruction error (make sure everything is standardized). Maybe simple classifier just to show we tried (classification from reconstruction errors into synthetic anomaly type). 
- add lagged feature