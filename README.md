# Multi Digit Detector for KTP ID Verification

## Project Overview
Numbers are everywhere around us. Develop OCR Model for KTP ID Verification
Now we are able to extend that to reading multiple digits as shown below. The underlying neural network does both digit localisation and digit detection. This can be quite useful in a variety of ML applications like reading labels in the store, license plates, advertisements etc.

![image](https://user-images.githubusercontent.com/25638454/233240154-56509ac2-c086-440d-b882-0c4bd798a3f0.png)

### Background
>Many users submitted their KTP ID Card with unqualified submission (Ex : Blank Image, Image other than KTP), and we cannot make sure that they already submit the same KTP ID Card with Data Information they submitted via App.

### Objectives
>Get their NIK number for each KTP ID Card match it with transactional data
 
### Scope
>- Image and Transactional Data
>- Data Period : 12 April 2022 - 23 August 2022

## Digit Detection Pipeline
The digit detection problem can be divided into 2 parts

1. Digits Localization
2. Digits Identification

### Digits Localization

An image can contain digits in any position and for the digits to be detected we need to first find the regions which contain those digits. The digits can have different sizes and backgrounds.

There are multiple ways to detect location of digits. We can utilize simple image morphological operations like binarization , erosion , dilations to extract digit regions in the images. However these can become too specific to images due to the presence of tuning parameters like threshold , kernel sizes etc. We can also use complex unsupervised feature detectors, deep models etc.

### Digits Identification

The localized digit regions serve as inputs for the digit identification process. MNIST dataset is the canonical data set for handwritten digit identification. Most data scientists have experimented with this data set. It contains around 60,000 handwritten digits for training and 10,000 for testing. Some examples look like :

![image](https://user-images.githubusercontent.com/25638454/233241902-72a63d62-a4f9-485d-b049-a29eb7f4704d.png)

However, the digits in real life scenarios are generally very different. They are of different colours and generally printed like the below cases.

![image](https://user-images.githubusercontent.com/25638454/233242050-49ebb8f0-e1c7-4658-bfe9-8b10a385df10.png)

![image](https://user-images.githubusercontent.com/25638454/233242076-d32c7ede-41e4-434c-9bd6-979d72ebfb23.png)

So we need to use dedicated dataset for this projects that we need and annotated it. The data set has a variety of digit combinations and match with KTP backgrounds and will work better for a generalized model.

## Modelling

Digit Localization is done using Maximally Stable Extremal Regions (MSER) method which serves as a stable feature detector. MSER is mainly used for blob detection within images. The blobs are continuous sets of pixels whose outer boundary pixel intensities are higher (by a given threshold) than the inner boundary pixel intensities. Such regions are said to be maximally stable if they do not change much over a varying amount of intensities. MSER has a lighter run-time complexity of O(nlog(log(n))) where n is the total number of pixels on the image. The algorithm is also robust to blur and scale. This makes it a good candidate for extraction of text / digits.

Digit recognition is done using a CNN with convolution, maxpool and FC layers that classify each detected region into 10 different digits. The classifier gets to 95% accuracy on the test set. We tested the dataset on a variety of examples and found that it works quite well. See examples shared above. There were some gaps where either the localizer didn’t work perfectly (digit 1’s location not detected) or the detector failed ( $ detected as 5).

![image](https://user-images.githubusercontent.com/25638454/233242639-4bc23f01-5616-49db-9d86-1ad65a8fe2b6.png)

THANK YOU !
