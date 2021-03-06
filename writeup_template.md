# **Traffic Sign Recognition** 

## Writeup

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/visualization.jpg "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[image4]: ./examples/placeholder.png "Traffic Sign 1"
[image5]: ./examples/placeholder.png "Traffic Sign 2"
[image6]: ./examples/placeholder.png "Traffic Sign 3"
[image7]: ./examples/placeholder.png "Traffic Sign 4"
[image8]: ./examples/placeholder.png "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.


Here is an exploratory visualization of the data set. It is a bar chart showing how the data is distributed among classes for train, validation and test set.

* Sample Image 

![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/submission/dsvis-1.png)

* Train set 

![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/submission/dsvis-2.png)

* Validation set 

![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/submission/dsvis-3.png)

* Test set 

![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/submission/dsvis-4.png)

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I decided not to convert images to grayscale, because color is an important part in identifying traffic signals. 

I made the training set balanced because of huge class imbalance. Training set after balancing: 

![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/submission/balance.png)

As a last step, I normalized the image data because we want to improve the conditioning of data, making it zero mean and unit variance so that gradient descent converges faster


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x8 					|
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16 					|
| Fully connected		| 400 inputs, 120 outputs						|
| RELU					|												|
| Dropout				|  												|
| Fully connected		| 120 inputs, 84 outputs						|
| RELU					|												|
| Dropout				|  												|
| Fully connected		| 84 inputs, 43 outputs							|
| Softmax				| 43 inputs, 43 outputs							|


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an ADAM optimizer, with learning rate of 0.001, 20 epochs, and batch size of 50. Keep probability for dropout was kept to be 0.5 during training.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.998
* validation set accuracy of 0.963 
* test set accuracy of 0.942

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
	* I started out with the lenet architecture because it was hinted to be a good starting point. Also, it seemed like something that already worked on classification problems of size 32x32, so a good start
* What were some problems with the initial architecture?
	* This architecture was not performing very well on the validation set but well on training - probably overfitting, 
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
	* As mentioned the architecture was overfitting, adding dropout to the internal fully connected layers solved the problem. 
* Which parameters were tuned? How were they adjusted and why?
	* The networks performance seemed to be sensitive to the batch size, and learning rate. I found batch size of 50 to work well. Learning rate of 0.001 worked ok. Also after 20 epochs the network seemed to have converged. 
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?
	* Convolution layer was a good choice because it works well with images, and we needed the translation invariance here. Dropout layer helped fixing the overfitting.

 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web, I resized them to 32x32:

Stop Sign: 
![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/test_images/0.jpeg)

Road Work: 
![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/test_images/1.jpeg)

Right-of-way at the next intersection:
![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/test_images/2.jpeg)

Yield:
![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/test_images/3.jpeg)

No passing for vehicles over 3.5 metric tons: 
![alt text](https://github.com/nihalsid/CarND-Traffic-Sign-Classifier-Project/raw/master/test_images/4.jpeg)

The first image might be difficult to classify because the stop sign is not actually covering the whole area of the image like the training set. The second image is similar. The third one should be fairly easy to classify. The yield sign's background might be a problem. The last ones view angle is not head on - different from the training dataset.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Stop Sign      		| Priority road   									| 
| Road Work     		| Road Work		 								|
| Right-of-way at the next intersection       		| Right-of-way at the next intersection:					 				|
| Yield					| Yield											|
| No passing for vehicles over 3.5 metric tons			| No passing for vehicles over 3.5 metric tons      							|


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This compares favorably to the accuracy on the test set of 94%. 

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 11th cell of the Ipython notebook.

For the first image, the model is sure that this is a priority road sign (probability of 1), and the image contains a stop sign. This is unexpected because the signs actually look pretty different. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1         			| Priority road   								| 
| ~1e-15     				| No entry 										|
| ~1e-19					| Stop											|
| ~1e-27	      			| Traffic signals					 			|
| ~1e-29				    | Yield      									|


For the second image, the model correctly predicts correct label with high probability 

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.993         			| Road work	   								| 
| ~7e-3     				| Dangerous curve to the right 										|
| ~4e-7					| Bumpy road											|
| ~1e-7	      			| No passing for vehicles over 3.5 metric tons					 			|
| ~8e-8				    | General caution    									|

For the third image, the model correctly predicts correct label with high probability 

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.999        			| Right-of-way at the next intersection   								| 
| ~2e-4     				| Pedestrians 										|
| ~3e-5					| Beware of ice/snow											|
| ~1e-5	      			| Double curve					 			|
| ~4e-8				    |General caution    									|


For the fourth image, the model correctly predicts correct label with absolute certainty

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1       			| Yield   								| 
| ~1e-32     				| Speed limit (60km/h) 										|
| ~0					| Speed limit (20km/h)											|
| ~0	      			| Speed limit (30km/h)					 			|
| ~0				    |Speed limit (50km/h)   									|

For the fifth image, the model correctly predicts correct label with high probability

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.974       			| No passing for vehicles over 3.5 metric tons   								| 
| ~2.33e-2     				| Pedestrians 										|
| ~3.3e-3					| Speed limit (100km/h)											|
| ~8e-4	      			| End of no passing by vehicles over 3.5 metric tons				 			|
| ~4e-5				    |Dangerous curve to the right   									|


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?

From the featuremaps it seems that the network uses edge information mostly in the first convolution layer. But each learned filter is a different kind of edge. I couldnt make sense of the second layer activations.
