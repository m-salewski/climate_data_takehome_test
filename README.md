# Climate Data Takehome Test

## Data Science Takehome Test

M. Salewski, 27.03.2021

## Introduction

All analyses use only Pandas; visualizations use Matplotlib.  
These should run with most recent version of Pandas and Matplotlib. 

All CSVs should be placed in a directory named "data"; not included in the repository.

## EDA

### EDA1

> "Which country has the most severe overall temperature change from decade to decade?   
>    Please elaborate on how you interpret severe change and decade to decade."

A simple interpretation of from "from decade to decade" will be used, namely any year ending with a "0", 
for example 1800,1810, etc.

For most severe temperature change, the procedure was to 

1. aggregate on country and decade
2. compute the mean temp for each decade
3. save the result

And then using the resulting data,

1. aggreate on Country 
2. compute the longest consecutive sequence of decades
3. compute the standard deviation of the differences of the mean temperatures per decade. 
    
Sort the results based on temperature change.

Since the standard deviation has some sensitivity to outliers, we use this as a measure of the variability of the temperature differences among consecutive decades. 

This means we measure the mean of the sequence of decades and look at the measure of the deviations. 

**Comments**  
According to this metric, Kuwait seems to have the maximum variance.  
This is initially surprising since Kuwait might be considered to be quite stable being nearer to the equator; other locations further north are expected to show variations. 

Geographically, Kuwait also stands out in this list; otherwise groupings of Caucus and North American regions are seemingly consistently represented.

More investigation of Kuwait is recommended.

For all results, please see notebook `EDA_1.ipynb`.


### EDA 2 

>2. Now write a function which returns N entries of cities which have the highest variability in `AverageTemperature` in a specified time range.

Variability for this measurement is based on the inter quantile range; 
this is more robust to outliers since we should consider more stable trends.

The procedure here filters the data to date ranges provided (as strings).  
There is an additional option to filter results which don't have a sufficient filling rate.

Once the data is filtered by date range, the data is aggreated by location and the IQR is computed. 

**Comments**  

The top cities with the maximum variability are in China and Russia. 
Further information would be needed to validate this.

**Bugs**  
There seems to be a bug which is only found in plotting. 

(I suspect here:

```python
    # Get time series for these cities
    pdf4 = pdf[pdf[city_key].isin(cities_list) & 
               pdf[country_key].isin(pdfN[country_key].tolist())].copy()
    pdf4 = filter_by_dates(pdf4, dt_range)
    pdf4.sort_values(by=[country_key, city_key, 'dt'], inplace=True)
```

The city and country pairs should be together. 
However, the bug may come earlier given the results of the variability function)

For all results, please see notebook `EDA_2_and_3.ipynb`.

### EDA 3

> 3. Plot the temperature development (and uncertainty) over time (aggregate as you see fit) of the top 4 entries of your function for a given time period and save the plot as a `.png` file.

For this part, the 4 locations from the previosu part are used with an aggreation based on the decade from earlier. It certainly shows a clear upward trend for all locations. 

**Comments**  
The uncertainty has been excluded from this. A simple measure would be the standard deviation for decade aggregations.

### Predictive modeling

This is omitted in this analysis. A proposed soluton would be seasonal ARIMA, which would also consider the larger timescales found in climate dynamics.
