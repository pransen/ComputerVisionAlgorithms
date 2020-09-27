# Dimensionality Reduction using Principal Component Analysis

------

### Introduction

Often features in existing data are highly correlated. Some of them are quite evident from the data whereas few of them have a hidden correlation which can be found out by careful observation of the data. E.g. let us say we have a structured data where one of the features is speed of a vehicle in miles per hour and another being in kilometers per hour. Definitely, both these features capture the same information i.e., speed and one can be easily derived from the other. In other words, either of the two columns can safely be discarded. 

Dimensionality reduction techniques are widely employed to reduce the input feature dimensions before training a Machine Learning algorithm. Once such widely used technique is the Principal Component Analysis.

------

### Principal Component Analysis

Let us say that we have a 2-dimensional data as shown in the plot below:

![image-20200927124053167](images\image-20200927124053167.png)

One can clearly see a linear dependence between the features x1 and x2. In other words, this data is highly correlated. 

What Principal Component Analysis does is to find and project the data into another uncorrelated lower dimensional feature space where the variance (information) in the original data is retained to the maximum extent possible. Let us visualize how.

Let us say the after applying PCA, the original data is projected onto a space represented by X' as shown in the figure below:

![image-20200927125218986](images\image-20200927125218986.png)

One can easily see that only a small fraction of the variance in the original data is captured when projected to the feature space X'. This implies that much of the original information is lost when projected to X'. 

Now let us consider a direction perpendicular to X' as shown in the figure below.

![image-20200927130335675](images\image-20200927130335675.png)



Clearly a much higher variance in the input data is retained when projected to X''. 

In nutshell, Principal Component Analysis (PCA) will find the new feature space (X', X''), project the input data onto the new feature space and then discard the dimension which captures the least variance in the input data. Since the unit vectors along X' and X'' are perpendicular to each other, the new feature space is un-correlated. 

In general, **Principal Component Analysis** works by finding out the correlation between features in a n-dimensional feature space band then projecting the features to a d-dimensional orthogonal feature space, where d < n. Note that the term orthogonal is important as it ensures that the features in the d-dimensional feature space is uncorrelated

------

### MNIST Dataset

MNIST Dataset consists of 60000 images of hand written digits from 0-9. It is split into a trianing set consisting of 50000 images and a test set consisting of 10000 images. Each image is a grayscale image with a dimension of 28 x 28.

![](images\mnist.png)



------

### Features from MNIST and PCA

As discussed above, MNIST images are 28x28 which gives rise to a 784-dimensional feature space of intensity values between 0-255 when unrolled into a column vector. 

Let us now use PCA from the scikit-learn library to lower the dimension such that 99% of the input data variance is captured.
`***pca = PCA(0.99, svd_solver='auto')`***

Using this, the input 784-dimensional data is projected onto a much lower 540-dimensional feature space which still is able to retain 99% of the input data variance.

------

### Training a Fully Connected Neural Network

A fully connected neural network is used for the training. The input layer is of size 540 owing to the 540-dimensional feature vector after applying PCA. The output layer consists of 10 neurons corresponding to the digits from 0-9. ReLU activation function is used for the non-linearity. A learning rate of 0.0001 and Adam's optimizer is used for the training. The network is trained for 200 epochs.

A training accuracy of 99.84% and a test accuracy of 97.21% was obtained. 
The accuracy is pretty good considering the fact that using CNNs, which are more powerful and better suited for images, an accuracy of 99.79% has been obtained on the MNIST dataset.
Also remember that with the much sophisticated HOG features, a training accuracy of 99.96% and a test accuracy of 98.64% was obtained. (Link in references below)

Total number of images from the test dataset that were incorrectly classified were 279 out of 10000.

Few of the incorrectly classified images are as shown below

![download](images\download.png)

### Conclusion

Even though PCA is rarely used in Computer Vision as there are much better techniques like autoencoders for dimensionality reduction, through this post I have tried to give an intuition of how powerful PCA is. 

PCA is a tool of choice for dimensionality reduction in structured data. It is to be noted that the data has to be normalized to zero mean and unit variance before applying PCA. 

------

### References

- https://en.wikipedia.org/wiki/Principal_component_analysis
- https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html
- https://medium.com/@prantiksen4/training-a-neural-network-with-histogram-of-oriented-gradients-hog-features-373b97be5971?source=friends_link&sk=a5a4005255ed67d00145fef7a4c381d0