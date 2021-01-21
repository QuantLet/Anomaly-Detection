# Anomaly-Detection (AD)
Toolkit for Anomaly Detection


## Univariate Anomaly Detection
### offline method (code: Univariate_AD_offline.py)
1. Use rolling mean and std to build confidence interval (say, 95% CI), as a standard to detect potential anomalies. 
   Here's the illustration of the AD detection result, with 95% CI := [Mean- 1.96*SD, Mean+ 1.96*SD]
   ![Japan - TS plot detecting anomalies with windowsize 31 (center=True)](https://github.com/DreamBird-Jane/Anomaly-Detection/blob/main/Univariate%20Anomaly%20Detection/Japan%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center%3DTrue).png)
1. Use rolling median and MAD (Median-absolute-deviation) instead, to detect potential anomalies (more robust AD). 
   Here's the illustration of the robust AD detection result, with 95% CI := [Median- 3*MAD, Median+ 3*MAD]. 
   ![Japan - TS plot detecting robust anomalies with windowsize 31 (center=True)](https://github.com/DreamBird-Jane/Anomaly-Detection/blob/main/Univariate%20Anomaly%20Detection/Japan%20-%20TS%20plot%20detecting%20robust%20anomalies%20with%20windowsize%2031%20(center%3DTrue).png)


## Reference
1. [How to build robust anomaly detectors with machine learning](https://www.ericsson.com/en/blog/2020/4/anomaly-detection-with-machine-learning)
