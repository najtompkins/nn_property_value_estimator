# Property_Value_NN_Estimator
### _Note, any .ipynb in this repo that uses the tensor-flow or keras-tuner library was processed in Google Coolab_
## Kaylee
  - **Kaylee, please add a quick note about concatenating the data that you pulled, as well as another quick note about formatting the addresses to push into the wrapper without error.**
  - Data acquisition and Redfin Wrapper, pulling data.
      - Cleaning Address data, feeding it into the wrapper
      - Pulling the data
      - Cleaning the pulled data
    
## Martin
  - Shape of the data, integrity of the data, regression and stats tests.
      - Understanding the upper/lower bounds, and resolving regression visual errors.

## Nathan-Andrew Tompkins
  ### _code located in file: nn_model_tuning_2_
  ### Keras Tuner structure
  - Using the Keras-Tuner library our hyperparameter tuning function initializes a Sequential keras model and uses four separate activation functions to test on for the hidden layers: 'relu', 'tanh', 'linear', 'softplus'.
  - The tuner-function was set to choose between 2 and 20 layers, each comprised of up to 20 neurons each.
  - The output layer was designed to have a single output, which would be the predicted house price.
  - As this was not a classic example of a model designed to cluster or classify data and instead predict a numerical value close to the actual value, it became neccesary to find a proper loss and evaluation method.
    - Our loss function was the "Mean Absolute Percentage Error" function. This allowed for our model to test the data's accuracy within a certain percentage of the target instead of exacting values.
    - The MAE metric was used to determine the average value that the model was off in it's pricing estimation. (Note that this value is greater than expected due to the high variance of housing prices above 2.5 million.)
  - The tuner-hyperband itself is initialized using the tuning-function as the first parameter. The "objective" here is "val_loss" as the primary function of this model is to reduce the percentage error between predicted and target vales. Likewise, the model callback/checkpoint "mode" variable is looking for the minimal value of the loss function to determine which tested model performs the best.

  ### Final model structure
  - Due to unexpected errors when using the best model determined using the keras-tuner our final model uses only a few of those specifications. In the end, our model is Sequential and comprised of 4 layers:
    1. Input layer using the "relu" activaiton function
    2. Two hidden layers with 11 and 13 neurons respectively using the "linear" activation function.
    3. Output layer using the "linear" function.
   
  ### Model Results
  - **loss: 9.2879** Indicates that predicted housing prices are within ~9.3% of the target values.
  - **mae: 38710.2656**. Indicates that our data, on average, is off by ~38k. Again this is due to the high variance and low number of datapoints of houses above 2.5$ million.
  
## David
  - Talk about the content of the data using Tableau visualizations and distribution of the features among the addresses
  - Price distribution

