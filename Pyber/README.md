# matplotlib-challenge



```python
# Dependencies
import pandas as pd
import numpy as np
import os
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
# Read the csv files
city_data = pd.read_csv("raw_data/city_data.csv")
ride_data = pd.read_csv("raw_data/ride_data.csv")
city_data.head()
x = city_data.loc[city_data['city'] == 'Port James']
x
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>Port James</td>
      <td>15</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Port James</td>
      <td>3</td>
      <td>Suburban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Check for NAN values and replace with 0
city_data.fillna(value=0)
city_data
#len(city_data)

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South Josephville</td>
      <td>4</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>6</th>
      <td>West Sydneyhaven</td>
      <td>70</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Travisville</td>
      <td>37</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Torresshire</td>
      <td>70</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Lisaville</td>
      <td>66</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Mooreview</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Smithhaven</td>
      <td>67</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Carrollfort</td>
      <td>55</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Port Josephfurt</td>
      <td>28</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Lake Jeffreyland</td>
      <td>15</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>15</th>
      <td>South Louis</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>16</th>
      <td>West Peter</td>
      <td>61</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Kimberlychester</td>
      <td>13</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Alyssaberg</td>
      <td>67</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Sarabury</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Yolandafurt</td>
      <td>7</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Edwardsbury</td>
      <td>11</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>22</th>
      <td>New Andreamouth</td>
      <td>42</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>23</th>
      <td>New David</td>
      <td>31</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Arnoldview</td>
      <td>41</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Williamshire</td>
      <td>70</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Lisatown</td>
      <td>47</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>27</th>
      <td>New Aaron</td>
      <td>60</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Swansonbury</td>
      <td>64</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Fosterside</td>
      <td>69</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>96</th>
      <td>New Lynn</td>
      <td>20</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Port Jose</td>
      <td>11</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Johnland</td>
      <td>13</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>99</th>
      <td>West Tony</td>
      <td>17</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Port James</td>
      <td>3</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Campbellport</td>
      <td>26</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Port Guytown</td>
      <td>26</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Webstertown</td>
      <td>26</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Clarkstad</td>
      <td>21</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>105</th>
      <td>North Tracyfort</td>
      <td>18</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Martinmouth</td>
      <td>5</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>107</th>
      <td>New Jessicamouth</td>
      <td>22</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>108</th>
      <td>South Elizabethmouth</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>109</th>
      <td>East Troybury</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>110</th>
      <td>Kinghaven</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>111</th>
      <td>New Johnbury</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>112</th>
      <td>Erikport</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>113</th>
      <td>Jacksonfort</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>114</th>
      <td>Shelbyhaven</td>
      <td>9</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Matthewside</td>
      <td>4</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>116</th>
      <td>Kennethburgh</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>117</th>
      <td>South Joseph</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>118</th>
      <td>Manuelchester</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Stevensport</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>120</th>
      <td>North Whitney</td>
      <td>10</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>121</th>
      <td>East Stephen</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>122</th>
      <td>East Leslie</td>
      <td>9</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Hernandezshire</td>
      <td>10</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Horneland</td>
      <td>8</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>125</th>
      <td>West Kevintown</td>
      <td>5</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
<p>126 rows × 3 columns</p>
</div>



### MERGE DUPLICATE ROWS FOR SAME CITY


```python
city_data_grouped = city_data.groupby(['city', 'type'])
city_data_unique = pd.DataFrame(city_data_grouped.sum())
#city_data_unique.head()
city_data_unique = city_data_unique.reset_index().set_index('city')
city_data_unique.head()

#x1 = city_data_unique.loc[['Port James']]
# len(x1)

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>Urban</td>
      <td>21</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>Urban</td>
      <td>67</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>Suburban</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>Urban</td>
      <td>21</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>Urban</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
</div>




