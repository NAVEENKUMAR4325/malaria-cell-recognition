# Deep Neural Network for Malaria Infected Cell Recognition

## AIM

To develop a deep neural network for Malaria infected cell recognition and to analyze the performance.

## Problem Statement and Dataset

Using data augmentation in the Convolutional Neural Network approach decreases the chances of overfitting. Thus, Malaria detection systems using deep learning proved to be faster than most of the traditional techniques. A Convolutional Neural Network was developed and trained to classify between the parasitized and uninfected smear blood cell images. The classical image features are extracted by CNN which can extract theimage features in three different categories – low-level, mid-level, and high-level features.

## Neural Network Model

![4](https://user-images.githubusercontent.com/75235032/194893277-ad87c464-e99e-41b7-bbb8-254d95eea483.png)

## DESIGN STEPS

### STEP 1:
Import tensorflow and preprocessing libraries

### STEP 2:
Read the dataset

### STEP 3:
Create an ImageDataGenerator to flow image data

### STEP 4:
Build the convolutional neural network model and train the model

### STEP 5:
Fit the model

### STEP 6:
Evaluate the model with the testing data

### STEP 7:
Fit the model

### STEP 8:
Plot the performance plot
Write your own steps

## PROGRAM

### Developed By : Naveen Kumar.E
### Register Number : 212222220029
```
import os
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from matplotlib.image import imread
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras import utils
from tensorflow.keras import models
from sklearn.metrics import classification_report,confusion_matrix
import tensorflow as tf
# to share the GPU resources for multiple sessions
from tensorflow.compat.v1.keras.backend import set_session
config = tf.compat.v1.ConfigProto()
config.gpu_options.allow_growth = True # dynamically grow the memory used on the GPU
config.log_device_placement = True # to log device placement (on which device the operation ran)
sess = tf.compat.v1.Session(config=config)
set_session(sess)
%matplotlib inline
# for college server
my_data_dir = '/home/ailab/hdd/dataset/cell_images'
os.listdir(my_data_dir)
test_path = my_data_dir+'/test/'
train_path = my_data_dir+'/train/'
os.listdir(train_path)
len(os.listdir(train_path+'/uninfected/'))
len(os.listdir(train_path+'/parasitized/'))
os.listdir(train_path+'/parasitized')[0]
para_img= imread(train_path+
                 '/parasitized/'+
                 os.listdir(train_path+'/parasitized')[0])
para_img.shape 
plt.imshow(para_img)
# Checking the image dimensions
dim1 = []
dim2 = []
for image_filename in os.listdir(test_path+'/uninfected'):
    img = imread(test_path+'/uninfected'+'/'+image_filename)
    d1,d2,colors = img.shape
    dim1.append(d1)
    dim2.append(d2)
sns.jointplot(x=dim1,y=dim2)
image_shape = (130,130,3)
image_gen = ImageDataGenerator(rotation_range=20, # rotate the image 20 degrees
                               width_shift_range=0.10, # Shift the pic width by a max of 5%
                               height_shift_range=0.10, # Shift the pic height by a max of 5%
                               rescale=1/255, # Rescale the image by normalzing it.
                               shear_range=0.1, # Shear means cutting away part of the image (max 10%)
                               zoom_range=0.1, # Zoom in by 10% max
                               horizontal_flip=True, # Allo horizontal flipping
                               fill_mode='nearest' # Fill in missing pixels with the nearest filled value
                              )
image_gen.flow_from_directory(train_path)
image_gen.flow_from_directory(test_path)
model = models.Sequential([
    layers.Input((130,130,3)),
    layers.Conv2D(32,kernel_size=3,activation="relu",padding="same"),
    layers.MaxPool2D((2,2)),
    layers.Conv2D(32,kernel_size=3,activation="relu"),
    layers.MaxPool2D((2,2)),
    layers.Conv2D(32,kernel_size=3,activation="relu"),
    layers.MaxPool2D((2,2)),
    layers.Flatten(),
    layers.Dense(32,activation="relu"),
    layers.Dense(1,activation="sigmoid")])
    
model.compile(loss="binary_crossentropy", metrics='accuracy',optimizer="adam")
model.summary()
train_image_gen = image_gen.flow_from_directory(train_path,
                                               target_size=image_shape[:2],
                                                color_mode='rgb',
                                               batch_size=16,
                                               class_mode='binary')
train_image_gen.batch_size
len(train_image_gen.classes)
train_image_gen.total_batches_seen
test_image_gen = image_gen.flow_from_directory(test_path,
                                               target_size=image_shape[:2],
                                               color_mode='rgb',
                                               batch_size=batch_size,
                                               class_mode='binary', shuffle = False)
train_image_gen.class_indices
results = model.fit(train_image_gen,epochs=5,validation_data=test_image_gen)
model.save('cell_model1.h5')
losses = pd.DataFrame(model.history.history)
losses.plot()
model.evaluate(test_image_gen)
pred_probabilities = model.predict(test_image_gen)
test_image_gen.classe
predictions = pred_probabilities > 0.5
print(classification_report(test_image_gen.classes,predictions))
confusion_matrix(test_image_gen.classes,predictions)
from tensorflow.keras.preprocessing import image
img = image.load_img('new.png')
img=tf.convert_to_tensor(np.asarray(img))
img=tf.image.resize(img,(130,130))
img=img.numpy()
type(img)
plt.imshow(img)
x_single_prediction = bool(model.predict(img.reshape(1,130,130,3))>0.6)
print(x_single_prediction)
if(x_single_prediction==1):
    print("Cell is UNINFECTED")
else:
    print("Cell is PARASITIZED")
```

## OUTPUT

### Training Loss, Validation Loss Vs Iteration Plot

![1](https://user-images.githubusercontent.com/75235032/194893423-37a8a9cc-cd3f-4ea0-93b7-649d75f2f967.png)

### Classification Report

![2](https://user-images.githubusercontent.com/75235032/194893482-3a909f6a-e47f-49e2-b9b2-91a32480d22b.png)

### Confusion Matrix

![3](https://user-images.githubusercontent.com/75235032/194893570-04638f47-ad27-412c-8682-ba577758c168.png)

### New Sample Data Prediction

![5](https://user-images.githubusercontent.com/75235032/194893665-f1696e44-be02-4e2c-ab65-edccc7054184.png)
![6](https://user-images.githubusercontent.com/75235032/194893674-91ab1b30-4f5f-4a21-b252-a2f898adb66c.png)

## RESULT

Thus, a deep neural network for Malaria infected cell recognized and analyzed the performance .
