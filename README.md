# Agriculture-Commodities-Price-and-Seasons

There are two raw datasets available, one with MSP values of different commodities throighout different years, and the other with minimum, maximum and modal price values for different commodities at different APMCs in different months.

## Aim:
1. Test and filter outliers.
2. Understand price fluctuations accounting the seasonal effect
    
    -Detect seasonality type (multiplicative or additive) for each cluster of APMC and commodities
    
    -De-seasonalise prices for each commodity and APMC according to the detected seasonality type
3. Compare prices in APMC/Mandi with MSP(Minimum Support Price)- raw and deseasonalised
4. Flag set of APMC/mandis and commodities with highest price fluctuation across different commodities in each relevant season, and year.


## Approach Methodology and Documentation:

* The code for the first task: [Task One](https://github.com/itsmepiyush2/Agriculture-Commodities-Price-and-Seasons/blob/master/exploration%20and%20outlier%20detection.ipynb)

The MSP varies greatly from commodity to commodity as can be seen from this bar plot.
<img src="msp.png" class="img-responsive" alt="">

While carrying out outlier detection, one must be very careful and meticulous in order not to remove any useful information. Thus, it is important to keep in mind that a univariate outlier might not be a multivariate or regression outlier and vice versa. Thus, simply plotting boxplots and removing data values that seem to have an abnormal behaviour might chop off some useful information. Hence, I'll follow a very rudimentary approach to detect outliers.
The basic idea is the intuition that minimum, maximum and modal prices, throughout time, should follow similar patterns. Thus, they should be directly proportional to each other. In other words, they should have a positive correlation.
<img src="scatterplot_before_cleaning.png" class="img-responsive" alt="">

From the scatterplots, it is evident that there are three data points that do not conform to the general behaviour, two with minimum price > 500000 and one with maximum price > 1400000. Once these data points are removed, the scatterplots are as shown.
<img src="scatterplot_after_cleaning.png" class="img-responsive" alt="">

Now even after cleaning the data, there is one particular point that shows some abnormal behaviour. But, it's scale is almost negligible compared to the previous outliers. Moreover, there could be some other reason of it showing that specific behaviour which might not be decipherable right now. So removing it might as well be more dangerous.
