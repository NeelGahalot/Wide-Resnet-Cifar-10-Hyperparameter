# ECE-GY 7123 Deep Learning Mini-Project 1: wide-resnet-cifar10

## Introduction
Residual Network makes it possible to train up to hundreds or even thousands of layers and still achieves compelling performance and as a CNN, it has seen incredible proliferation over the years. Its powerful representational ability has produced seminal results in Computer Vision applications. Residual Neural Network (ResNet) is an Artificial Neural Network, which makes use of skip connections to jump over layers. A ResNet may contain N residual layers, and each residual layer may contain more than one residual block. A residual block contains two convolutional layers with skip connection from input to output. Formal definitions have been mentioned in the problem statement itself. All experimentation is done CIFAR-10 dataset, which is a collection of 60,000 32x32 color images in 10 different classes. The 10 different classes represent airplanes, cars, birds, cats, deer, dogs, frogs, horses, ships, and trucks. There are 6,000 images of each class. The goal is to maximize test accuracy on CIFAR 10 dataset, used as a benchmark for Computer Vision applications.

Choice of hyper-parameters for Neural Networks is crucial for obtaining best training results and this report summarizes all experimentations done to tune these parameters for highest accuracy. The cap for trainable parameters is set at 5 million with flexibility in choosing different data augmentation strategies. The model can be trained with any optimizer and regularization scheme of choice.

## Methodology
ResNet can employ a very large number of layers to train over a dataset and various techniques (like well-defined initialization strategies) have been proposed for deeper networks [1]. We set out to train our first model using 110 total layers (3 residual layers with 18 residual blocks each, each residual block has 2 convolutional layers). Batch size for all our models is 128. But as demonstrated in the work of Zagoruyko et al. [2], a deeper architecture may not the best implementation and can run problems like vanishing gradient descent.

Also, a deeper architecture takes much longer to run and makes computation more expensive. Cutting down on the number of layers for our next model, we use a total of 26 layers (4 residual layers, with 3 residual blocks in each, each block with 2 convolutional layers). A wider network presents a much better accuracy with runs faster. To further improve upon our wide network, we use a dropout layer, which can demonstrably yield consistent, even state of the art results. 

To further improve our results, we made use of two widely used data augmentation (techniques used to increase the amount of data by adding slightly modified copies of already existing data or newly created synthetic data from existing data techniques), namely random horizontal flip (horizontal flip is better than vertical flip) [3] and random crop. We do it from image padded by 4 pixels on each side and we use reflections of original image to fill missing pixels. With these two changes (dropout layer and data augmentation), we see significant improvement in our accuracy, with test accuracy reaching 92%. 

We tried using other augmentation techniques as well, one of which is rotation (90 degree). However, rotation didn’t optimize our model and accuracy was reduced. We, then, tried using transformation techniques, and experimented with RandomAffine (Random affine transformation of the image keeping center invariant) and ColorJitter (Randomly change the brightness, contrast, and saturation of an image). 

Adam was the optimizer used in all previously mentioned models. We tried using Adadelta optimizer but with worse results. Our next choice was Stochastic Gradient Descent optimized with weight decay of 0.0001 and momentum of 0.9 and starting learning rate 0.1, as suggested in [5], and performance did not enhance over Adam. Using Adam along with SGD as a Meta optimizer has been proposed [6], but its application was too convoluted for this project.

Going forward with our model, we widen our model (widening factor 4) and increase the number of parameters while keeping the limit capped at 5 million. We have a total of 10 layers, 4 residual layers with one residual block, each block with 2 convolutional layers and a dropout. This model of ours has the best accuracy so far. To increase the number of parameters further, we set the widening factor to 2 and the number of layers to 32. We changed the parameters for normalization, but the results did not match our expectations and did not better our 10-layer 4-widening factor model. 

With our 10-layer, 4-widening factor, we try different epoch cycles – starting with 40 epochs, all the way till 200 epochs. Accuracy as measured with different epoch cycles is shown in the results section.
Our final architecture, thus, has a total of 10 layers, consisting of 4 residual layers of 1 block each, each block has 2 convolutional layers with batch normalization and ReLU activation, and with a dropout layer between them. After 4 residual layers, the output goes through average pooling and a fully connected layer before the final result.
This is the implementation for the above architecture. The code is built upon the basic [Deep ResNet code](https://github.com/akamaster/pytorch_resnet_cifar10) authored by Yerlan Idelbayev.

## Conclusion
We conclude that Wide Residual Networks offer more efficient training and higher test accuracy than Deep ResNets on the CIFAR-10 dataset. Adding a Dropout layer helps in reducing the train and validation error, but not to a significant effect. We also conclude that various data augmentation techniques such as Horizontal Flipping, Cropping and Color Jittering helps in reducing overfitting and hence reduces the validation error. Finally, using the Adam optimizer with Wide ResNets gives better results than using Stochastic Gradient Descent.

## Submitted by:
- Kunal Kashyap (kk4564)
- Mukta Maheshwari (mm11070)
- Neelanchal Gehlot (ng2436)
