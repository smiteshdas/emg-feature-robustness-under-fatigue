# Robustness Analysis of sEMG Features for Gesture Classification Under Muscle Fatigue

Surface electromyography (sEMG) based gesture recognition systems are typically trained under non-fatigued conditions, whereas real-world usage inevitably involves muscle fatigue. Fatigue alters muscle conduction velocity and spectral characteristics of EMG signals, potentially degrading model performance.

This project evaluates the robustness of commonly used time-domain and frequency-domain sEMG features for gesture classification under fatigue. A Support Vector Machine (SVM) is trained exclusively on non-fatigued data and evaluated on both non-fatigued and fatigued samples to quantify performance degradation.


## Dataset and Features

The dataset used for evaluation was taken from the paper [A Comprehensive Dataset of Surface Electromyography and Self-Perceived Fatigue Levels for Muscle Fatigue Analysis](https://www.mdpi.com/3093670) .

This dataset had 13 subjects performing 12 activities(gesture) with self-perceived fatigue levels [0,1,2].

![Gestures For this study](https://raw.githubusercontent.com/smiteshdas/emg-feature-robustness-under-fatigue/refs/heads/main/images/t1-t3-gestures-0-1_.png)

Out of 12 gestures , two gestures were taken for this study.

Gesture 0 in this study was the T1(Right Shoulder Flexion) of the paper's dataset.

Gesture 1 in this study was the T3 (Right Shoulder Extension) of the paper's dataset.


sEMG recordings were taken of four upper-limb muscles:

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

I built a comphrehensive CSV file with all features calculated (Window- 250ms, Overlap- 50%) from the raw paper dataset for conducting this study smoothly.

## Methodology

- Classifier: SVM with RBF kernel  
- Training: Non-fatigued data only  
- Testing:
  - On separate Non-fatigued data which was not used for training (baseline)
  - Fatigued data (robustness evaluation)
  - 
Feature ablation (using individual features to train,test and evaluate the model) was performed at the feature-type level (selected features included across all muscles).

Evaluation metrics:
- Accuracy  
- Weighted Recall  
- Weighted F1-score  

Performance Drop = (Metric on Fatigued Data) − (Metric on Non-Fatigued Data)

![accuracy-recall-f1-drops-sEMG](https://raw.githubusercontent.com/smiteshdas/emg-feature-robustness-under-fatigue/refs/heads/main/images/acc-recall-f1-drops.png)

As the Accuracy, Recall and F1 values are quite similar, hence only Accuracy was taken to measure performance drop

Smaller drop indicates greater robustness.


| Feature_Type     | NonFatigue_Acc    | Fatigue_Acc       | Accuracy_Drop       |
|------------------|-------------------|-------------------|---------------------|
| fVAR             | 92.36085849399782 | 89.60084687714055 | -2.7600116168572697 |
| fRMS             | 93.08839578028373 | 89.43271685659133 | -3.655678923692406  |
| fMAV             | 92.5427428155693  | 88.84426178466903 | -3.6984810309002683 |
| fIEMG            | 92.5427428155693  | 88.84426178466903 | -3.6984810309002683 |
| fWL              | 91.92433612222626 | 87.62376237623762 | -4.300573745988643  |
| fZC              | 89.8144779919971  | 84.77177906469893 | -5.0426989272981615 |
| fSSC             | 90.6147690069116  | 81.94781742325176 | -8.666951583659838  |
| fMPF             | 82.2117133503092  | 75.33470328164891 | -6.877010068660283  |
| fMF              | 77.11895234630775 | 74.09552275982315 | -3.023429586484596  |
| fSpectralEntropy | 77.91924336122227 | 71.1314527679183  | -6.787790593303967  |

## Key Findings

![Accuracy Percentage Plot corresponding to tested on fatigue and Non-Fatigue](https://raw.githubusercontent.com/smiteshdas/emg-feature-robustness-under-fatigue/refs/heads/main/images/accuracy_plot_fat_non_fat.png)

![Drops when tested across fatigue and non-fatigue data](https://raw.githubusercontent.com/smiteshdas/emg-feature-robustness-under-fatigue/refs/heads/main/images/fat-non_fat_drop_hist_plot.png)

Time-domain amplitude features (VAR, RMS, MAV, IEMG, WL) achieved high baseline(non-fatigue) accuracy with relatively small performance degradation under fatigue, indicating stronger robustness for gesture classification. Mean Frequency (MF) showed small degradation but lower overall classification accuracy. Slope Sign Changes (SSC) exhibited the highest performance drop. MPF and Spectral Entropy were also strongly affected by fatigue.

These results suggest that frequency-related features are more sensitive to fatigue-induced spectral shifts, whereas amplitude-based features remain more stable for gesture recognition tasks.

## Discussion

Muscle fatigue slows down muscle fiber conduction and shifts the EMG signal’s power toward lower frequencies. Because of this, frequency-based features like MF, MPF, and Spectral Entropy change significantly under fatigue. SSC is also affected since it depends on rapid signal fluctuations, which reduce as high-frequency components decrease.

On the other hand, amplitude-based features such as MAV, RMS, IEMG, VAR, and WL mainly measure signal strength rather than frequency content. This makes them more stable and robust for gesture classification when fatigue is present.


## Conclusion

Feature robustness is critical when designing EMG-based gesture recognition systems for prolonged use. Time-domain amplitude features provide stronger generalization under fatigue, while frequency-domain features are more sensitive to physiological changes and may be better suited for fatigue monitoring.


## Future Work
- Study on the shift in feature values as fatigue progresses
- Dedicated fatigue vs non-fatigue classification   

## References

- Cerqueira, S.M.; Vilas Boas, R.; Figueiredo, J.; Santos, C.P. A Comprehensive Dataset of Surface Electromyography and Self-Perceived Fatigue Levels for Muscle Fatigue Analysis. Sensors 2024, 24, 8081. https://doi.org/10.3390/s24248081
- Zhang, L. (2024). EMG-Toolbox Python Package (1.0.1). Zenodo. https://doi.org/10.5281/zenodo.13957415
- B. Hudgins, P. Parker and R. N. Scott, "A new strategy for multifunction myoelectric control," in IEEE Transactions on Biomedical Engineering, vol. 40, no. 1, pp. 82-94, Jan. 1993, doi: 10.1109/10.204774. https://ieeexplore.ieee.org/document/204774
- González-Izal M, Malanda A, Navarro-Amézqueta I, Gorostiaga EM, Mallor F, Ibañez J, Izquierdo M. EMG spectral indices and muscle power fatigue during dynamic contractions. J Electromyogr Kinesiol. 2010 Apr;20(2):233-40. doi: 10.1016/j.jelekin.2009.03.011. Epub 2009 Apr 29. PMID: 19406664. https://pubmed.ncbi.nlm.nih.gov/19406664/
- Viitasalo JH, Komi PV. Signal characteristics of EMG during fatigue. Eur J Appl Physiol Occup Physiol. 1977 Sep 16;37(2):111-21. doi: 10.1007/BF00421697. PMID: 902652. https://pubmed.ncbi.nlm.nih.gov/902652/
- Krogh-Lund C, Jørgensen K. Changes in conduction velocity, median frequency, and root mean square-amplitude of the electromyogram during 25% maximal voluntary contraction of the triceps brachii muscle, to limit of endurance. Eur J Appl Physiol Occup Physiol. 1991;63(1):60-9. doi: 10.1007/BF00760803. PMID: 1915335. https://pubmed.ncbi.nlm.nih.gov/1915335/

## Tools

[Python](https://www.python.org/), [NumPy](https://numpy.org/), [Pandas](https://pandas.pydata.org/), [Scikit-learn](https://scikit-learn.org/), [Matplotlib](https://matplotlib.org/) , [emg-toolbox](https://doi.org/10.5281/zenodo.13957415)
