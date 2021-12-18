# EPFL CS433 Machine Learning Porject 1 - Boulder

## Contributors
Project Advisor:
- Prof. Martin Jaggi (martin.jaggi@epfl.ch)

Project Students:  
- Julia Heiniger (julia.heiniger@epfl.ch)   
- Loïc Comeliau (loic.comeliau@epfl.ch)
- Weiran Wang (weiran.wang@epfl.ch)

## Project Description & Target
Competitive bouldering is a new, very popular sport. In 2021, the combined event of climbing wasfor the first time part of the Summer Olympic Games featuring bouldering together with lead and speed climbing. Nevertheless, there exist only a few studies about bouldering.  
This project focuses on the understanding of bouldering problems from video, where we use the pose estimation as our feature and predict the solutions of boulder problems. Specifically, we first aim to predict the climber's name based on his/her climbing style. We then predict whether a climber will succeed to the top by analysing the first 150 frames of his/her climbing video.

##  Dataset and Data Processing
The data mainly focuses on 2021 International Federation of Sport Climbing (IFSC) World Cup Series.  
We manually selected videos which can be used for analysis and abandoned those low-quality videos such as some working staff walking in front of a lens. Then, we input the key metrics data into a Google Sheet which will be used for analysis afterwards.  
[MediaPipe](https://google.github.io/mediapipe/) is used for human pose estimation from videos and output pose landmark statistics into .json files.  
[tsfresh](https://tsfresh.readthedocs.io/en/latest/) calculates a large number of time series characteristics (the so-called features), which is important for on classification tasks.  
[Discriminative Guided Warping](https://github.com/uchidalab/time_series_augmentation) is a time series data augmentation method which could help generate a richer training set based on the output of tsfresh. We will then used the augmented dataset for the analysis.

## Python Scripts Description
Considering this is a team work, all the codes are written in Google Colab and therefore, they are stored in .ipynb notebook format. It is allowed by the Prof. Martin that we can upload the .ipynb scripts.

`Mediapipe.ipynb`  
This scripts used [MediaPipe](https://google.github.io/mediapipe/) to extract pose landmark data which will be used for analysis by [tsfresh](https://tsfresh.readthedocs.io/en/latest/).

`tsfresh_150Frames.ipynb`  
This code is based on Mediapipe.ipynb and uses the landmarks.json file as input for [tsfresh](https://tsfresh.readthedocs.io/en/latest/) calculation. It outputs time series features in .csv files.

`dataAugmentation_climber` and `dataAugementation_success`  
These two scripts work similarly where they take the output from tsfresh_150Frames.ipynb and output the augmented data by using [Discriminative Guided Warping](https://github.com/uchidalab/time_series_augmentation)

`CV_150Frames.ipynb`  
We use random forest for the classification prediction, a meta estimator that fits a number of decision tree classifiers on various sub-sampels of dataset. Insides, we also use 10-fold Cross Validation for both climber name prediction and 10-fold Cross Validation.

## Accuarcy
| Target Name | Accurcay |  
|:-----------:|:--------:|
|Name Prediction|0.868|
|Success Predcition|0.854|