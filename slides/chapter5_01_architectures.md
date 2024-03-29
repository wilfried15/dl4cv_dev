---
type: slides
---

# Architectures of CNNs

---

# Motivation

- Vanishing gradient problem
- Optimal kernel size

Note: Up to the point, all necessary techniques to train a CNN were introduced. However, it is worthwhile to discuss different design decisions in the architecture of neural networks. We will discover architectures using different kernel sizes for discovering large as well as small representations of the objects as well as techniques against the vanishing gradient problem. Loosely speaking, networks that are very deep with 100 layers, the weight updates can get so small, because of the gradients that, they vanish. This holds especially true for layers near the input. Thus, the neurons in the top of the network (near the output) learn very fast, while the other neurons need a lot of time to adjust. As a consequence, a very deep network is not able to learn the object representation and consequently are not able to outperform a network with fewer layers, although in theory, it should have more capacity to learn. 

Moreover, the question arises what is the optimal kernel size and how to find it. 

In the following slides, we will discuss different architectures, their features, strenght and weaknessess.

Most of the architectures were tested in the ImageNet Challenge. In this challenge, the network needs to classify 1000 different objects (people, cars, ships, flowers, etc.) as accurately as possible. For training, there are 1000s of images per category. The dataset is a commonly used benchmark to test classification algorithms on its performance.

Keras includes the latest architectures, so that these can be used without a large implementation effort. Moreover, they can be incorprated and initalized with the weights from the ImageNet Challenge, so that transfer learning is possible. For a better understanding, this chapter will have a deep dive into the architectures and code those from scratch.

---

# AlexNet

<img src="vl4/alexnet.png" alt="This image is in /static" width="50%">

- by (Alex) Krizhevsky, Sutskever, Hinton, 2012
- Top-performance on 2012 ILSVRC 
- Uses large kernels 
- ~ 63 million parameters 

Source: Krizhevsky, Sutskever, & Hinton (2012)

img source: https://towardsdatascience.com/the-w3h-of-alexnet-vggnet-resnet-and-inception-7baaaecccc96

Note: AlexNet was the first winner of the 2012 ImageNet Challenge.
It was invented in 2012 by Krizhevsky, Sutskever, and Hinton and is known as one major milestone in computer vision. It was the first network trained on a large dataset and the first one using large filter sizes, which was not computational feasible before. The original model was split into two and trained on two GPUs because of memory restrictions (Krizhevsky, Sutskever, & Hinton, 2012). 

---

# VGG 

<img src="vl4/vgg16.png" alt="This image is in /static" width="50%">

Introduced by Simonyan and Zisserman 2014
- Winner of ILSVRC-2014 Challenge
- Replacement of large filters by many small filters
- "First" deep network structure
- ~ 138 million parameters

Source: Simonyan & Zisserman (2014)

Note: Visual Geometry Group (VGG) architecture was introduced by Simonyan and Zisserman in 2014, the winner of Imagenet 2013. As the complexity of the network grew, the training time was growing similarly. To reduce training time, the number of network parameters has to be reduced. Therefore, the authors introduced the concepts of stacking multiple convolutional layers with a smaller kernel together, providing the same effect as a convolutional layer with a large filter size.  However, the number of parameters is reduced compared to the larger convolution. Replacing an 7 by 7 convolution with C channels (7x7xC)=49C while 3 3 by 3 convolutions results in (3x3xC)x3=27C
Consequently, the number of parameters is reduced by 44 \%. The reduction of parameters increases when the filter size increases. The existing VGG architecture can still be improved by using Batch Normalization, or Dropout layers, as well as a global pooling instead of the flatten layer to increase learning stability and reducing the network parameters (Simonyan & Zisserman, 2014, Vasilev, 2019).

---

# VGG - Replacement of large filter sizes

<img src="vl4/vgg_block.png" alt="This image is in /static" width="40%">

img source: https://towardsdatascience.com/the-w3h-of-alexnet-vggnet-resnet-and-inception-7baaaecccc96

