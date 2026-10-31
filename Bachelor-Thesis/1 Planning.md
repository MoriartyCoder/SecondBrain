# Todos

- [x] Find seq with text
- [x] Basic data augmentation pipeline
- [x] Text removal? No
- [ ] Analyse scale variation
- [ ] Test augmentation for: histogram compression, 
- [ ] Consider and research secondary dataset with the same measurements as as secondary test-dataset (CST Anti-UAV)

## Papers to review
- [ ] [[AUGTARGET DATA AUGMENTATION FOR INFRARED SMALL TARGET DETECTION]]
* [ ] [[Dot Distance for Tiny Object Detection in Aerial Images]]
* [ ] [[Evaluating the Robustness of Anti-UAV Detection under  Controlled Fog Degradation]]
* [ ] [[Infrared Small-Target Detection Using Multiscale Gray Difference Weighted Image Entropy]]

---

# Steps
## Research
- [ ] Research methods for each of the categories: IR-sensors, Corner cases and SotA
- [ ] Research licence stuff for albumentation and Ultraliytics YOLO
- [ ] Finish outline for the thesis
## Pre-training
- [ ] Select YOLO model
- [ ] Select metrics for evaluation
- [ ] Select weights management strategy
- [ ] Find way to evaluate data easily
- [ ] Implement augmentation pipeline
- [ ] Implement data loader
- [ ] Implement the training pipeline
## Training
- [ ] Train a baseline model
## Evaluation

## Writing

---
 NETD
 Fixed Noise Pattern
 Common Mode Noise
 small Area + low constrast/low Signal-to-Clutter/Noise?-ratio
 Corner-cases identification
 Augmentation for corner-cases:
* Downscaling
* Shift of grey-value histogram 
	* Stauchung?
* 
 ---
# Title
Analysis (of the effect) of Augmentations on prediction performance for Artificial Neural Networks on IR datasets


# Intro
## Motivation
## Problem statement

## Research question(s)

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
What augmentations methods can be applied to a dataset to improve the performance of a ANN?
-> Excludes GAN!



## Thesis structure?
What happens in the chapters?

# State of the art
## Sources

## Approaches (as table, by whom?, criteria to compare)

## Research gap

# Requirements?

# Method
Verlauf des Experiments

# Augmentation

## Overall Methods

## Selected Methods
and why?

# Evaluation

## Metrics

## Models

### CNN vs Transformer

# Results/Discussion
What are the results. What went good and bad? How to interpret the result? Can it be evaluated?

# Conclusion
Looked at sources, requ, method, implem, eval(, results; worked )

# Future work
For the next one to suffer


-----
# To be sorted
* Selection of the dataset
* Selection of models
* Selection of evaluation methods



I have to write a work in the field of data augmentation for drone detection in the IR spectrum. 
I have the following subfields that are of interest: Effects of Ir-Sensors for augmentation like NEDT, Fixed Pattern Noise and Common Mode Noise, augmentation techniques for edge cases like low SNR and small objects (downscaling, shifing of greyvalue-histograms and stauchung von grauwert-histogrammen) and I am also looking for some state-of-the-art augmentation method from the literature.

These are some ideas and reference points. I want you to basis for these things and possible expand or/and provide if there are good ones improvements.

I need papers that have a good scientific base. This includes citations and clean work for the papers.