# Motion Compensated Pulse Rate Estimation
This repository contains a completed cap-stone project for the Udacity "Applying AI to Wearable Device Data" course, 
part of the AI for Healthcare Nanodegree program.  It has been reviewed by Udacity instructors and met project specifications.

**Introduction**  
Pulse rate estimation, also known as heart rate monitoring, is a common feature in wearable devices.  The ability to measure pulse rates outside clinical settings has allowed users to gain unprecedented access to data and information about their personal health.  Users can measure their heart rates throughout the day during various activities to quantify workout intensities and establish their normal heart rate based on large amounts of data.  

The Photoplethysmogram (PPG) sensor is built into smart watches to enable pulse rate estimation at the wrist.  
A short description of how a PPG functions:  
- The PPG sensor emits light that is absorbed by red blood cells. Light that is not absorbed is reflected back to the PPG sensor's photodetector where the intensity is measured.  
- The difference between emitted light and reflected light is correlated to the amount of present blood cells.  
- When the heart contracts, blood is released to the body and wrist, so more of the PPGs light is absorbed.  
- When the heart relaxes, blood returns to the heart from the body, so that there are less blood cells that absorb the light at the wrist.  
- The intensities of light absorption over time form the time series data.  

For this project, an algorithm was created to estimate pulse rates from PPG sensor data taken from a study of people running on a treadmill at changing speeds.  

**Utilizing Accelerometers to Reduce Motion-Related Signals**  
A source of noise for standalone wrist-worn PPG sensors is arm motion.  Swinging an arm back and forth affects how blood flows through the wrist, and this is adds complexity to PPG data.    

To reduce the arm motion noise from PPG data, this project uses data from accelerometers which are also frequently built into wearable devices.  3-Axis Accelerometers worn on the wrist can measure arm positions, which allows the determination of arm motion frequencies.  By compensating for arm motion frequencies in PPG data, a better pulse rate estimation is achieved.
The performance requirement for this pulse rate estimation algorithm is to meet a mean absolute error, at 90% availability of estimates, of less than 15 BPM compared to pulse rates computed from concurrent ECG measurements.

**Algorithm and Performance**

