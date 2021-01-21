# Anomaly-Detection (AD)
Toolkit for Anomaly Detection


## Univariate Anomaly Detection
### offline method (code: Univariate_AD_offline.py)
1. Use rolling mean and std to build confidence interval (say, 95% CI), as a standard to detect potential anomalies
   ![Japan - TS plot detecting anomalies with windowsize 31 (center=True)](https://github.com/DreamBird-Jane/Anomaly-Detection/blob/main/Univariate%20Anomaly%20Detection/Japan%20-%20TS%20plot%20detecting%20anomalies%20with%20windowsize%2031%20(center%3DTrue).png)
1. Use rolling median and MAD (Median-absolute-deviation) instead, to detect potential anomalies (more robust AD)


## Reference
1. [How to build robust anomaly detectors with machine learning](https://www.ericsson.com/en/blog/2020/4/anomaly-detection-with-machine-learning)
