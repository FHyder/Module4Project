# Module4Project
Regression analysis of wine prices


### Module 4! Took a long time to get here but boy am I glad to have made it this far! For our penultimate project we were given the freedom to choose our own dataset to complete our project. I am a HUGE wine enthusiast. My love for wine resulted from my several years spent waiting tables at Italian restaurants. No pasta is complete without a good glass of wine. However, it can't be just any type of wine. Each dish has it's wine-pair made in heaven and the more you learn about wine the higher your likelihood of achieving the perfect amalgamation of flavors in your mouth. That's why, when I found this dataset I knew I had to jump on this opportunity to learn more about my favorite drink. In this project, I will attempt to build a model that can accurately predict the price of wine based a a set number of features available in this dataset. This dataset is available on kaggle and lifted from the 8/2017 issue of Wine Enthusiast Magazine.


### The features presented in this csv file are as follows:

#### Country: The country that the wine is from
#### Description: Description of wine by Sommeliers
#### Taster_name: Sommelier name
#### Taster_twitter_handle: Taster twitter handle
#### Designation:The vineyard within the winery where the grapes that made the wine are from
#### Winery: The winery that made the wine
#### Points: The number of points WineEnthusiast rated the wine on a scale of 1-100 (though they say they only post reviews for wines that score >=80)
#### Province: The province or state that the wine is from
#### Region_1: The wine growing area in a province or state (ie Napa)
#### Region_2: Sometimes there are more specific regions specified within a wine growing area (ie Rutherford inside the Napa Valley), but this value can sometimes be blank
#### Title: The title of the wine
#### Variety: The type of grapes used to make the wine (ie Pinot Noir)
#### Price: The cost for a bottle of the wine (target variable)

### As with every project, my first step was to load the CSV file into a dataset and do some preliminary EDA. My initial observations showed the following:

['df.info](https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.10.05%20PM.png)

### I noticed that there are few columns with missing values. '''df.isna().sum() shows the following:
### Off the bat, designation, region_1 and region_2 have way too many null values and taster_name or twitter_handle isn't relevant to the price of our wines drop these columns. After this I used '''df.dropna()''' to drop remaining null values. This leaves me with the following features to explore: country, description. winery, points, province, title, and variety. My approach was to tackle each categorical value first:

### Description was made into a score value through Setiment Analysis 
### Only kept the top 50 values of the variety column which makes up about 90% of the entire series
### Only kept the top 60 values of province which makes up about 92% of the entire series
### This leaves us with only 40 values for countries
### Dropped title and winery because there was too many nunique values in both series.
### Made categorical dummies out of my categorical features leaving me with the following dataset:


### Now onto continuous features:
### First I observed the distribution of our three continous features: price, score, and points: 
### Price has some outliers so I edited the set to only include prices <= 100. This fixed the distribution up. The score and points distributions didn't need help after that. 

[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.57%20PM.png]
###### price distribution



[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.57%20PM.png]
###### price distribution <=100


[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.04%20PM.png]
###### points

[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.11%20PM.png]
###### scores


### Next I checked for multicollinearity issues by printing a correlation matrix and comparing scatter plots, didn't find any strong correlations, kept my features.

[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.28%20PM.png]

[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.41%20PM.png]

[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.47%20PM.png]

[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.12.57%20PM.png]

## NOW TIME TO MODEL

### The metric I used to measure how well my models are doing are mean squared errors. In total I built 12 models: Multiple regression model, XGB Regression Model, 8 2-layer networks followed by 2 three layered networks to see if adding layers resulted in an improved MSE. I also monitored the training MSE for all of my models to assure that my models did not over/underfit. Here are my results:

https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-16%20at%2012.56.36%20AM.png

### My XGB Regression model performed the best with an MSE of 193.26, to demonstrate my model performance I presented a live simulation:


```r = random.randint(0,98057)```
```r2 = r + 10 ```
```wines10 = wdf.iloc[r:r2]```
```wines10```






```actualPrice = wines10.price```
```wines10 = wines10.drop('price', axis = 1)```
```y_pred = x_gb.predict(wines10)```
```prices = pd.DataFrame([])```
```prices['Actual'] = actualPrice```
```prices['Predicted'] = y_pred```
```prices```


[https://github.com/FHyder/Module4Project/blob/master/Screen%20Shot%202019-08-20%20at%207.54.50%20PM.png]


```N = 10```
```ind = np.arange(N) ```
```width = 0.35    ```
```fig = plt.figure(figsize=(10,10)) # Create matplotlib figure```



```width = 0.4```
```plt.title('Actual vs Predicted Prices')```
```plt.ylabel('Price')```
```plt.bar(ind, prices.Actual, width, label='Actual')```
```plt.bar(ind + width, prices.Predicted, width,label='Predicted')```
```plt.legend()```
```plt.xticks(ind + width, ('Wine1', 'Wine2', 'Wine3', 'Wine4', 'Wine5', 'Wine6', 'Wine7', 'Wine8', 'Wine9','Wine10'))```

```plt.show()```


[(https://raw.githubusercontent.com/FHyder/Module4Project/master/Screen%20Shot%202019-08-16%20at%2012.56.36%20AM.png)]

###  I think having access to the vintage of these wines would have improved my model performance and maybe some more experimentation with tuning paramaters for each of my models. My best model predicted price with an average error of 13 dollars. Considering that this error was calculated across 98,057 observation, I'd say it performed quite well.