There are two Parts in this project:
- [Part 1: Pulse Rate Algorithm](https://github.com/ElliotY-ML/Heart_Rate_Estimation_PPG_Acc/tree/master/Part%20I%20Pulse%20Rate%20Algorithm)
- [Part 2: Clinical Application](https://github.com/ElliotY-ML/Heart_Rate_Estimation_PPG_Acc/tree/master/Part%20II%20Clinical%20Application)

**Part 1: Pulse Rate Algorithm**  

[`Part 1: Pulse Rate Algorithm/pulse_rate_EY_completed.ipynb`](https://github.com/ElliotY-ML/Heart_Rate_Estimation_PPG_Acc/blob/master/Part%20I%20Pulse%20Rate%20Algorithm/pulse_rate_EY_completed.ipynb) contains exploratory data analysis, pulse rate algorithm, algorithm performance, and discussion about this project.

This algorithm determines the first pulse rate estimate with the first 8 seconds of data and subsequent estimates occur every 2 seconds afterward with 6 seconds of overlap between adjacent data windows.  
It meets the project's requirement that the mean absolute error at 90% availability is less than 15 BPM on the test dataset.
Using the training dataset, the error is 11.96 BPM. With the test dataset, the error is 7.55 BPM.

For full discussion, please read the "Project Write-up" section.  


**Part 2: Clinical Application** 

[`Part 2: Clinical Application/clinical_app_EY_completed.ipynb`](https://github.com/ElliotY-ML/Heart_Rate_Estimation_PPG_Acc/blob/master/Part%20II%20Clinical%20Application/clinical_app_EY_completed.ipynb) contains an analysis of a heart rate data set from the Cardiac Arrhythmia Suppression Trial (CAST).  Resting heart rates of over 1,500 subjects are computed and visualized to understand heart rate trends for different age groups.  
![Heart Rate Trend](Part%20II%20Clinical%20Application/out/Resting_HR_by_Age_Groups.png)
**Figure 1.** Mean resting heart rates by age groups with 95% confidence intervals, computed from the Cardiac Arrhythmia Suppression Trial(CAST) dataset.

For full discussion, please read the "Clinical Conclusion" section.

### Datasets

**Part 1: Pulse Rate Algorithm**

The dataset used to train this algorithm is from Z. Zhang, Z. Pi, and B.Liu's paper "TROIKA: A General Framework for Heart Rate Monitoring Using Wrist-Type Photoplethysmographic Signals During Intensive Physical Exercise." In their study, 12 subjects aged 18 to 35 were monitored as each subject ran on a treadmill with changing speeds for 5 minutes in this interval:
> Rest(30s)-> 6 or 8km/h(1min) -> 12 or 15km/h(1min) -> 6 or 8km/h(1min) -> 12 or 15km/h(1min) -> Rest(30s)

Two-channel PPG signals, 3-axis accelerometer signals, and one-channel ECG signals were simultaneously recorded. All signals were sampled at 125Hz.

The training dataset is located in the `/data/datasets/troika/training_data` folder.  Raw data files are named in the format 'DATA_XX_TYPEXX'. Each file has 6 rows: Row1 ECG data, Rows 2 and 3 are the two channels of PPG; and Rows 4,5, and 6 are the x-, y-, and z-axis accelerometer measurements.  
Files containing ground truth pulse rates are named in the format 'REF_XX_TYPEXX'. The ground truth is calculated from the ECG data in the data files. Z.Zhang et al calculate the ground truth pulse rates with every 8-second window and overlap of 6-seconds.

**References**  
[1] Zhilin Zhang, Zhouyue Pi, Benyuan Liu, ‘‘TROIKA: A General Framework for Heart Rate Monitoring Using Wrist-Type Photoplethysmographic Signals During Intensive Physical Exercise,’’IEEE Trans. on Biomedical Engineering, vol. 62, no. 2, pp. 522-531, February 2015. [Link](https://arxiv.org/pdf/1409.5181.pdf)

**Part 2: Clinical Application**  

The dataset comes from the Cardiac Arrhythmia Suppression Trial (CAST) sponsored by the National Heart, Lung, and Blood Institute (NHLBI). It consists of 24 hours of heart rate data from ECGs monitoring people who have experienced a myocardial infarction within the past two years.[1]  
This data has been smoothed and resampled to more closely resemble PPG-derived pulse rate data from a wrist wearable.[2]  
The training dataset is contained in the `/data/datasets/crisdb/` folder.


**References**  
[1] Stein PK, Domitrovich PP, Kleiger RE, Schechtman KB, Rottman JN. "Clinical and Demographic Determinants of Heart Rate Variability in Patients Post Myocardial Infarction: Insights from the Cardiac Arrhythmia Suppression Trial (CAST)". Clin Cardiol 23(3):187-94; 2000 (Mar)  
[2] Goldberger AL, Amaral LAN, Glass L, Hausdorff JM, Ivanov PCh, Mark RG, Mietus JE, Moody GB, Peng C-K, Stanley HE. "PhysioBank, PhysioToolkit, and PhysioNet: Components of a New Research Resource for Complex Physiologic Signals" (2003). Circulation. 101(23):e215-e220. [Link](https://physionet.org/content/crisdb/1.0.0/)


## Getting Started

1. Set up your Anaconda environment.  
2. Clone `https://github.com/ElliotY-ML/Heart_Rate_Estimation_PPG_Acc.git` GitHub repo to your local machine.
3. Open `Part 1: Pulse Rate Algorithm/pulse_rate_EY_completed.ipynb` with Jupyter Notebook to explore EDA, signal processing, PPG and Accelerometer waveform analysis, and pulse rate estimation confidence calculation.
4. Open `Part 2: Clinical Application/clinical_app_EY_completed.ipynb` with Jupyter Notebook for an analysis of resting heart rates for Cardiac Arrhythmia Suppression Trial (CAST) dataset. 


### Dependencies
Using Anaconda consists of the following:

1. Install [`miniconda`](http://conda.pydata.org/miniconda.html) on your computer, by selecting the latest Python version for your operating system. If you already have `conda` or `miniconda` installed, you should be able to skip this step and move on to step 2.
2. Create and activate * a new `conda` [environment](http://conda.pydata.org/docs/using/envs.html).

\* Each time you wish to work on any exercises, activate your `conda` environment!

---

## 1. Installation

**Download** the latest version of `miniconda` that matches your system.

|        | Linux | Mac | Windows | 
|--------|-------|-----|---------|
| 64-bit | [64-bit (bash installer)][lin64] | [64-bit (bash installer)][mac64] | [64-bit (exe installer)][win64]
| 32-bit | [32-bit (bash installer)][lin32] |  | [32-bit (exe installer)][win32]

[win64]: https://repo.continuum.io/miniconda/Miniconda3-latest-Windows-x86_64.exe
[win32]: https://repo.continuum.io/miniconda/Miniconda3-latest-Windows-x86.exe
[mac64]: https://repo.continuum.io/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
[lin64]: https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
[lin32]: https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86.sh

**Install** [miniconda](https://docs.conda.io/en/latest/miniconda.html) on your machine. Detailed instructions:

- **Linux:** https://docs.conda.io/en/latest/miniconda.html#linux-installers
- **Mac:** https://docs.conda.io/en/latest/miniconda.html#macosx-installers
- **Windows:** https://docs.conda.io/en/latest/miniconda.html#windows-installers

## 2. Create and Activate the Environment

For Windows users, these following commands need to be executed from the **Anaconda prompt** as opposed to a Windows terminal window. For Mac, a normal terminal window will work. 

#### Git and version control
These instructions also assume you have `git` installed for working with GitHub from a terminal window, but if you do not, you can download that first with the command:
```
conda install git
```

**Create local environment**

1. Clone the repository, and navigate to the downloaded folder. This may take a minute or two to clone due to the included image data.
```
git clone https://github.com/ElliotY-ML/Heart_Rate_Estimation_PPG_Acc.git
cd Heart_Rate_Estimation_PPG_Acc
```

2. Create and activate a new environment, named `PPG` with Python 3.8. Be sure to run the command from the project root directory since the environment.yml and pkgs.txt files are there.  If prompted to proceed with the install `(Proceed [y]/n)` type y.

	- __Linux__ or __Mac__: 
	```
	conda create --name PPG --file pkgs.txt
	source activate PPG
	```
	- __Windows__: 
	```
	conda env create -f environment.yml
	conda activate PPG
	```
	
	At this point your command line should look something like: `(PPG) <User>:USER_DIR <user>$`. The `(PPG)` indicates that your environment has been activated.


## Repository Instructions

Please read the original Udacity project instructions in [`Udacity_Project_Instructions.md`](Udacity_Project_Instructions.md).

Additional instructions for each part of this project are provided in each corresponding folder's `README.md` file.

**Part 1: Pulse Rate Algorithm**

[`Part 1: Pulse Rate Algorithm/pulse_rate_EY_completed.ipynb`](https://github.com/ElliotY-ML/Heart_Rate_Estimation_PPG_Acc/blob/master/Part%20I%20Pulse%20Rate%20Algorithm/pulse_rate_EY_completed.ipynb) contains exploratory data analysis, pulse rate algorithm, algorithm performance, and discussion about this project.

Inputs: 
- .mat files containing input files contain PPG, accelerometer, and measurements. 
- .mat files containing ground truth pulse rates calculated from ECG measurements.

Outputs:
- Estimated Pulse Rate, Ground Truth Pulse Rate, Estimation Confidence, and Error.  
- Aggregate mean absolute error (BPM) at 90% availability of pulse rate estimates from this algorithm.


1. Open `/Part I Pulse Rate Algorithm/pulse_rate_EY_completed.ipynb` with Jupyter Notebook
2. The `RunPulseRateAlgorithm(data_fl,ref_fl)` function does the following:
	1. Load input datasets.
	2. Apply `scipy.signal` functions to Bandpass Filter sensor measurements for frequencies that are within plausible pulse rate frequencies [40 BPM, 240 BPM].
	3. Apply `matplotlib.pyplot.specgram` to discretize sensor measurements into time windows, and for each time window, compute characteristic frequencies and amplitudes.
	4. Rank PPG characteristic frequencies by amplitude and compare to accelerometer frequencies to compensate for arm motions.
	5. Select PPG characteristic frequency to be the pulse rate estimate for that time window.  Calculate confidence in the pulse rate estimate.
	6. Calculate the absolute difference between pulse rate estimate and ground truth pulse rate.
3. The `Evaluate()` top level function loads the entire Troika dataset, runs `RunPulseRateAlgorithm` on the loaded dataset, and computes the aggregate mean absolute error (BPM) at 90% availability.

**Part 2: Clinical Application**  
`/Part II Clinical Application/clinical_app_EY_completed.ipynb` contains an analysis of a heart rate data set from the Cardiac Arrhythmia Suppression Trial (CAST).  
Resting heart rates of over 1,500 subjects are computed and visualized to understand heart rate trends for different age groups. 

Inputs:
- `/data/datasets/crisdb/metadata.csv`
- 1,537 .npz files containing 24 hours of heart rate data from ECGs from subjects in  `/data/datasets/crisdb/` folder

Outputs:
- Plot of Resting Heart Rate by Age Groups
- Discussion of results in "Clinical Conclusion" section


1. Open `/Part II Clinical Application/clinical_app_EY_completed.ipynb` with Juypter Notebook
2. Load `metatdata.csv`
3. Follow the notebook for exploratory data analysis (EDA) of CAST dataset metadata.
4. Extract data from .npz files with `numpy.load` function.
5. The `AgeAndRHR(metadata, npzFileName)` function computes the resting heart rate from time-series input by finding the lowest 5th percentile value in the heart rate data.
6. A pandas DataFrame is created to store resting heart rates information, along with sex, and age group for all subjects in the dataset.
7. Plot "Resting Heart Rates by Age Groups" with `seaborn.lineplot`.
8. The "Clinical Conclusion" discusses observations from the "Resting Heart Rates by Age Groups" plot. 


## License

This project is licensed under the MIT License - see the [LICENSE.md](./LICENSE.md)
