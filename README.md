# Agriculture-Commodities-Price-and-Seasons

There are two raw datasets available, one with MSP values of different commodities throughout different years, and the other with minimum, maximum and modal price values for different commodities at different APMCs in different months.

## Aim:
1. Test and filter outliers.
2. Understand price fluctuations accounting the seasonal effect
    
    -Detect seasonality type (multiplicative or additive) for each cluster of APMC and commodities
    
    -De-seasonalise prices for each commodity and APMC according to the detected seasonality type
3. Compare prices in APMC/Mandi with MSP(Minimum Support Price)- raw and deseasonalised
4. Flag set of APMC/mandis and commodities with highest price fluctuation across different commodities in each relevant season, and year.


## Approach Methodology and Documentation:

### Exploration and Finding Outliers
* The code for the first task: [Task One](https://github.com/itsmepiyush2/Agriculture-Commodities-Price-and-Seasons/blob/master/exploration%20and%20outlier%20detection.ipynb)

The MSP varies greatly from commodity to commodity as can be seen from this bar plot.
<img src="msp.png" class="img-responsive" alt="">
#### "An outlier is an observation which deviates so much from the other observations as to arouse suspicions that it was generated by a different mechanism."
While carrying out outlier detection, one must be very careful and meticulous in order not to remove any useful information. Thus, it is important to keep in mind that a univariate outlier might not be a multivariate or regression outlier and vice versa. Thus, simply plotting boxplots and removing data values that seem to have an abnormal behaviour might chop off some useful information. Hence, I'll follow a very rudimentary approach to detect outliers.
The basic idea is the intuition that minimum, maximum and modal prices, throughout time, should follow similar patterns. Thus, they should be directly proportional to each other. In other words, they should have a positive correlation.
<img src="scatterplot_before_cleaning.png" class="img-responsive" alt="">

From the scatterplots, it is evident that there are three data points that do not conform to the general behaviour, two with minimum price > 500000 and one with maximum price > 1400000. Once these data points are removed, the scatterplots are as shown.
<img src="scatterplot_after_cleaning.png" class="img-responsive" alt="">

Now even after cleaning the data, there is one particular point that shows some abnormal behaviour. But, it's scale is almost negligible compared to the previous outliers. Moreover, there could be some other reason of it showing that specific behaviour which might not be decipherable right now. So removing it might as well be more dangerous.

We know that MSPs are decided by government policy. So the MSP dataset is simply a policy jotted down in a tabular form. There is no experiment so there are no observations in actuality. We are treating the data values in MSP dataset as ground truths. So attempting to detect outliers in the MSP dataset would seem absurd.

### Seasonality Detection, Seasonality Removal and Comparing Prices
* The code for the second and third tasks: [Tasks Two and Three](https://github.com/itsmepiyush2/Agriculture-Commodities-Price-and-Seasons/blob/master/seasonality%20detection%2C%20deseasonalising%20and%20comparing%20prices.ipynb)

#### "Any predictable change or pattern in a time series that recurs or repeats over a specific period can be said to be seasonal."
In order to detect seasonality in a time series, the signal needs to be broken down into trend, seasonality and the residue. In case of an additive seasonality, the signal is the summation of trend, seasonality and residue; and in case of multiplicative seasonality, the signal is the product of the three components.
<img src="seasonal_decompose.png" class="img-responsive" alt="">
The function `identify_seasonality` takes in two inputs, `APMC` and `Commodity`, and displays the type of seasonality that is present in the signal for the corresponding APMC and Commodity. It also decomposes the signal into its 3 components and plots them. Two use cases are given below in the form of a gif.
(The gifs are little slower than the actual running code.)
* Detecting additive seasonality
<img src="additive_example.gif" class="img-responsive" alt="">
* Detecting multiplicative seasonality
<img src="multiplicative_example.gif" class="img-responsive" alt="">

The function `deseasonalise` acts in a similar way except that it only returns the original and the deseasonalised signals. It acts as a helping function for another function `compare_prices` which takes in the inputs `APMC` and `Commodity` and compared the MSP, raw prices and de-seasonalised prices for the corresponding APMC and Commodity. A use case is given below.
<img src="compare_prices.gif" class="img-responsive" alt="">

### Flagging the Set of APMC-Commodity Clusters that have High Price Fluctuation:
* The code for the fourth task: [Task Four](https://github.com/itsmepiyush2/Agriculture-Commodities-Price-and-Seasons/blob/master/fluctuations.ipynb)

The basic idea here is to first collect all the sets of APMCs and Commodities that have more than a year's worth of data. Then finding how separated the data values are by finding the coefficient of variation. Then sort the APMC-Commodity clusters in decreasing order of coefficient of variation and choose the top few clusters which have the highest coefficients of variation.

After sorting the clusters in descending order of coefficients of variation, it is plotted.
<img src="fluctuation_cluster1.png" class="img-responsive" alt="">
Logically, only those clusters need to be selected which lie to the left of the elbow formation since these have high coefficients of variation. However, it is difficult to pin-point exactly where the elbow is being formed. Let's zoom in.
<img src="fluctuation_cluster2.png" class="img-responsive" alt="">
It is evident that the clusters having indices less than or equal to 20 need to be selected.

Now, if we consider Gaussian (Normal) distribution, we know that ~99.7% of the entire data lies within 3 standard deviations of the mean on both sides. So if in case, a data point crosses 3 standard deviations, then the cluster has high price fluctuation. Using this intuition, the values of modal price that cross 3 standard deviations are recorded.

### Conclusion:

A Minimum Support Price (MSP) is an example of price floors that are set by the government to prevent them from falling further. It is the minimum price in which a commodity can be sold. But setting of price floors have other effects on individual demand and supply. If a price floor is set above the equilibrium price, then it reduces the demand for the commodity but there is an increase in its supply.
<img src="price_floor.gif" class="img-responsive" alt="">

In other words, the producers produce more but the consumers are reluctant to buy at that price point. Thus, it creates a surplus of the commodity.
Since, it is the government that has intervened in the naturally facilitated demand and supply forces, it is the government that buys this surplus from the producer to counter the excess supply problem. However, that incurs a loss to the government itself.
In some other cases, the government introduces production quotas; these increase the prices while the producer is incentivised to produce less.