```python
ride_data.fillna(value=0)
ride_data.head()
#y = ride_data.loc[ride_data['city'] == 'Port James']
# y['fare'].count()
#y['fare'].mean()
#len(y)
ride_data = ride_data.set_index('city')
ride_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Sarabury</th>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>South Roy</th>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>Wiseborough</th>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>Spencertown</th>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>Nguyenbury</th>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
# join both the DFs into one
cityride_df = city_data_unique.join(ride_data)
cityride_df = cityride_df[["type", "driver_count", "ride_id", "fare"]]
cityride_df.head()
# len(cityride_df)

#h = cityride_df.loc[['Port James']]
# w =h['ride_id'].unique()
#i =  cityride_df.loc[cityride_df['ride_id'] == 2259499336994]
# len(w)
#len(h)
#i

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>driver_count</th>
      <th>ride_id</th>
      <th>fare</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>Urban</td>
      <td>21</td>
      <td>4267015736324</td>
      <td>31.93</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>Urban</td>
      <td>21</td>
      <td>8394540350728</td>
      <td>6.42</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>Urban</td>
      <td>21</td>
      <td>1197329964911</td>
      <td>18.09</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>Urban</td>
      <td>21</td>
      <td>357421158941</td>
      <td>20.74</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>Urban</td>
      <td>21</td>
      <td>6431434271355</td>
      <td>14.25</td>
    </tr>
  </tbody>
</table>
</div>



# PYBER ANALYSIS
------

### GET KEY VARIABLES


```python
# Groupby on city
cityride_grouped_df = cityride_df.groupby(["city"])

# Average fare per city by using groupby object
average_fare = cityride_grouped_df["fare"].mean()

# Total number of rides per city
total_rides_city = cityride_grouped_df["ride_id"].count()

# Total number of driver per city
driver_count_df = city_data_unique[["driver_count"]]

# City Type (Urban, Suburban, Rural)
city_type_df= city_data_unique[["type"]]

  
#average_fare.head()
#total_rides_city
#final_df
# driver_count_grouped_df.head()
#city_type_grouped_df.head(100)
city_type_df.loc[['Port James']]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Port James</th>
      <td>Suburban</td>
    </tr>
  </tbody>
</table>
</div>



### RESULTING DATAFRAME




```python
results_df = pd.DataFrame({"Average Fare":average_fare, "Rides Per City":total_rides_city})
results_df = results_df.join([city_type_df, driver_count_df])

# Format the Average Fare column
#results_df["Average Fare"] = results_df["Average Fare"]#.map("${0:,.2f}".format)
results_df['Average Fare'] =results_df['Average Fare'].astype(int)
results_df.head()
#len(results_df)
#results_df.loc[['Port James']]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Fare</th>
      <th>Rides Per City</th>
      <th>type</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23</td>
      <td>31</td>
      <td>Urban</td>
      <td>21</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20</td>
      <td>26</td>
      <td>Urban</td>
      <td>67</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37</td>
      <td>9</td>
      <td>Suburban</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23</td>
      <td>22</td>
      <td>Urban</td>
      <td>21</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21</td>
      <td>19</td>
      <td>Urban</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
</div>




```python
#### TEST
z = results_df.loc[["Port James"]]
z
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Fare</th>
      <th>Rides Per City</th>
      <th>type</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Port James</th>
      <td>31</td>
      <td>32</td>
      <td>Suburban</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>



#### TEST DATAFRAME RESULTS FOR DUPLICATE VALUES:


```python
if z['type'].value_counts().tolist()[0] == 1:
    print("Dataframe results Ok!")
else:
    print("Warning: Duplicate Values Present!")
```

    Dataframe results Ok!



```python
# Get total fare per city
total_fare_city = cityride_grouped_df["fare"].sum()

# % of Total Fares by City Type
results_df['Percent Total Fare(%)'] = (total_fare_city/total_fare_city.sum()*100).round(2)#.map("{0:,.2f}%".format)

# % of Total Rides by City Type
results_df["Percent Total Rides(%)"] = (results_df['Rides Per City']/results_df['Rides Per City'].sum()*100).round(2)#.map("{0:,.2f}%".format)

# % of Total Drivers by City Type
results_df['Percent Total Drivers(%)'] = (results_df['driver_count']/results_df['driver_count'].sum()*100).round(2)#.map("{0:,.2f}%".format)

# Rename the some columns
results_df = results_df.rename(columns={'driver_count':'Driver Counts', "type":"City Type"})

results_df.head()

####Test
# a = results_df.loc[results_df['City Type'] == 'Urban']
# a['Percent Total Fare(%)'].sum()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Fare</th>
      <th>Rides Per City</th>
      <th>City Type</th>
      <th>Driver Counts</th>
      <th>Percent Total Fare(%)</th>
      <th>Percent Total Rides(%)</th>
      <th>Percent Total Drivers(%)</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23</td>
      <td>31</td>
      <td>Urban</td>
      <td>21</td>
      <td>1.17</td>
      <td>1.31</td>
      <td>0.63</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20</td>
      <td>26</td>
      <td>Urban</td>
      <td>67</td>
      <td>0.84</td>
      <td>1.09</td>
      <td>2.00</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37</td>
      <td>9</td>
      <td>Suburban</td>
      <td>16</td>
      <td>0.53</td>
      <td>0.38</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23</td>
      <td>22</td>
      <td>Urban</td>
      <td>21</td>
      <td>0.82</td>
      <td>0.93</td>
      <td>0.63</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21</td>
      <td>19</td>
      <td>Urban</td>
      <td>49</td>
      <td>0.66</td>
      <td>0.80</td>
      <td>1.46</td>
    </tr>
  </tbody>
</table>
</div>




```python
####Test
z = results_df.loc[["East Douglas"]]
#z
#total_fare_city.sum()
```


```python
# Percent total Fare, Rides, Drivers per City Type
percent_total_citytype_df = results_df.groupby('City Type')

