# **EEG Signal Analysis and Classification of Multisensory Stimuli Using Machine Learning.**
---


## **Project Overview**
The goal of this project is to investigate whether different sensory stimuli produce distinguishable EEG patterns and whether these patterns can be classified using Machine Learning. The project also aims to learn the complete EEG data analysis workflow, including EEG preprocessing, ERP analysis, feature extraction, and Machine Learning model development using Python.
EEG preprocessing was performed using the MNE-Python library, followed by Event-Related Potential (ERP) analysis to compare the brain responses across three sensory modalities: visual pictorial (image), visual orthographic (text), and auditory (audio). Different EEG feature extraction methods were then applied to train a Machine Learning classifier for multisensory stimulus classification.
The dataset used in this project is the OpenNeuro EEG Semantic Imagination and Perception Dataset. EEG signals were recorded using a 124-channel ANT Neuro EEG system. The experiment consists of two tasks: semantic imagination and semantic perception, each presented in three modalities (image, text, and audio).
For this project, only the perception task was used to analyze the EEG responses to multisensory stimuli.


## **Methods**
The analysis was conducted entirely in Jupyter Notebook using Python and the MNE-Python library.
The project consisted of two major stages:
1. EEG Data Preprocessing (Single Subject)
2. Machine Learning Classification using the preprocessed EEG dataset


## **Data Analysis and Results**
### **1. EEG Data Preprocessing**
- **Import EEG Data**  
Subject used: sub-010_ses-01_task-experiment_run-01_eeg.set

- **Downsampling** 
The EEG data were downsampled from 1024 Hz to 250 Hz to reduce computational cost and memory usage while preserving the frequency range required for ERP and Machine Learning analysis.

- **Set Montage**  
The standard electrode montage was assigned using the built-in MNE montage system to correctly define the spatial locations of the EEG electrodes.

- **Detecting Bad Channels**  
The EEG recording contained several signal interruptions. Therefore, the recording was cropped, resulting in 283 events from the original 639 events.
Only perception events corresponding to the project objective were selected for further analysis.
Bad channels were identified based on: flat signals, excessive noise, correlation with neighbouring channels; a total of 9 bad channels were detected.

- **Independent Component Analysis (ICA)**  
ICA was applied to identify and remove eye-blink artifacts.
The following EOG channels were used to detect ocular artifacts: HEOGR, HEOGL, VEOGU, VEOGL. This improves the quality of the EEG signal before further analysis.

- **Filtering**  
A notch filter was applied to remove power-line noise, while a high-pass filter was used to remove slow baseline drift and low-frequency noise from the EEG signals.

- **Epoching**  
Continuous EEG signals were segmented into epochs around each stimulus presentation. (Only perception trials were retained.)
Number of epochs: Image->31, Text->30, Audio->29

- **ERP Analysis**  
ERP responses were visualized using several methods:
• Butterfly plots
• Topographic maps at selected ERP latencies
• ERP waveform comparison with topographic maps
• Global Field Power (GFP)
These visualizations were used to compare the temporal and spatial brain responses among the three sensory modalities.


### **Machine Learning Classification**
Three different EEG feature extraction methods were compared.  
***(1) PSD Full Spectrum Features***  
Power Spectral Density (PSD) was used to convert EEG signals from the time domain into the frequency domain.
Instead of using the raw EEG waveform, PSD measures the power of each frequency within every EEG epoch.
The PSD values from all frequencies and all channels were flattened into feature vectors, which were then used as the input features for Machine Learning.

***(2) Frequency Band Power Features***  
Instead of using every frequency individually, PSD features were grouped into the five conventional EEG frequency bands:
• Delta (1–4 Hz)
• Theta (4–8 Hz)
• Alpha (8–13 Hz)
• Beta (13–30 Hz)
• Gamma (30–40 Hz)
The average power within each frequency band was calculated for every EEG channel.
These averaged band powers were used as the Machine Learning features.

***(3) Time-Domain Features***  
Ten statistical features were extracted directly from the EEG waveform in the time domain: mean, variance ,standard deviation, minimum voltage, maximum voltage, peak-to-peak amplitude, Root Mean Square (RMS), skewness, kurtosis, Zero Crossing Rate (ZCR).
These features describe the statistical characteristics and signal properties of each EEG epoch without transforming the data into the frequency domain.

- **Support Vector Machine (SVM)**  
Support Vector Machine (SVM) was selected as the Machine Learning classifier because it performs well on high-dimensional datasets such as EEG signals and can effectively classify complex data using kernel functions.
Model performance was evaluated using test accuracy, cross-validation accuracy, classification reports, and confusion matrices.

- **Machine Learning Results**  
Three feature combinations were evaluated.

|       **Feature Set**      | **Test Accuracy** | 
| -------------------------- | ----------------- | 
| PSD                        |       56.67%      | 
| PSD + Domain               |       61.11%      | 
| PSD + Band Power + Domain  |       58.89%      |


## **Overall Learning Outcomes**
Through this project, I learned:
1. How to analyze EEG data in Python using the MNE-Python library.
2. The complete EEG preprocessing pipeline, including filtering, ICA, bad channel detection, epoching, and ERP analysis.
3. How different sensory stimuli produce different brain responses that can be visualized using ERP waveforms and topographic maps.
4. Different EEG feature extraction methods, including PSD, frequency band power, and time-domain statistical features.
5. How Machine Learning can be applied to EEG data for stimulus classification, including feature extraction, feature selection, model training, and model evaluation using SVM.


## **Dataset Reference:**
Wilson, H., Golbabaee, M., Proulx, M.J. et al. EEG-based BCI Dataset of Semantic Concepts for Imagination and Perception Tasks. Sci Data 10, 386 (2023). https://doi.org/10.1038/s41597-023-02287-9
