# analytics-II-final-workshop-2021-II

# Introduction 📔

The agricultural sector is one of the sectors that is most benefited from climate forecasts. In particular, the sub-seasonal scale provides information that could help short-term planning of agricultural activities and efficient management of water resources.

There is currently a great boom to implement and improve agroclimatic services in countries that present high vulnerability and little adaptation, since the models that currently provide information on a sub-seasonal scale do not satisfy the needs of the agricultural sector, because they sometimes present problems as very coarse resolution or biases with respect to local climatic conditions.

During the last few years, different studies have shown that there is a wide field of application of artificial intelligence in the modeling of climatic variables. In particular, different authors have proposed AI models for the calibration of precipitation forecasts produced by general circulation models.

# Statement 🌧

You have been hired to train a model responsible for the calibration of sub-seasonal precipitation forecasts in the Colombian rice sector. The target variables is the accumulated precipitation of 1 week occured in each of 4 Colombian departments (Sucre, Córdoba, Norte de Santander and César) and the features are: the forecast outputs of different variables of the European global circulation model (ECMWF) and other variables derived from satellite information.

# Data Set ℹnformation 

There are two sources from which data were gathered:

* [ECMWF model](https://iridl.ldeo.columbia.edu/SOURCES/.ECMWF/.S2S/.ECMF/.reforecast/.perturbed/) that provides a resolution of 165 kilometers and retrospective forecasts of 20 years. This model is initialized twice a week (Monday and Thursday), generating in each of these, forecasts with a horizon of 46 days ahead. The first 7 days of forecast generated by this model in the same week of the period of interest are considered. Furthermore, this models generates different output variables, from which 3 were pre-selected for you:

  * Convective Available Potential Energy (CAPE), is the amount of fuel available to a developing thunderstorm, it describes the instabilily of the atmosphere and provides an approximation of updraft strength within a thunderstorm.

  * Total Colum Water (TCW), provides information about the amount
of water, in its different states (water vapor, liquid water, ice clouds, rain and snow), which is present in a column that extends from the earth's surface to the top of the earth atmosphere.

  * Total Accumulated Precipitation (TCP)

* [Climate Hazards Group InfraRed Precipitation with Station](https://iridl.ldeo.columbia.edu/SOURCES/.UCSB/.CHIRPS/.v2p0/) (CHIRPS) is a 35+ year quasi-global rainfall data set since 1981 that provides a resolution of 5 kilometers. From this source two features were extracted:

  * Precipitation map of the prevoius week. Some pixels are NaN because correspond to precipitation in oceanic locations that are not areas of interest.

The training dataset consists of 1400 weeks, for each week, there are four images of shape (56, 42), each one corresponds to the features described above. Notice that each of the features can be considered as channels of an image of shape (56, 42), therefore, the training dataset can be thought as a 4D - tensor with dimensions (1400, 56, 42, 4) where:

* axis 0 is the number of weeks of the dataset (1400) and **can vary**.

* axis 1 is the height of the image (56)

* axis 2 is the width of the image (42)

* axis 3 is the depth or channels of the tensor and correspond to the number of features described above (4).

# Part 0. Data (0.5 points) 🕵

- There are missing values in some pixels of the CHIRPS channel. What technique can you use in order to deal with them? (0.25)

- Try with different normalization techniques. Over which axis should this normalization be perfomed? (0.25)


# Part 1. Shallow models (2.5 points) ☝

Apply SHALLOW models (opposed to deep) to develop predictive models for forecasting the temperature of the seventh future day:

- Establish variables from historical data. For instance, you can set data such as the average value of each pixel for each feature of the last 2 weeks, 4 weeks, etc., (0.5 points)
  
- Create a cross-sectional dataset, estimating the variables to be used every day, or every week, or every month. (0.5 points)

- Apply feature selection techniques such as PCA, LDA and Lasso. (0.5 points) 

- Try SVM, Lasso and Ridge regression models (1 point)

- Try traditional artificial neural network models (0.5 points)

Be aware that you must follow a correct evaluation protocol in order to avoid data leakage from the test sets on the normalization and feature selection processes. Mistakes will be punished (-0.5 points)


# Part 2. Convolutional models (2 points + 1 **BONUS** point) ✌

Consider the data as a multivariate time series and 2-D convolutional neural networks (ConvNets) to predict the accumulated precipitation of the next week:

- Use 2-D convolutional filters and pooling layers. Test different architectures in terms of kernel sizes (for convolutional layers) pooling sizes(for pooling layers) and number of neurons (for dense layers). For example, the input shape of the first Conv2D layer should be (56, 42, 3) (1 point)

- Apply and evaluate the impacts of using dropout layers and Ridge/Lasso (also known as L1/L2) regularization techniques with different hyper-parameter values to probe. (0.5)

- **(BONUS)** Take into account observations of previous weeks and establish a  window prediction. The features must then be transformed into a 4D tensor of shape **(window_size, height, width, channels)** in order to predict the next week's accumulated precipitation, plus, adding the batch dimension it would be **(batch_size, window_size, height, width, channels)**. You could take a look at the [TensorFlow Conv3D](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv3D) implementation for this (1 point)


As with the shallow models, it is important to establish a well designed evaluation protocol that allows for the comparison of the different architectures.


# Part 3: Competition and deliverables (1 **BONUS** point) 🏆

## Inputs 🤖

You are given: 

* Both the features and targets of the training set (1400 weeks until 2018) already pre-processed. You should split a part of this dataset to create a validation set and perform hyper-parameter optimization in order to choose the best architecture.

* Only the features of the **test set** (70 weeks until 2019) already preprocesed. You should generate the predictions for this data with the best model you chose in order to be compared with the true target that will be kept unrevealed.

## Submission format and deliverables ✅

You should deliver:

- Code: a jupyter notebook with the code with which you developed your solution.

- {your_name}_submission.csv: a file with the predictions of the model you chose, where:

  - The first row or header of the file contains the four places of interest: "sucre,cordoba,norte_de_santander,cesar"

  -  The 70 subsequent rows contains the prediction of accumulated precipitation for each of the 4 places of interest.

  - Do **NOT** shuffle the test features, such that the i-th + 1 row of your submission corresponds to the prediction for the i-th datapoint in the test set.   

The metric to assess the performance of your model is [RMSE](https://www.statisticshowto.com/probability-and-statistics/regression-analysis/rmse-root-mean-square-error/).
