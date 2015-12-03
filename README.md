
  
#Topographic Factor Analysis

Ning Li, Department of Industrial & System Engineering, University of Washington   
Haoran Cai, Department of Statistics, University of Washington

##Introduction
Functional Magnetic Resonance Imaging (fMRI) data sets contain multiple 3D images, where each image reflects the activities of thousands of voxels. Instead of studying individual voxel, researchers generally tend to look at the functionality of brain regions, which is composed of a number of voxels.


Topographic Factor Analysis (TFA) is a technique first introduced in Manning, Jeremy R. et al [1] to  automatically discover the meaningful brain regions based on recorded images. This method is fully unsupervised, flexible and it provides simple spatial interpretation for the resulting brain regions. However, to our knowledge, there is no open-sourced implementation of this useful method. Therefore, in this one-day Brainhack event, we proposed to implement TFA method in Python.  

##Approach
TFA was inspired by Latent Dirichlet allocation model in the field of topic modeling. The key assumption of TFA is it assumes that fMRI images are generated by a linear combination of finite number of sources distributed throughout the brain. The goal of the model is to automatically learn both the features of these sources and the distribution of sources on each image.    
![alt text](https://github.com/ninginthecloud/Brainhack2015/blob/master/man/fig/rbf.png?raw=true  "radial basis functions. ")

TFA defines a joint distribution over the image (observed) and all unobserved (latent) variables which include center and width of each source (features of sources) and weights of each voxels contributed by different sources (the distribution of sources on each image).    
![alt text](https://github.com/ninginthecloud/Brainhack2015/blob/master/man/fig/joint.png?raw=true "joint distribution")   
Similar to other graphic models, optimizing this joint likelihood function is intractable. Thus, variational inference method is used to do approximate inference. It has been showed that the real objective function can be well approximated by Evidence Lower Bound (ELBO), which is relatively easier to optimize.
![alt text](https://github.com/ninginthecloud/Brainhack2015/blob/master/man/fig/elbo.png?raw=true "evidence lower bound (ELBO)")

##Results
In this event, we implemented TFA model along with Black Box variational inference [2] using Python and applied the model on fMRI data set by [3]. This data set contains data from 9 participants with 360 images when they were showed all 60 drawings with 6 epochs, where all 60 drawings were randomly ordered. Due to the time limit for this event, we only explored the first participant's collection data.


Even though we only have very limited time, we were able to implement all the core parts of TFA model proposed in PAPER CITATION HERE. however we only have time to run very limited number of iterations, as a result, the model may not be fully converged.  The trend of ELBO is shown as below.

![alt text](https://github.com/ninginthecloud/Brainhack2015/blob/master/results/ELBO_iteration.png?raw=true "ELBO value vs iteration times")


##Discussion
TFA is a flexible graphical model. Depending on different fMRI image datasets, we can vary our type of basis functions, number of sources etc. However, the optimization algorithm will remain the same. 

After this experiment, we found that the intialization of parameters is extremely important for a fast convergence of the complex TFA model. Randomly selected initialized parameters made the starting ELBO value vary from negative value to positive value, which increases the difficulty of the following learning process. As shown in the figure above, the trend of our approximate objective function does not significantly increase. In Manning's paper, a slightly efficient initialization method, named "hotspot" was proposed, it is worthwhile to add this to our implementation.

##Conclusion

TFA has the advantage of giving a simple spatial interpretation of brain image, and it also can offer researchers useful information for further brain functional analysis. Furthermore,  varational inference method is powerful and it shows the potential to scale the model to real big dataset. Our implementation covers all the core parts of TFA model as well as variational inference algorithm. It is all written in python and requires very minmum third-party package dependency. After some code optimization, it runs reasonably fast. Due to the natural of TFA model and our implementation, it can also be easily parallelized and deployed in any cloud environment.

##Reference:
[1] Manning, Jeremy R., et al. "Topographic factor analysis: a Bayesian model for inferring brain networks from neural data." PloS one 9.5 (2014).    
[2] Ranganath, Rajesh, Sean Gerrish, and David M. Blei. "Black box variational inference." arXiv preprint arXiv:1401.0118 (2013).    
[3] Mitchell, Tom M., et al. "Predicting human brain activity associated with the meanings of nouns." science 320.5880 (2008): 1191-1195.
