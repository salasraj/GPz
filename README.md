
# GPz 2.0
Heteroscedastic Gaussian processes for uncertain and incomplete data

## Introduction
   This is a matlab implementation for building a regression model using Gaussian process (GP), class of Bayesian non-parametric models, with heteroscedastic noise. A dataset <img src="http://www.sciweavers.org/tex2img.php?eq=%20D%3D%5Cbig%5C%7BX%2CY%5Cbig%5C%7D%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt=" D=\big\{X,Y\big\} " width="86" height="21" /> is considered which consists of input <img src="http://www.sciweavers.org/tex2img.php?eq=X%3D%5Cbegin%7BBmatrix%7Dx_%7Bi%7D%5Cend%7BBmatrix%7D_%7Bi%3D1%7D%5E%7Bn%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="X=\begin{Bmatrix}x_{i}\end{Bmatrix}_{i=1}^{n}" width="92" height="29" /> <img src="http://www.sciweavers.org/tex2img.php?eq=%20%5Cin%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt=" \in " width="14" height="12" /> <img src="http://www.sciweavers.org/tex2img.php?eq=%7BR%7D%5E%7Bd%5Ctimes%20n%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="{R}^{d\times n}" width="43" height="19" /> and the target output <img src="http://www.sciweavers.org/tex2img.php?eq=Y%3D%5Cbegin%7BBmatrix%7Dy_%7Bi%7D%5Cend%7BBmatrix%7D_%7Bi%3D1%7D%5E%7Bn%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="Y=\begin{Bmatrix}y_{i}\end{Bmatrix}_{i=1}^{n}" width="92" height="29" />, where n is the number of samples in the dataset and d is the dimensionality of the input. The target <img src="http://www.sciweavers.org/tex2img.php?eq=y_%7Bi%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="y_{i}" width="19" height="15" /> is generated by a function of input <img src="http://www.sciweavers.org/tex2img.php?eq=x_%7Bi%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="x_{i}" width="18" height="15" /> plus additive noise<img src="http://www.sciweavers.org/tex2img.php?eq=%20%5Cvarepsilon%20_%7Bi%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt=" \varepsilon _{i}" width="18" height="15" />) (Almosallam ,2017):
   
<span align="center" style="border:1px solid red;">
 <img align="center" src="http://www.sciweavers.org/tex2img.php?eq=%20y_%7Bi%7D%3D%20x_%7Bi%7D%2B%5Cvarepsilon%20_%7Bi%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" border="0" alt=" y_{i}= x_{i}+\varepsilon _{i}" width="89" height="17"/>.
</span>

The objective of this work is to find the simplest function which maximize the probability of observing the target output given the input.

## Radial Basis Functions (RBF)
The model optimizes the probability of the data generated by a linear combination of m Radial Basis Functions. In the proposed solution, the RBF has different methods,see figure 1, as the follow:
  * **Global length-scale (GL):** All the basis functions share the same length scale.
  * **Variable length-scale (VL):** Each basis has a specific length-scale.
  * **Global Diagonal (GD):** All the basis functions share the same diagonal covariance.
  * **Variable Diagonal (VD):** Each basis has a specific diagonal covariance.
  * **Global Covariance (GC):** All the basis functions share the same full covariance.
  * **Variable Covariance (VC):** Each basis has a specific full covariance.
 
 
 
<span align="center">
  <img src="https://github.com/salasraj/images/blob/master/methods.png">
  <br /><caption>Figure 1: The results of training GPVL, GPVD and GPVC with different numbers of basis functions (m) on the same data. The ellipses represent the learned covariances of the RBFs. The log marginal likelihoods are shown above each plot (Almosallam ,2017)</caption>

</span> 

## Heteroscedastic Noise
The model predicts variance of two components. These include the model variance and the term <img src="http://www.sciweavers.org/tex2img.php?eq=%20%5Csigma%5E%7B2%7D%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt=" \sigma^{2} " width="22" height="18" /> which is the noise uncertainty and  assumed to be white Gaussian noise. The predictive variance enhanced by modelling the noise variance as a function, linear combination of basis functions, of input to account for variable and input-dependent noise such as heteroscedastic noise. In the proposed code, when the heteroscedastic noise option is disabled, the model assumed a global Gaussian noise variance. However, If the point estimates was only the interest, this feature can be disabled to minimize the required number of basis functions and speedup the optimization process. Figure 2 shows the effect of modelling with heteroscedastic noise.

<span align="center">
  <img src="https://github.com/salasraj/images/blob/master/HeteroNoise.png">
  <br /><span>Figure 2: Presenting the effect of modelling the noise variance as a function of the input<img src="http://www.sciweavers.org/tex2img.php?eq=%20%5Csigma%5E%7B2%7D%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt=" \sigma^{2} " width="22" height="18" />(x) compared to the full GP all using an RBF function. Note that the BFM with heteroscedastic noise prediction fits the data best with the largest log marginal likelihood (Almosallam ,2017)
  </span>
</span> 

## Splitting the dataset
The data is split into three sets including training, validation and Testing. The training set is used for optimizing the models while the validation set for model Selection. Finally, the testing set to prediction and reporting the results.

## Input noise
The model deals with the magnitude errors in the dataset in two different ways. If the input noise option is assigned to false, the errors will be treated as input features. However, they will be used as an additional input noise if the option was set to true value.

## Cost-sensitive Learning
Cost-sensitive learning is used to train the model with balanced data distribution. For this reason, the model uses input weight vector in order to weigh the samples differently during the optimization time. There are three weighting options:
* **Balanced:** Rare samples is weighted heavily during the training stage.
* **Normalized:** Each input will have an error cost. 
* **Normal:** The samples are equally important so no weights will be assigned. 

## References
Almosallam, I. (2017). Heteroscedastic Gaussian processes for uncertain and incomplete data (Doctoral dissertation)
