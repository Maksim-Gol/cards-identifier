
# Poker Cards Identifier

![result](/results/res_3.png)
## Overview

This project is a poker cards object identifier using [YOLOv5](https://github.com/ultralytics/yolov5). It was trained in Google Colab with the use of a dataset of 3500 poker cards images.

## Features

- Real-time poker card detection using YOLOv5
- Creation and preparation of data for the model
- Example of setting up and training a model in Google Colab

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Data Preparation](#data-preparation)
- [Model Training](#model-training)
- [Results](#results)

## Installation
### Clone the Repository

```sh
git clone https://github.com/yourusername/your-project.git
cd your-project
```
### Activate conda environment 
```sh
conda activate ./yolo_poker_env
```
#### Or, alternatievly, install requirements into environment of your choice
```sh
pip install -r requirements.txt
```

### On windows, open <code>detect.py</code> and add:
```py
import pathlib
pathlib.PosixPath = pathlib.WindowsPath
```
after
```py
from pathlib import Path
```
It has to do with differences in how file paths are handled between operating systems, as the weights were extracted from Google Colab environment running Linux.
## Usage

### Run the program
```sh
python3 detect.py --weights best.pt --source 0
```
- source 0 - choose webcam as an input 

## Data Preparation
### Base dataset
As a base dataset for this project I've used [this](https://universe.roboflow.com/proba-knra2/cards-idf0e/dataset/5) dataset from Roboflow.\
![initial_dataset](/results/initial_dataset.png)\
It contains 3207 images and result was decent at first.\
But the problem is that the deck of cards I own has cards that are absent in that dataset.

### Creating data for my unique cards

#### Making images
For the cards that are unique in my deck - Two Jokers and Ace of Spades - I've made my own photos with varied background(light and dark - for dark I was using my sweatshirt) and light conditions. Also I've added card back and box from the deck because why not.

#### I've made 304 photos:
80 images of Aces:\
	- 20 with light on table\
	- 20 without light on table\
	- 20 with light on sweatshirt\
	- 20 without light on sweatshirt\

160 images of jockers with two back of the card:\
	- 40 with light on table\
	- 40 without light on table\
	- 40 with light on sweatshirt\
	- 40 without light on sweatshirt\

64 images of box:\
	- 16 with light on table\
	- 16 without light on table\
	- 16 with light on sweatshirt\
	- 15 without light on sweatshirt\

#### Resizing images
Images were made in 1:1 ratio.\
For training purposes I've resized all of the images to 640x640 resolution with [IrfanView](https://www.irfanview.com/)

#### Labeling images
All of the labeling was done in [Label Studio](https://labelstud.io):
![Labeling Showcase](/results/labeling_showcase.png)

#### Uploading dataset
In the end I've uploaded [my own](https://app.roboflow.com/pokercardswithjokers/poker_cards_with_my_deck/2) dataset on roboflow, extended with images of my cards, resulting in 3510 images.
![my roboflow](/results/my_roboflow.png)

## Model Training
The training was done in [Google Colab](https://colab.research.google.com/), using their GPUs:
```sh
!python yolov5/train.py --img 320 --batch 16 --epochs 70 --data /content/Poker_Cards_With_My_Deck-2/data.yaml --weights /content/yolov5/yolov5s.pt --device 0
```
### Parameters
 - img 320 - size of images used for training. Initial size of images is 640x640, this option scales them.

 - batch 16 - the number of images model trains on in "one go"

 - epochs 70 - number of epochs - one epoch is one complete pass of data through the model.

 - data ... - path to .yaml config file of our dataset

 - weights - initial weights our model based upon(here yolov5 small is used - it is not particularly enhancing our model)

 - device 0 - specify to use GPU

### Model Metrics

Here we can look at our model performance on various metrics on train and validation data:
![metrics](/results/results.png)

- box_loss - error in predicting coordinates of bounding boxes
- obj_loss - error in predicting existence of an objeect in a bounding box
- cls_loss - error in identifying class of an object in a boundary box
- precision - out of all prediction our model made, what is the ratio of right ones
- recall - out of all objects that exist, what is the ratio of right predictions our model made
- mAP - mean Average Precision - a metric that combines precision and recall to get overall quality of a model
## Results
Here are the results obtained from webcam as a source:
![res1](/results/res_1.png)
![res2](/results/res_2.png)