Note: In this image, it can be seen that applying two 3 by 3 kernels after each other leads to an effective kernel size of 5 by 5. The 5 by 5 input is reduced to one single number. 

Reducing parameters:
- 2 x Conv3 = Conv5
- 3 x Conv3 = Conv7
- 5 x Conv3 = Conv11


---

# Residual network
<img src="vl4/residual_block.png" alt="This image is in /static" width="40%">

- by He, Zhang, Ren, Sun, 2015
- Vanishing gradient problem with very deep networks
- Idea: Add identity shortcut connection or skip connection

Source: He, Zhang, Ren, & Sun (2016), Fung (2017)

Note: He, Zhang, Ren, and Sun in 2015 made the discovery that when training very deep networks of 50+ layers, the networks could not outperform networks with 20 layers. This is contradicting to the theory that network capacity increases prediction performance.
The phenomenon is called vanishing gradient problem. In large networks, the weight updates might get very small and thus insignificant because the gradients are multiplied in the backpropagation process. To overcome this hurdle, the authors introduced the identity mapping or skip connection. ResNet takes advantage of the multi-branch architecture. The basic idea is that after transforming the image by (multiple) convolutional operation, the original input of the convolution is added to the results. The advantage is that still the original scenario is captured in the result as well as the learned objects. However, this can just be achieved if the skip connected input, and the output of the convolution has the same size. Thus, layers with the padding "same" are used. For mapping dimensions, when decreasing the output size, a one by one convolutional layer is used (Fung, 2017, He, Zhang, Ren, & Sun 2016, Vasilev, 2019).

---

# Residual network versions

<img src="vl4/resnet_variants.png" alt="This image is in /static" width="70%">

Source: He, Zhang, Ren, & Sun (2016)

Note: Up to five new different versions of the residual blocks are used. 

---

# ResNet-34 

<img src="vl4/resnet_full.png" alt="This image is in /static" width="25%">

Source: He, Zhang, Ren, & Sun (2016)

Note: This image shows the Resnet 34 architecture. The first convolutional layer is using a stride of 2 with followed by a max-pooling.  After these, the residual blocks are following. Two convolutional blocks are following with  "f" number of filters. In the original implementation, the first convolution is followed by batch normalization, relu activation, the second convolution, and another batch normalization before it is added to the shortcut path. The shortcut path (arrow) is skipping these blocks to prevent the vanishing gradients and to keep some representation of the previous layer. After 3 residual blocks, the number of filters is increased. In each residual block, the number of filters is increased. In the first convolution in the block, convolutions with strides 2 are used to shrink the input dimensions. The strided convolution replaces max-pooling layers. To connect also the shortcut to the decreased input size, in the short a 1 by 1 convolution with the same strides is performed. Moreover, every convolution needs the padding with the parameter "same" so that the output size equals the input size. This is necessary for adding the results of the shortcut and the normal path. Otherwise, the tensors would different shapes and could not be added together.

For a large image click <a href="https://miro.medium.com/max/306/1*CEg9KOO0mwgTmttvv0pSzQ.png" target="blank"> here </a>


---

# Resnet Video explanation (1)

<html>
<iframe width="800" height="500" src="https://www.youtube.com/embed/ZILIbUvp5lk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</html>

Source: Deeplearning.ai (2017c)

---

# Resnet Video explanation (2)

<html>
<iframe width="800" height="500" src="https://www.youtube.com/embed/RYth6EbBUqM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</html>

Source: Deeplearning.ai (2017d)

Note: This is a good description of why Resnet works

---

# Inception

<img src="vl4/inception_block.jpg" alt="This image is in /static" width="60%">

- by Szegedy, Ioffe, Vanhoucke, Alemi, 2015
- different versions, latest: V4
- What is the right kernel size? Objects can have different sizes
- Apply multiple sizes on the same level

Source: Szegedy, Ioffe, Vanhoucke, Alemi (2015)

