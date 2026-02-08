# EMG Feature Robustness Under Fatigue

## Overview

Surface electromyography (sEMG) based gesture recognition systems are typically trained under non-fatigued conditions, whereas real-world usage inevitably involves muscle fatigue. Fatigue alters muscle conduction velocity and spectral characteristics of EMG signals, potentially degrading model performance.

This project evaluates the robustness of commonly used time-domain and frequency-domain sEMG features for gesture classification under fatigue. A Support Vector Machine (SVM) is trained exclusively on non-fatigued data and evaluated on both non-fatigued and fatigued samples to quantify performance degradation.


## Dataset and Features

sEMG recordings were obtained from four upper-limb muscles:

- Right Biceps Brachii  
- Right Deltoid Anterior  
- Right Deltoid Medius  
- Right Deltoid Posterior  

Each sample includes:
- `GESTURE`
- `SUBJECT`
- `FATIGUE_LEVEL` (0 = non-fatigue, 1/2 = fatigue)

Extracted features:

**Time-domain:** MAV, RMS, IEMG, VAR, WL, ZC, SSC  
**Frequency-domain:** MF, MPF, Spectral Entropy  

Feature ablation was performed at the feature-type level (selected features included across all muscles).



## Methodology

- Classifier: SVM with RBF kernel  
- Training: Non-fatigued data only  
- Testing:
  - Held-out non-fatigued data (baseline)
  - Fatigued data (robustness evaluation)

Evaluation metrics:
- Accuracy  
- Weighted Recall  
- Weighted F1-score  

Performance Drop = (Metric on Fatigued Data) − (Metric on Non-Fatigued Data)

Smaller drop indicates greater robustness.



## Key Findings

Time-domain amplitude features (VAR, RMS, MAV, IEMG, WL) achieved high baseline accuracy with relatively small performance degradation under fatigue, indicating stronger robustness for gesture classification.

Mean Frequency (MF) showed small degradation but lower overall classification accuracy.

Slope Sign Changes (SSC) exhibited the highest performance drop. MPF and Spectral Entropy were also strongly affected by fatigue.

These results suggest that frequency-related features are more sensitive to fatigue-induced spectral shifts, whereas amplitude-based features remain more stable for gesture recognition tasks.

## Discussion

Muscle fatigue is associated with reduced conduction velocity and compression of the EMG power spectrum toward lower frequencies. Frequency-dependent descriptors (MF, MPF, Spectral Entropy) are directly influenced by these spectral shifts. SSC, which depends on rapid slope transitions, is similarly affected by high-frequency attenuation.

In contrast, amplitude-based features (MAV, RMS, IEMG, VAR, WL) primarily capture signal magnitude rather than spectral distribution, which may explain their greater robustness in gesture classification under fatigue.

An important observation is that features exhibiting high sensitivity to fatigue are not necessarily optimal for fatigue-robust gesture recognition. Feature suitability is task-dependent.

## Conclusion

Feature robustness is critical when designing EMG-based gesture recognition systems for prolonged use. Time-domain amplitude features provide stronger generalization under fatigue, while frequency-domain features are more sensitive to physiological changes and may be better suited for fatigue monitoring.


## Future Work
- Dedicated fatigue vs non-fatigue classification   


## Tools

Python, NumPy, Pandas, Scikit-learn, Matplotlib
