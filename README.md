# Anomaly-Detection (AD)
Toolkit for Anomaly Detection


## Univariate Anomaly Detection
### offline method (code: Univariate_AD_offline.py)
1. Use rolling mean and std to build confidence interval (say, 95% CI), as a standard to detect potential anomalies
1. Use rolling median and MAD (Median-absolute-deviation) instead, to detect potential anomalies (more robust AD)


## Reference
1. [How to build robust anomaly detectors with machine learning](https://www.ericsson.com/en/blog/2020/4/anomaly-detection-with-machine-learning)