Note: The inception architecture was introduced in 2015 by Szegedy, Ioffe, Vanhoucke, Alemi (Google). It is the winner of the ImageNet 2014 and also uses multi-branches. It is challenging to know the right filter size that needs to be defined a priori to training. Distant objects in an image might be very small, while the same object in the near might be a central component of the image. The main concept of Inception is applying different filter sizes for the convolved images accounting for the different sized objects. Thus, the input is split up to different filters, mostly 1x1, 3x3, 5x5 convolution, and a 3 x 3 max pooling. The results of the different filters are concatenated. Again, as in ResNet, the padding "same" is used to keep the dimensions. The 1x1 convolution is used for downsampling. Otherwise, the output would quadruple from block to block. There are multiple versions of Inception, improving the architecture over time, introducing in v2 the idea of stacked convolutions (similar to VGG), in v3 factorized convolutions to speed up computation and in v4 residual blocks (similar to ResNet) (Szegedy, Ioffe, Vanhoucke, Alemi, 2015, Vasilev, 2019). Factorized convolutions are applying instead of an NxN convolution first an Nx1 and afterward a 1xN convolutional operation, increasing efficiency.

---

# Inception Video description (1)

<html>
<iframe width="800" height="500" src="https://www.youtube.com/embed/C86ZXvgpejM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</html>

Source: Deeplearning.ai (2017b)

---

# Inception Video description (2)
<html>
<iframe width="800" height="500" src="https://www.youtube.com/embed/KfV8CJh7hE0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</html>

Source: Deeplearning.ai (2017a)

---

<html>
<h3>References</h3>
<list>
    <li>Deeplearning.ai (2017a). Inception Network. Retrieved from: https://www.youtube.com/watch?v=KfV8CJh7hE0&feature=emb_title</li>
    <li>Deeplearning.ai (2017b). Inception Network Motvation: Retrieved from: https://www.youtube.com/watch?v=C86ZXvgpejM&feature=emb_title</li>
    <li>Deeplearning.ai (2017c). Resnets. Retrieved from: https://www.youtube.com/watch?v=ZILIbUvp5lk&feature=emb_title</li>
    <li> Deeplearning.ai (2017d). Why ResNets Work. Retrieved from: https://www.youtube.com/watch?v=RYth6EbBUqM&feature=emb_title </li>
    <li>Fung, V. (2017). An Overview of ResNet and its Variants. Retrieved from: https://towardsdatascience.com/an-overview-of-resnet-and-its-variants-5281e2f56035 . Last Access: 23.02.2020</li>
        <li>He, K., Zhang, X., Ren, S., & Sun, J. (2016). Deep residual learning for image recognition. In 
            Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 770-778).</li>
        <li>He, K., Zhang, X., Ren, S., & Sun, J. (2016). Identity mappings in deep residual networks. In European 
            conference on computer vision (pp. 630-645). Springer, Cham.</li>
        <li>Krizhevsky, A., Sutskever, I., & Hinton, G. E. (2012). Imagenet classification with deep convolutional 
            neural networks. In Advances in neural information processing systems (pp. 1097-1105).</li>
        <li>Raschka, S., & Mirjalili, V. (2019). Python Machine Learning: Machine Learning and Deep Learning with 
            Python, scikit-learn, and TensorFlow 2. Packt Publishing Ltd.</li>
</li>
        <li>Szegedy, C., Ioffe, S., Vanhoucke, V., & Alemi, A. A. (2017). Inception-v4, inception-resnet 
            and the impact of residual connections on learning. In Thirty-first AAAI conference on artificial 
            intelligence.</li>
        <li>Simonyan, K., & Zisserman, A. (2014). Very deep convolutional networks for large-scale image 
            recognition. arXiv preprint arXiv:1409.1556.</li>
        <li>Torrey, L., & Shavlik, J. (2010). Transfer learning. In Handbook of research on machine learning 
            applications and trends: algorithms, methods, and techniques (pp. 242-264). IGI Global.</li>
        <li>Vasilev, I. (2019). Advanced Deep Learning With Python: design and implement advanced next-generation 
            ai solutions using tensorflow and pytorch. S.l.: PACKT PUBLISHING LIMITED.</li>
        
</list>
</html>

---

# Let's do some coding ... 