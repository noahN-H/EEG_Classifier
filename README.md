# EEG_Classifier
A Machine Learning (ML) program that processes EEG recordings (.edf format) and classifies simple brain states

Currently can differentiate between an "eyes open" and "eyes closed" state.

Datasets used from Subject S001, runs R01 & R02 - which are the sets that contain "eyes open" and "eyes closed". PhysioNet: https://physionet.org/content/eegmmidb/1.0.0/

# Pipeline overview:
- loads .edf via MNE
- raw data processed through bandpass filter (1-40hz)
- data is then segmented into 4s chunks
- data is then labelled
- the alpha or beta band power is then extracted
- features are scaled using StandardScaler
- data is then classified using logistic regression
- data is then validated via cross-validation (Pipeline + cross_val_score)

# Results:
  v1:
    - alpha power only
    - had power scaling issue which got solved in v2 and was then brought into scale v1 correctly
    - once scaled correctly it had an avg 0.9 across all folds
    - [0.7, 1., 1.] - avg 0.9
  v2:
    - alpha and beta
    - fixed power scaling so no issue to begin with
    - [1., 1., 1.] - avg 1.0    

# Limitations:
- all data is from a single subject
- small sample size of 30
- all "eyes open" chunks come from one single reading (R01) and all "eyes closed" come from a different single reading (R02). This means that the classifier may be using differences between the two recording sessions rather than the actual relevant data
- alpha/beta power averaged across 64 channels rather than keeping per-channel to avoid overfitting as there are only 30 examples - could be changed as examples are added

# Plans for the future:
- add more subjects
- add more classifications e.g. "left arm" movement, "right leg" movement, etc.
- add real time classification - have a live feed that is being processed and classified


# imports needed to run:
numpy
mne
math
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline



# Sources
Barry et al. 2007 (eyes-open/closed EEG differences): https://pubmed.ncbi.nlm.nih.gov/17911042/
Neurophysiology (eyes-open/closed EEG differences): https://link.springer.com/article/10.1007/s11062-018-9706-6
Scientific Reports (alpha-band activity, eyes open/closed): https://www.nature.com/articles/s41598-024-78173-0
PMC (occipital alpha-band activity): https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8786963/
Sapien Labs (EEG variability, eyes open/closed): https://sapienlabs.org/eyes-open-eyes-closed-and-variability-in-the-eeg/
PhysioNet (dataset): https://physionet.org/content/eegmmidb/1.0.0/
MNE-Python (docs): https://mne.tools/stable/
scikit-learn (docs): https://scikit-learn.org/stable/
