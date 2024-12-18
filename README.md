## Things to do now (summary)

### pre-processing
- understand what breaks random forest (OK -> time features!)
- give a "sine encoding" of time of day as extra feature (works super well!)
- add lagged features (OK)

### Regression Model
- build basic CNN to try and regress all control vars into a "measurement type" vector (stat_coil_tmp, water_circ_flow, ph_current, ...)
- use cross-validation/dataloaders w/ CNN
- predict "probability of anomaly" from 0 to 1 for each sample based on normalised reconstruction error
- domain shift between training and test data: some form of domain adaptation is probably good. 

### Unsupervised Learning Model
- Train autoencoder on VG5 pump/turbine
- Compare distance (can use MSE) between bottleneck layers (when input is healthy vs anomaly). Threshold based on this like before (3 standard deviations away from mean)
- predict "probability of anomaly" for autoencoder for each sample by normalising reconstruction error.

### Isolation Forest
- Use bottleneck features as input to isolation forest
- Use PCA features as input to isolation forest
- pick whichever performs best

### Anomaly Score
- get output from all relevant models
- average it out (single vote for each model? weight by individual performance?)
- -> anomaly score for each timestamp
- if probability > some threshold (3 std), for some amount of time (like 2 timestamps) -> anomaly
- don't think about it too much. Pick some threshold that makes sense (eg 3 standard deviations away from mean) and voila.

### Root cause identification
- use autoencoder model -> find out which features are reconstructed most poorly (first root cause layer)
- feed reconstruction error into a random forest classifier to predict which synthetic anomaly type it is
  - get performance. OK if shit. At least we tried.

### Compare with manual thresholds
- compare performance of our anomaly score vs using thresholds from Alpiq
- have plot showing when threshold is triggered and anomaly score for certain sensor values + when we flag as anomaly, overlaid on some sensor plots

### Transfer between units (auxilary task, domain adaptation)
- Train model on VG5, test raw performance on VG6 and VG4 without domain adaptation
- numbers comparing raw performance on source and target domains
- try adversarial domain adaptation on autoencoder -> add discriminator to try and guess source and target domains
- with regression CNN -> also have adversarial domain adaptation between convolutional layers and prediction layers. 
- get results with domain adaptation

### file structure/organisation
- EDA -> random EDA file showing the EDA we did for report
- data_preprocessing 
- regression_models -> Random forest, CNN
- unsupervised_models -> Autoencoder, Isolation Forest
- anomaly_score -> get full output from models, combine into anomaly score.
- root_cause -> get full output from models, build classifier
- Alpiq_model -> build manual threshold model, compare with anomaly score
- domain_adaptation_models -> build regression CNN and Autoencoder again, but this time with adversarial domain adaptation.
- DA_anomaly_detection -> get full output from DA models, get new anomaly score. Get old anomaly score. Compare results on different datasets. 

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