# ENPM808A Final Project

## Technolgy Stack / Tools

<p align="left">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original-wordmark.svg" alt="python" width="70" height="70" />
</p>

## Installation

Download and install the required libraries for the python file by running the following commands in the terminal.

```sh
pip install numpy
pip install sklearn
pip install pandas
pip install os
pip install matplotlib
```

## Problem Statement
- The car like robot senses its environment using a 1D LIDAR sensor. The data recorded is documented in the form of “.CSV” files. The LIDAR data is read and tabulated as columns of the csv file, total 1080 columns named from ‘Laser 1’ to ‘Laser 1080’ where each column is a 0.25-degree scan of the LIDAR. The final goal, local goal and robot’s current position and pose information (which are spatially represented by 3D Cartesian coordinated and quaternion) is provided in subsequent columns. The last two columns consist of target variables linear velocities and the angular velocities of the robot. The data provided is captured from 3 different sets namely “Corridor”, “Open_box” where the robot is tested under different conditions to make learning more general. The file “special” contains high instances of some special maneuvers such as backing up.


### Step 1 (Data pre-processing & Feature Selection)
- Data preprocessing for the input is done to reduce noise and computation time.
Transforming or scaling the inputs and reducing the number of features will help us save 
a lot of computational time and the model will run more effectively giving us better 
results. We combine 20 columns of the LIDAR data into one by taking their mean which is equivalent to scanning at every 5 degrees. Since we know that quaternion q = qr + qi + qj + qk, we can add the qr and qk columns to a single column since qr and qi are equal to zero. This results in a CSV that has 60 features and last 2 columns as outputs or target features.

### Step 2 (Model training)
- This is a regression problem since our problem statement requires us to predict the linear and angular velocities. So, we went ahead to try 3 different models namely Linear Regression, Support vector machines and Neural networks. A pipeline was made where the data was firstly standardized to improve the computation and accuracy of the results. As we have multiple outputs, MultiOutputRegressor function was used in case of Linear Regression and SVM models.

#### Linear Regression:
###### Hyperparameters:
- a) Penalty- [l1 , l2] <br />
  b) Alpha- [0.0001, 0.001, 0.01, 0.1] (Regularization term) <br />
  c) Learning rate- [constant, optimal, invscaling, adaptive] <br />
- After Hyperparameter tuning the best parameters found were: <br />
 • Best penalty- l2 <br />
 • Best alpha- 0.01 <br />
 • Best learning rate- adaptive <br />
###### Results:
- 

#### Support Vector Machines (SVM):
###### Hyperparameters:
- a)	C- [1, 10, 100] (Regularization parameter) <br />
  b)  Gamma- [0.0001, 0.01, 1] <br />
  c)	Kernel - [rbf, poly] <br />
  d)	Degree- [1, 2] <br />

- After Hyperparameter tuning the best parameters found were: <br />
 • Best C- 100 <br />
 • Best gamma - 1 <br />
 • Best kernel- rbf <br />

###### Results:
- 

#### Neural Network:
###### Hyperparameters:
- a)	solver- [adam, sgd] (Strength of the L2 regularization term) <br />
  b)	activation- [relu, tanh] <br />
  c) alpha - [[0.001, 0.002, 0.003, 0.004, 0.005, 0.006, 0.007, 0.008, 0.009]] <br />
  d)	hidden layers sizes- range(5,100) <br />


- After Hyperparameter tuning the best parameters found were: <br />
•	Best solver - sgd <br />
•	Best activation - relu <br />
•	Best alpha- 0.002 <br />
•	Best hidden layer size- (77,77,77) <br />

###### Results:
- 

### Step 3 (Model selection and Testing)
- From the cross-validation score results obtained we have selected SVM as our optimum model with optimal hyper-parameters obtained during training, as it provided the best cross validation score (0.72) among the three. Now we deploy it for testing over the test dataset where similar procedure for data preprocessing is done and then fed to the model. We randomly shuffled the training data and took 800000 data points for the model testing. The scoring parameter used to find E_test is “r_2”. <br />

• E_test: 0.073 <br />
• E_out:  0.080 <br />

### Conclusion 
- Hence, we successfully compared 3 different models, decided the better model, and trained a SVM model for predicting the linear and angular velocities of the test data with minimal E_in. This model can be used for predicting future values of linear velocity and angular velocity albeit with lower E_out.
