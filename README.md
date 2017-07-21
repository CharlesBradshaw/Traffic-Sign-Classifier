# TrafficSignClassifier

To see results, open the Traffic_Sign_Classifier.ipynb

### Idea
In the late 1990's a [paper](http://yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf) was published describing how to use neural networks to recognize individual characters. This network was called LeNet. They created what is now known as a convolutional neural network. Essentially, this means they have a neural network scan over an image to make a new, typically deeper, image. For example, a neural network with a 5x5 input and a 1x6 output could scan over a 20x20 pixel image and have an output of 16x16x6. Then this new deeper image would be reduced, ie 16x16x6 -> 8x8x6, and run through another convolutional network. LeNet has 3 convolutional layers, and 3 fully connected (normal nn) layers. 

This network has been used by the United States Postal Service to recognize handwritten addresses and by banks to read checks. The next big update for convolutional neural networks came in 2012 when Alex Krizhevsky presented [AlexNet](https://www.nvidia.cn/content/tesla/pdf/machine-learning/imagenet-classification-with-deep-convolutional-nn.pdf) at the ImageNet Large Scale Visual Recognition Challenge, a competition where computers attempt to identify objects in an image. AlexNet was a modified version of LeNet, and had an accuracy of ~85 percent. Since then, as of 2016, convolutional network have been the only networks to win ImageNet.

### LeNet Architecture
<img src = "letnet.png">

### This Project

The goal of this project is to use the architecture from LeNet to classify traffic signs instead of characters. First I implemented LeNet in Tensorflow and trained it on MNIST data. From there I modified the input and output of LeNet to accept color images and output 43 classes (for signs) instead of 10 (for digits). Using color required modifying the structure of lenet so that it could learn three input filters, but also required not making the structure too large as the network can easily overfit.

Because of the difficulty in creating the perfect network size, and since lenet is optimized for grayscale, I returned to using grayscale. From there I made one update where I fed the output of the second convolitonal layer along with the output of the third convoltional layer to the fully connected layer. 

The data was preprocessed simply by making it grayscale and doing histogram equalization, although the network learns just fine without the equalization. To improve the network I could create more data by manipulating the pictures I was given.


Finally, after the network was trained I tested 5 different images I found that weren't in the dataset. While my model scored 93% on the test set, it only scored 40% on my validation set. I think this is because the data was gathered from video so the train, test, and validation set are likely very similar. That and I threw some curveballs at the model during my manual test. 