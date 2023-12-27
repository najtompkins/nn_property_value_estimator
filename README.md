# Property_Value_NN_Estimator
### _Note, any .ipynb in this repo that uses the tensor-flow or keras-tuner library was processed in Google Colab_

## Project Overview
The development process of this neural network model included 4 team members. The model, located in the [models](models/) directory as either an H5 or Keras filetype, was trained to predict the value of **single family homes in Dallas, TX** by evaluating features such as 1) number of bedrooms, 2) square footage, 3) zip code, etc.

### Results
#### PULL IMAGES FROM THE GOOGLE SLIDES FILE
##### https://docs.google.com/presentation/d/1ZFXWJKJN3-PoUnpL__Gys1JQMzztjaONkHsKJBH1v6c/edit#slide=id.g1ec42e7cc60_2_2

### Team Members: <br> 
**Martin Bedino**: GitHub: [mbedino99](https://github.com/mbedino99) <br>
**David Pinsky**: GitHub: [dpinsky1](https://github.com/dpinsky1) <br>
**Kaylee Patterson**: GitHub: [kayleepat](https://github.com/kayleepat) <br>
**Nathan-Andrew Tompkins (self)**: GitHub: [najtompkins](https://github.com/najtompkins) <br>

*Note:*<br>
*This project was developed as part of the 2023 UCF Data Analytics and Visualization Bootcamp.*

## Sourcing and Cleaning
There are 3 primary sources of the data used in training this model:
  1. List of Dallas addresses used in API call: [Dallas Central Appraisal District](https://www.dallascad.org/)
      * *Note* The list of cleaned addresses can be found in the [data](data/addresses_cleaned.csv) directory as cleaned: 
  2. Data included API calls to: [Redfin.com](redfin.com)
  3. The [Redfin API Wrapper](https://github.com/reteps/redfin) developed by GitHub user "[reteps](https://github.com/reteps)"

## Kaylee Patterson - Data Aquisition and Processing 
  ### Data Aquisition
  - This model is trained on Redfin housing data for a list of addresses located in Dallas, TX. These were sourced from the [Dallas Central Appraisal District website](https://www.dallascad.org/). This downloadable .csv was then cleaned to remove any rows with empty values. This cleaned dataset can be found [here](data/addresses_cleaned.csv).
  - All house feature data was obtained from [Redfin](redfin.com) through the Redfin Wrapper. The addresses from DCAD were reformatted to be used with a wrapper to access Redfin's unlisted API. (cleaning code [here](./code/ETL_addresses.ipynb).) This wrapper allowed access to the data without violating Redfin's terms of use against webscraping.
  - The code to pull this data is located in in the [code](./code/) directory. ([File located here](./code/redfin_data_collector.ipynb))
      - *Due to the extremely long length of time it took to pull data using this wrapper method, these API calls were  done in batches of 10,000 addresses. These batches were exported as .csv files to the data folder and then concatenated into the [combined_file.csv](./data/combined_file.csv).*
      - *Despite procurring a list of over 600,000 addresses, the time constraints of this project permitted data extraction for only around 40,000 homes. (Though this was more than enough to train the model.)*
    
## Martin - Statistical Analysis of Training Data
  - The training data extracted from the Redfin Wrapper was analyzed so that we may better understand how to process the data further before training our model on it.
    - We used analytical and visualization tools such as MatplotLib, Seaborn, SKLearn, ScyPi to better understand the shape and scope of our data. 
    - We performed statisatical analysis to understand the correlations between the variables in our data and our target variable of home prices.
    - Our data was cleaned in order to better account for outliers, visualize upper and lower bounds, as well as test for normality
  - Performed a preliminary OLS regression of the coefficients in order to visualize the validity of our data for further tuning.
    - The data was tested to ensure it clears the classical assumptions.
      - We found a small but possible instance of autocorrelation
      - We found the data to be homoskedastic
      - There is no perfect linear relation between home prices and our explanatory variables
  - From this quick analysis we find a adjusted. r^2 value of 0.678, meaning there is room for improvement of the model through hyperparameter tuning.

## Nathan-Andrew Tompkins - Model Architecture and Tuning
  ### Code: [Here](code/nn_model_training.ipynb), Keras Model: [Here](models/redfin_property_estimator.keras)
  ### Keras Tuner structure
  - Using the Keras-Tuner library, our hyperparameter tuning function initializes a Sequential keras model and uses four separate activation functions to test on for the hidden layers: 'relu', 'tanh', 'linear', 'softplus'.
  - The tuner-function was set to choose between 2 and 20 layers, each comprised of up to 20 neurons each.
  - The output layer was designed to have a single output, which would be the predicted house price.
  - As this was not a classic example of a model designed to cluster or classify data and instead predict a numerical value close to the actual value, it became neccesary to find a proper loss and evaluation method.
    - Our loss function was the "Mean Absolute Percentage Error" function. This allowed for our model to test the data's accuracy within a certain percentage of the target instead of exacting values.
    - The MAE metric was used to determine the average value that the model was off in it's pricing estimation. (Note that this value is greater than expected due to the high variance of housing prices above 2.5 million.)
  - The tuner-hyperband itself is initialized using the tuning-function as the first parameter. The "objective" here is "val_loss" as the primary function of this model is to reduce the percentage error between predicted and target vales. Likewise, the model callback/checkpoint "mode" variable is looking for the minimal value of the loss function to determine which tested model performs the best.

  ### Final model structure
  - Due to unexpected errors when using the best model determined by the keras-tuner, our final model uses only a few of those specifications. In the end, our model is Sequential and comprised of 4 layers:
    1. Input layer using the "relu" activaiton function
    2. Two hidden layers with 11 and 13 neurons respectively using the "linear" activation function.
    3. Output layer using the "linear" function.
   
  ### Model Results
  - **loss: 9.2879** Indicates that predicted housing prices are within ~9.3% of the target values.
  - **mae: 38710.2656**. Indicates that our data, on average, is off by ~38k. Again this is due to the high variance and low number of datapoints of houses above 2.5$ million.
  
## David Pinsky - Data Visualization Dashboard
A Tableau Public Visualization of the data can be found [here](https://public.tableau.com/shared/XTXD43674?:display_count=n&:origin=viz_share_link), or downloaded [here](redfin_training_analysis.twbx)

- The visualizations within the Tableau Public dashboard are desinged to illustrate the full Redfin-extracted dataset as well as our compare it to our cleaned_data.csv.
- Our model's performance. 
- Visualizations include price distribution maps, 
- And datapoints by ZIP Code.

