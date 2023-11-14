# Lung-Cancer-Detection

## Abstract

This dataset of lung cancer was collected over a period of 3 months in fall 2019 by the Iraq-Oncology teaching hospital/national center for cancer (you can find the data here: https://data.mendeley.com/datasets/bhmdr45bh2/1).
It consists of CT scans of either patients who are diagnosed with lung cancer in 2 different stages (benign, and malignant) or healthy ones. Those CT scans are originally saved in DICOM format (Digital Imaging and Communication).
Each scan contains several slices that show an image of human chest from different sides and angles. All cases vary in gender, age, area of residence and living status.

Using Computer Vision to Preprocess the data and Random Forest for Classification.


## Table of Contents

- [Used Libraries](#Libraries)
- [Data Visualization](#Visualization)
- [Preprocessing](#Preprocessing)
- [Features Extraction](#Features_Extraction)
- [Training and Results](#Training_and_Results)


## Libraries

Here are the imports of the used libraries
![libs](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/b9e56bd5-babd-4e61-a842-34d1e029152d)


## Visualization

Each scan contains several slices that show an image of human chest from different sides and angles. Here are some random samples of the 3 different classes in the data:

![v1](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/988f02e3-7cc7-4aae-bdfb-2758c49f9a92)
![v2](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/f8c6c738-44de-4a7d-b2c2-fe90af3f264f)
![v3](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/d9fe8205-4699-4151-8fd7-4f97e67780b7)

The dataset contains 1097 samples labeled with one of 3 classes (benign, malignant, normal) and distributed as follows: 120 benign, 561 malignant and 416 normal. We will split the data later into 90% train with 5 folds cross validation and 10% test while keeping the same classes distribution in all (stratify).

![dist](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/f766ab8c-e3d4-40f5-bfec-eb05dcec2deb)

We see that the Bengin class have much lower samples than the other 2 classes. which can cause a problem of biasing away from it if not dealt with properly.


## Preprocessing

Preprocessing is essential for our work to ensure the best representation of the key points in the data and to get the best possible accuracy. This is achieved by applying several processes on the original images data.We start by applying Gaussian blur on images to smooth them and eliminate noise. Next, Thresholding is applied, which in our case is keeping only pixel values between 120 and 255 to remove unnecessary objects and isolate the lungs from the background. Then, Edge Detection filters are applied to extract lungs borders and its infra structure outlines. Then, Dilation is applied to enlarge and thicken the boundaries obtained from the previous process. and finally, we resize all images to be in 256*256 resolution as a standard.

![preprocessing](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/4d9c378e-6cf3-4167-853c-796553aacaff)


## Features_Extraction

Here we get the most important features of the images to train our model on. We start by normalizing the images pixel values to be from (0 to 255), Then we get the GLCM (Gray Level Co-occurrence Matrix) of each image. The GLCM explains the spatial relationship between two adjacent pixels in an image as a function of the gray level. It computed by counting the frequency of occurrence of pairs of pixel values with a certain distance and orientation as follows:

![eq](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/220934f4-107a-45f2-adb9-5f65df2899ef)

The GLCM is used to extract various texture features from images by applying various statistical measures to the matrix. It is widely used in image analysis, especially in the biomedical field. The next step is to extract image features from the matrix. The features extracted are as given below:

![feat ext equations](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/40436b8d-27c1-49cd-a6b7-26b03288909b)
![eq2](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/53873649-85bd-4ef3-9ef4-540d5642243e)


and here's the code for the previous steps:

![feat ext code](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/21c7e7bb-a7bd-4c09-a28c-3e53ca953c49)


## Training_and_Results

After extraction the most important features of the data, we fit those features into different classification models and tested its prediction accuracy and precision on them. We choose the SVM, KNN and Random Forest algorithms to test and fit our data into them. Then the best model algorithm was then chosen based on the best accuracy and fastest prediction time.

![model results](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/34c5c01d-647e-42d7-98bb-700bb09b885d)

Weighted Random Forest was more suitable for the imbalance in our data that was obvious in the distribution part as the Random Forest classifier tends normally to the majority class. We give each class a weight, with the minority class receiving a heavier weight (A higher cost of misclassification) as shown in this equation:

![eq3](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/20d6666b-bc9b-4e28-a497-04d2dca4e603)

and here are the final metric scores for our model:

![final model](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/986f60b3-7aef-46d1-8168-a10cdad3ee49)

and here is a comparison of the confusion matrices for our final model and knn model:

![fff](https://github.com/Mo-Moustafa/Lung-Cancer-Detection/assets/81877648/bdd34673-50c3-4b76-b063-a1957285c22a)
