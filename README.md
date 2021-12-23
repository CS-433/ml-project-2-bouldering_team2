# EPFL CS433 Machine Learning Porject 1 - Boulder

## Contributors
Project Advisor:
- Prof. Martin Jaggi (martin.jaggi@epfl.ch)

Project Students:  
- Julia Heiniger (julia.heiniger@epfl.ch)   
- Loïc Comeliau (loic.comeliau@epfl.ch)
- Weiran Wang (weiran.wang@epfl.ch)

## Project Description & Target
Competitive bouldering is a new, very popular sport. In 2021, the combined event of climbing was for the first time part of the Summer Olympic Games featuring bouldering together with lead and speed climbing. Nevertheless, there exist only a few studies about bouldering.  
This project belongs to [ML4Science](https://www.epfl.ch/labs/mlo/ml4science/), an interdisciplinary Machine Learning Project across EPFL campus, which is guided under Swiss  Bouldering Olympic Team and Prof. Martin Jaggi. During the project, we mainly focus on the understanding of bouldering problems from video, where we use the pose estimation as our feature and predict the solutions of boulder problems. Specifically, we first aim to predict the climber's name, and then we predict whether a climber will succeed to the top by analysing the climbers' recording videos. More information can be found in our report.

##  Dataset and Data Processing
The data mainly is mainly from [2021 International Federation of Sport Climbing (IFSC) World Cup Series](https://www.ifsc-climbing.org/index.php/world-competition/calendar).  
We manually selected videos which can be used for analysis and abandoned those low-quality videos such as some working staff walking in front of lens. Then, we input the key metrics data into a Google Sheet which will be used for analysis afterwards.  
[MediaPipe](https://google.github.io/mediapipe/) is used for human pose estimation from videos and output pose landmark statistics into .json files.  
[tsfresh](https://tsfresh.readthedocs.io/en/latest/) calculates a large number of time series characteristics (the so-called features), which is important for on classification tasks.  
[Discriminative Guided Warping](https://github.com/uchidalab/time_series_augmentation) is a time series data augmentation method which could help to generate a richer training set based on the output of tsfresh. We will then used the augmented dataset for the analysis.

## Python Scripts Description
Considering this is a team work, all the codes are written in Google Colab and therefore, they are stored in .ipynb notebook format. It is allowed by the Prof. Martin that we can upload the .ipynb scripts.

`Mediapipe.ipynb`  
This scripts used [MediaPipe](https://google.github.io/mediapipe/) to extract pose landmark data which will be used for analysis by [tsfresh](https://tsfresh.readthedocs.io/en/latest/). For reproducibility purpose, it is worth mentioning that the following 11 videos cannot work correctly on MediaPipe, so we exclude them from our dataset: c35.mp4, c150.mp4, c162.mp4, c322.mp4, c366.mp4, c376.mp4, c378.mp4, c216.mp4, c311.mp4, c313.mp4, c314.mp4.

`FullTimeseries.ipynb`  
This code uses the previously generated landmarks.json file as input for [tsfresh](https://tsfresh.readthedocs.io/en/latest/) calculation. It outputs time series features in .csv files. However, without data augmentation, the test accuracy is not good.

`1Frame.ipynb`  
This notebook predicts the climbers' names and achievements on videos whose duration is 1 frame to predict the climbers' names and achievements.

`150Frames.ipynb`  
The working mechanism of this notebook is similiar with **1Frames.ipynb** but uses 150Frames instead to predict the climbers' names and achievements. It also made a comparasion before and after data augmentation.  

`PoseNormalization.ipynb`  
Suggested by Prof. Martin, we normalize the data before putting them into training model. In this notebook, we also make a comparsion between data before augmentation and the data after augmentation by using [Discriminative Guided Warping](https://github.com/uchidalab/time_series_augmentation). It is obvious to see that test accuracy after data augmentation is improved. 

## Accuracies  
|Model|Climber accuracy|Achievement accuracy|
|:---:|:--------------:|:------------------:|
|Full Time series|0.343|0.803|
|1 Frame|0.473|0.571|
|150 Frames|0.44|0.583|
|150 Frames augmented|0.881|0.804|
|150 Frames normalized|0.146|0.58|
|150 Frames augmented & normalized| 0.839|0.781|

## Usage Specification
To reproduce this project code, our project in [Google Drive](https://drive.google.com/drive/folders/1bXuYPRGQAE4X9DNayvMkN0cKofJozSAB?usp=sharing) contains all the dataset. To run the code, it is recommended as followed:
1. Clone the notebooks from this github repo, and link the dataset in the notebooks.
2. Take a look on `Mediapipe.ipynb` which demonstrates how pose landmarks are extracted from the given videos.
3. `FullTimeseries.ipynb` illustrates how time series features are generated based on the pose landmarks.
4. `PoseNormalization.ipynb` shows how we normalize the landmark data.
5. There is no strong dependency order on `150Frames.ipynb` and `1Frame.ipynb`, so please feel free to take a look on these two notebooks.