percent_fare_citytype = pd.DataFrame(percent_total_citytype_df['Percent Total Fare(%)'].sum())
percent_fare_citytype

percent_rides_citytype = pd.DataFrame(percent_total_citytype_df['Percent Total Rides(%)'].sum())
percent_rides_citytype

percent_drivers_citytype = pd.DataFrame(percent_total_citytype_df['Percent Total Drivers(%)'].sum())
percent_drivers_citytype

Combined_percent_citytype_df =percent_fare_citytype.join([percent_rides_citytype, percent_drivers_citytype])
Combined_percent_citytype_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percent Total Fare(%)</th>
      <th>Percent Total Rides(%)</th>
      <th>Percent Total Drivers(%)</th>
    </tr>
    <tr>
      <th>City Type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>6.69</td>
      <td>5.25</td>
      <td>3.12</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>30.34</td>
      <td>26.34</td>
      <td>19.14</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>62.97</td>
      <td>68.42</td>
      <td>77.84</td>
    </tr>
  </tbody>
</table>
</div>



### SCATTER PLOT



```python
# Create legend for colors
colors = ['lightcoral', 'gold', 'lightskyblue']

# Use seaborn to make the scatter plot
sns.lmplot(x='Rides Per City', y='Average Fare', data=results_df, fit_reg=False, hue='City Type', 
           legend=False, size=8, 
           scatter_kws={"s":results_df["Driver Counts"]*4, 'alpha':0.3, 'edgecolors':'black', 'linewidths':2},
           palette=colors)
# Make the grid, set x-limit and y-limit
plt.grid()
plt.xlim(0,results_df['Rides Per City'].max()+5)
plt.ylim(0,results_df['Average Fare'].max()+10)

# Make x-axis, y-axis & title labels
plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel("Average Fare (per city)")
plt.ylabel("Total Rides (per city)")

# Format the legend and plot
plt.legend(loc='upper right', title='City Types')
plt.show(1)
```


![png](output_19_0.png)


### PIECHART: General Labels and Colors


```python
# Labels for the sections of our pie chart
labels = Combined_percent_citytype_df.index.tolist()

# The colors of each section of the pie chart
colors = ['lightskyblue', 'gold', 'lightcoral']
```

### PIECHART:  % of Total Fares by City Type 


```python
# The values of each section of the pie chart
sizes_fare = Combined_percent_citytype_df['Percent Total Fare(%)'].round(2).tolist()

# Title for Percent Fair
plt.title("% of Total Fares by City Type")

# Tells matplotlib to seperate the "Python" section from the others
explode_fare = (0, 0, 0.1)

# Creates the pie chart based upon the values above
plt.pie(sizes_fare, explode=explode_fare, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

# Print pie chart to the screen
plt.show()
```


![png](output_23_0.png)


### PIECHART:  % of Total Rides by City Type 


```python
# The values of each section of the pie chart
sizes_rides = Combined_percent_citytype_df['Percent Total Rides(%)'].round(2).tolist()

# Title for Percent Fair
plt.title("% of Total Rides by City Type")

# Tells matplotlib to seperate the "Python" section from the others
explode_rides = (0, 0, 0.1)

# Creates the pie chart based upon the values above
plt.pie(sizes_rides, explode=explode_rides, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

# Print pie chart to the screen
plt.show()
```


![png](output_25_0.png)


### PIECHART:  % of Total Drivers by City Type 


```python
# The values of each section of the pie chart
sizes_rides = Combined_percent_citytype_df['Percent Total Drivers(%)'].round(2).tolist()

# Title for Percent Fair
plt.title("% of Total Drivers by City Type")

# Tells matplotlib to seperate the "Python" section from the others
explode_rides = (0, 0, 0.1)

# Creates the pie chart based upon the values above
plt.pie(sizes_rides, explode=explode_rides, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

# Print pie chart to the screen
plt.show()
```


![png](output_27_0.png)


