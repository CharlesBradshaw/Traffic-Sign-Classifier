# TrafficSignClassifier

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: datasetVisualization.png "Visualization"
[image2]: modifiedLeNet.jpeg 
[image3]: beforeHistEqual.jpg "Image Before Histogram"
[image4]: afterHistEqual.jpg "Image After Histogram"
[image5]: customImages.png "Manual Data Test"


#### 1. A basic summary of the data set

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is [32, 32]
* The number of unique classes/labels in the data set is 43

#### 2. Exploratory Visualization of the Dataset.

I wanted to get a feel for the visuals of the traffic sign data. I figured that being able to see a random sample of signs would help me have a better understanding of what my network is trying to accomplish. Below is an example of what is seen for every sign class.


![Sample Class Visuaization][image1]

### 3. Model Architecture

![Modified LeNet][image2]

In the end I used a network much like LeNet to classify the traffic signs. The end result was a three stage convolutional network with one fully connected layer. The fully connected layer took the output of the second and third convolutional layer as input. 

As for preprocessing, I decided to use grayscale, histogram equalization, and normalization. Grayscale was used because it allows for a smaller, and therefore faster network. Histogram Equalization was used on a hunch. I noticed that many of the test images were in very poor lighting conditions. Given that histogram equalization can improve the visibility of graysale images for humans, I used it as a tool to improve the visibility for my network. Finally I used normalization because it makes the data much more understandable for the network. 

The goal of histogram equalization, is to make the slope between the bars of a cumulative histogram as constant as possible. Below is an example from Wikipedia showing the effect of histogram equalization (Before on the left | After on the right)

![Histogram Equalization][image3] ![Histogram Equalization][image4]

Yet again, here is a sample class visualization, where you can see the pre-processing pipeline I use.

![Sample Class Visualization][image1]
Here is an example of a traffic sign image before and after grayscaling.

### 4. Final Model Architecture

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image  							|
| Pre-processing 		| Grayscale -> Equalized Histogram -> Normalized|		 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6 					|
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16	 				|
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 1x1x400 	|
| RELU					|												|
| Dropout 				| Combined 2nd + 3rd Layer, Keep Prob 50%		|
| Fully connected		| Input from 2nd and 3rd conv layer. 775->43    |
| Softmax				| 		    									|
|						|												|
 
A requirement of this project was to get the Validation Accuracy above 93%. I found that the LeNet architecture worked fine for the problem, but I wanted to explore something different. After doing some research I found [this paper] describing what modifications the creator of LeNet, Yann LeCun, made to make the network function better on traffic signs. Since it was a clear upgrade to the model I was using I decided to go with that model. I also attempted to work with color data, as converting to grayscale loses data. This ended up not working. Since color images have more data than grayscale, they need a bigger network to process that data. I ran into trouble finding a good size for a color image network. All of the ideas that I tried were either far to small or far too big. With a better idea of what it takes to create a good sized network I continued using the modified LeNet architecture. Once I had the architecture I played around with the hyper parameters, mainly trying to combat overfitting. In the end I used L2 normalization along with dropout. 

The reason for choosing a convolutional network, beyond "because they work", is that since data in images is spatially related, "scanning" over an image with a 5x5 "scanner" helps the network truly read the input image. 

### 5. Hyper Parameters

* Number of Epochs: 10 
* Batch Size: 128
* Learning Rate: 0.0013
* Beta: 0.04
* Fully Connected Dropout: 50%
* Mu : 0
* Sigma : 0.12 


The final model results were:
* training set accuracy of 99.84%
* validation set accuracy of 94.83%
* test set accuracy of 93.33%
 

### 6.Test the Model on New, Hand Picked Images

Udacity provided most of the training data, which I believe was simply an open source German traffic sign dataset. It's possible that this dataset doesn't represent the real world and therefore the accuracy of my network may be far lower than 93.33%. I found five different images of German traffic signs from the web, resized them and ran them through my network.

![Five Different German Traffic Signs and their Preprocessing][image5]

The first two images should be simple to detect. The third image is the first image to not take up the entire space, and the last two have two signs and a complex background.

### 7. Analysis of The model's predictions.

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| No Vehicles     		| No Vehicles 99.83% Confidence					| 
| Road Work  			| Road Work	100.00% Confidence					|
| Turn Right			| Turn Left	72.85% Confidence					|
| No Entry	      		| Bumpy Road 56% Confidence				 		|
| Bumpy Road			| Slippery Road 98% Confidence     				|
|						|												|


The model was able to correctly guess 2 of the 5 traffic signs, which gives an accuracy of 40%. Compared to the test set accuracy of 93%, this is horrible, but I'm not too worried. 

It's also important to note that when testing the network on these 5 images, for each image I had the network output it's top 5 guesses and their confidence. For all of the images that the network incorrectly classified, the correct sign was not among the top 5 guesses. 

As I mentioned, the last 3 images (the incorrectly classified ones) weren't centered and zoomed in on the sign in question, and in the last 2 there was a lot of non relevant data. Simply put, while convolution networks are very powerful, almost magical, they aren't perfect. Whatever data they're trained on is what they're going to be able to classify. It wasn't just shown images where the sign was somewhere in the image.It was shown images where the sign to be classified was near the center. And it was only given images with one sign in the image. This means that if I wanted to put this network on a car and expect the 93.33% test accuracy then I would have to really focus on creating a good "sign finding" algorithm. That said, the manual test also tells me that if I were to use this in the real world I would need to train it with a class output of "I don't know.". I wouldn't feel comfortable distributing a network to hundreds if not thousands of cars that claims a sign means "Turn Left" with 72.85% confidence when the sign actually meant "Turn Right".

It may seem like I'm just making excuses for an under preforming network, but given that the test accuracy was 93%, it's clear that the network can learn, it just needs the right data. Beyond just relying on an excellent sign finding algorithm an improvement would also be to augment the dataset to make signs non centered, tilted, blurred, and perspective warped. 

One last issue may be with how the data was collected. After looking through many of the images it seems that the data was collected from a video. This is very important because if the data was shuffled, and then split into training and testing data, then the network may have been shown a slightly modified version of the test data during training. For example, if the third frame of a video is used in training and the fourth frame of a video is used in testing, then the network may be seeing the same image, shifted slightly. This is important because convolutional networks are designed to be impartial to tiny changes in an images input.



