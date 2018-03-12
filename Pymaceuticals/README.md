# matplotlib-challenge



```python
# Dependencies
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
# Read the csv files
clinical_trial_data = pd.read_csv("raw_data/clinicaltrial_data.csv")
mouse_drug_data = pd.read_csv("raw_data/mouse_drug_data.csv")
```


```python
# Check for NAN values and replace with 0
clinical_trial_data.fillna(value=0)
clinical_trial_data = clinical_trial_data.loc[clinical_trial_data['Mouse ID'] != 'g989']

####Test
#x = clinical_trial_data.loc[clinical_trial_data['Mouse ID'] == 'g989']
# len(x)

clinical_trial_data.head()
x = clinical_trial_data.loc[clinical_trial_data['Mouse ID'] == 'b128']
#x
```


```python
# Check for NAN values and replace with 0
mouse_drug_data.fillna(value=0)
mouse_drug_data = mouse_drug_data.loc[mouse_drug_data['Mouse ID'] != 'g989']

####Test
# x = mouse_drug_data.loc[mouse_drug_data['Mouse ID'] == 'g989']
# x
# mouse_drug_data['Mouse ID'].value_counts().sort_values(ascending=False)

#mouse_drug_data.head()
```


```python
# Combine the data from both the files into one df
combined_data_df = clinical_trial_data.merge(mouse_drug_data, on='Mouse ID')
combined_data_df.head()
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
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b128</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b128</td>
      <td>5</td>
      <td>45.651331</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>2</th>
      <td>b128</td>
      <td>10</td>
      <td>43.270852</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>3</th>
      <td>b128</td>
      <td>15</td>
      <td>43.784893</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b128</td>
      <td>20</td>
      <td>42.731552</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
  </tbody>
</table>
</div>



### Creating a scatter plot that shows how the tumor volume changes over time for each treatment.
#### Tumor Response to Treatment


```python
combined_data_grouped_df = combined_data_df.groupby(['Drug', 'Timepoint'])
drug_data_df = pd.DataFrame(combined_data_grouped_df['Tumor Volume (mm3)'].mean())
drug_data_df.head()
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
      <th></th>
      <th>Tumor Volume (mm3)</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Timepoint</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Capomulin</th>
      <th>0</th>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>44.266086</td>
    </tr>
    <tr>
      <th>10</th>
      <td>43.084291</td>
    </tr>
    <tr>
      <th>15</th>
      <td>42.064317</td>
    </tr>
    <tr>
      <th>20</th>
      <td>40.716325</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pivot the df to get drugs as columns and tumor volumes as its values
drug_data_pivot_df = pd.pivot_table(drug_data_df, index='Timepoint', columns='Drug', values='Tumor Volume (mm3)')
drug_data_pivot_df.round(0)
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Ceftamin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Naftisol</th>
      <th>Placebo</th>
      <th>Propriva</th>
      <th>Ramicane</th>
      <th>Stelasyn</th>
      <th>Zoniferol</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>0</th>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>45.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>44.0</td>
      <td>47.0</td>
      <td>47.0</td>
      <td>47.0</td>
      <td>47.0</td>
      <td>47.0</td>
      <td>47.0</td>
      <td>44.0</td>
      <td>47.0</td>
      <td>47.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>43.0</td>
      <td>48.0</td>
      <td>49.0</td>
      <td>50.0</td>
      <td>49.0</td>
      <td>49.0</td>
      <td>49.0</td>
      <td>43.0</td>
      <td>49.0</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>42.0</td>
      <td>50.0</td>
      <td>51.0</td>
      <td>52.0</td>
      <td>51.0</td>
      <td>51.0</td>
      <td>51.0</td>
      <td>41.0</td>
      <td>51.0</td>
      <td>51.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>41.0</td>
      <td>52.0</td>
      <td>53.0</td>
      <td>55.0</td>
      <td>54.0</td>
      <td>54.0</td>
      <td>53.0</td>
      <td>40.0</td>
      <td>54.0</td>
      <td>53.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>40.0</td>
      <td>54.0</td>
      <td>56.0</td>
      <td>58.0</td>
      <td>57.0</td>
      <td>57.0</td>
      <td>55.0</td>
      <td>39.0</td>
      <td>56.0</td>
      <td>55.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>39.0</td>
      <td>57.0</td>
      <td>58.0</td>
      <td>61.0</td>
      <td>60.0</td>
      <td>60.0</td>
      <td>58.0</td>
      <td>39.0</td>
      <td>60.0</td>
      <td>58.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>38.0</td>
      <td>59.0</td>
      <td>61.0</td>
      <td>63.0</td>
      <td>63.0</td>
      <td>62.0</td>
      <td>60.0</td>
      <td>37.0</td>
      <td>62.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>37.0</td>
      <td>61.0</td>
      <td>63.0</td>
      <td>66.0</td>
      <td>66.0</td>
      <td>65.0</td>
      <td>63.0</td>
      <td>37.0</td>
      <td>65.0</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>36.0</td>
      <td>64.0</td>
      <td>66.0</td>
      <td>71.0</td>
      <td>69.0</td>
      <td>68.0</td>
      <td>66.0</td>
      <td>35.0</td>
      <td>68.0</td>
      <td>66.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate the Standard Error
standard_errors_df = drug_data_pivot_df.sem(axis=1)
#standard_errors_df
```


```python
# Make the Scatter Plot
fig = plt.figure()
ax = fig.add_subplot(111)

# Line plot
pl1, = plt.plot(drug_data_pivot_df.index, drug_data_pivot_df['Capomulin'], '.r--', linewidth=0.5, dashes=(17,10))
pl2, = plt.plot(drug_data_pivot_df.index, drug_data_pivot_df['Infubinol'], '.b--', linewidth=0.5, dashes=(17,10))
pl3, = plt.plot(drug_data_pivot_df.index, drug_data_pivot_df['Ketapril'], '.g--', linewidth=0.5, dashes=(17,10))
pl4, = plt.plot(drug_data_pivot_df.index, drug_data_pivot_df['Placebo'], '.--', linewidth=0.5, color='black', 
                dashes=(17,10))

# ErrorBar Plot
pe1 = plt.errorbar(drug_data_pivot_df.index, drug_data_pivot_df['Capomulin'], standard_errors_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='red')
pe2 = plt.errorbar(drug_data_pivot_df.index, drug_data_pivot_df['Infubinol'], standard_errors_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='blue')
pe3 = plt.errorbar(drug_data_pivot_df.index, drug_data_pivot_df['Ketapril'], standard_errors_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='green')
pe4 = plt.errorbar(drug_data_pivot_df.index, drug_data_pivot_df['Placebo'], standard_errors_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='black')

# Scatter Plot
ps1 = plt.scatter(x=drug_data_pivot_df.index, y=drug_data_pivot_df['Capomulin'], marker="o", facecolors="red", 
                  edgecolors="black", alpha=0.75)
ps2 = plt.scatter(x=drug_data_pivot_df.index, y=drug_data_pivot_df['Infubinol'], marker="^", facecolors="blue", 
                  edgecolors="black", alpha=0.75)
ps3 = plt.scatter(x=drug_data_pivot_df.index, y=drug_data_pivot_df['Ketapril'], marker="s", facecolors="green", 
                  edgecolors="black", alpha=0.75)
ps4 = plt.scatter(x=drug_data_pivot_df.index, y=drug_data_pivot_df['Placebo'], marker="D", facecolors="black", 
                  edgecolors="black", alpha=0.75)

# Format Legend
legend_df = plt.legend(((pl1,ps1,pe1),(pl2,ps2,pe2),(pl3,ps3,pe3),(pl4,ps4,pe4)), 
                       ('Capomulin','Infubinol','Ketapril','Placebo'), loc='best', title='Drug Type', 
                       frameon=True)
legend_df.get_frame().set_edgecolor('black')

# Format Grid
plt.grid(linestyle = '--', dashes=(5,6))

# Set the axis limits
plt.xlim(-5,drug_data_pivot_df.index.max()+5)
plt.ylim(20,drug_data_pivot_df['Ketapril'].max()+10)

# Adjust Chart Size
plt.rcParams["figure.figsize"] = [8,8]

# Make x-axis, y-axis & title labels
ax.set_title("Tumor Response to Treatment", fontsize=15)
ax.set_xlabel("Time (Days)", fontsize=12)
ax.set_ylabel("Tumor Volume (mm3)", fontsize=12)

plt.show()
```


![png](output_9_0.png)


### Creating a scatter plot that shows how the number of metastatic (cancer spreading) sites changes over time for each treatment.
#### Metastatic Response to Treatment


```python
combined_data_grouped_df = combined_data_df.groupby(['Drug', 'Timepoint'])
drug_data2_df = pd.DataFrame(combined_data_grouped_df['Metastatic Sites'].mean())
drug_data2_df.head()
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
      <th></th>
      <th>Metastatic Sites</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Timepoint</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Capomulin</th>
      <th>0</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.160000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.320000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.375000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.652174</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pivot the df to get drugs as columns and tumor volumes as its values
drug_data2_pivot_df = pd.pivot_table(drug_data2_df, index='Timepoint', columns='Drug', values='Metastatic Sites')
drug_data2_pivot_df.round(2)
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Ceftamin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Naftisol</th>
      <th>Placebo</th>
      <th>Propriva</th>
      <th>Ramicane</th>
      <th>Stelasyn</th>
      <th>Zoniferol</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>0</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.16</td>
      <td>0.38</td>
      <td>0.28</td>
      <td>0.30</td>
      <td>0.26</td>
      <td>0.38</td>
      <td>0.35</td>
      <td>0.12</td>
      <td>0.26</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.32</td>
      <td>0.60</td>
      <td>0.67</td>
      <td>0.59</td>
      <td>0.52</td>
      <td>0.83</td>
      <td>0.62</td>
      <td>0.25</td>
      <td>0.52</td>
      <td>0.50</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.38</td>
      <td>0.79</td>
      <td>0.90</td>
      <td>0.84</td>
      <td>0.86</td>
      <td>1.25</td>
      <td>0.80</td>
      <td>0.33</td>
      <td>0.81</td>
      <td>0.81</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.65</td>
      <td>1.11</td>
      <td>1.05</td>
      <td>1.21</td>
      <td>1.15</td>
      <td>1.53</td>
      <td>1.00</td>
      <td>0.35</td>
      <td>0.95</td>
      <td>1.29</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.82</td>
      <td>1.50</td>
      <td>1.28</td>
      <td>1.63</td>
      <td>1.50</td>
      <td>1.94</td>
      <td>1.38</td>
      <td>0.65</td>
      <td>1.17</td>
      <td>1.69</td>
    </tr>
    <tr>
      <th>30</th>
      <td>1.09</td>
      <td>1.94</td>
      <td>1.59</td>
      <td>2.06</td>
      <td>2.07</td>
      <td>2.27</td>
      <td>1.67</td>
      <td>0.78</td>
      <td>1.41</td>
      <td>1.93</td>
    </tr>
    <tr>
      <th>35</th>
      <td>1.18</td>
      <td>2.07</td>
      <td>1.67</td>
      <td>2.29</td>
      <td>2.27</td>
      <td>2.64</td>
      <td>2.33</td>
      <td>0.95</td>
      <td>1.53</td>
      <td>2.29</td>
    </tr>
    <tr>
      <th>40</th>
      <td>1.38</td>
      <td>2.36</td>
      <td>2.10</td>
      <td>2.73</td>
      <td>2.47</td>
      <td>3.17</td>
      <td>2.78</td>
      <td>1.10</td>
      <td>1.58</td>
      <td>2.79</td>
    </tr>
    <tr>
      <th>45</th>
      <td>1.48</td>
      <td>2.69</td>
      <td>2.11</td>
      <td>3.36</td>
      <td>2.54</td>
      <td>3.27</td>
      <td>2.57</td>
      <td>1.25</td>
      <td>1.73</td>
      <td>3.07</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate the Standard Error
standard_errors2_df = drug_data2_pivot_df.sem(axis=1)
#standard_errors2_df
```


```python
# Make the Scatter Plot
fig = plt.figure()
ax = fig.add_subplot(111)

# Line plot
pl1, = plt.plot(drug_data2_pivot_df.index, drug_data2_pivot_df['Capomulin'], '.r--', linewidth=0.5, dashes=(17,10))
pl2, = plt.plot(drug_data2_pivot_df.index, drug_data2_pivot_df['Infubinol'], '.b--', linewidth=0.5, dashes=(17,10))
pl3, = plt.plot(drug_data2_pivot_df.index, drug_data2_pivot_df['Ketapril'], '.g--', linewidth=0.5, dashes=(17,10))
pl4, = plt.plot(drug_data2_pivot_df.index, drug_data2_pivot_df['Placebo'], '.--', linewidth=0.5, color='black', 
                dashes=(17,10))

# ErrorBar Plot
pe1 = plt.errorbar(drug_data2_pivot_df.index, drug_data2_pivot_df['Capomulin'], standard_errors2_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='red')
pe2 = plt.errorbar(drug_data2_pivot_df.index, drug_data2_pivot_df['Infubinol'], standard_errors2_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='blue')
pe3 = plt.errorbar(drug_data2_pivot_df.index, drug_data2_pivot_df['Ketapril'], standard_errors2_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='green')
pe4 = plt.errorbar(drug_data2_pivot_df.index, drug_data2_pivot_df['Placebo'], standard_errors2_df, capsize=4, 
             elinewidth=0.5, markeredgewidth=.5, fmt='none', ecolor='black')

# Scatter Plot
ps1 = plt.scatter(x=drug_data2_pivot_df.index, y=drug_data2_pivot_df['Capomulin'], marker="o", facecolors="red", 
                  edgecolors="black", alpha=0.75)
ps2 = plt.scatter(x=drug_data2_pivot_df.index, y=drug_data2_pivot_df['Infubinol'], marker="^", facecolors="blue", 
                  edgecolors="black", alpha=0.75)
ps3 = plt.scatter(x=drug_data2_pivot_df.index, y=drug_data2_pivot_df['Ketapril'], marker="s", facecolors="green", 
                  edgecolors="black", alpha=0.75)
ps4 = plt.scatter(x=drug_data2_pivot_df.index, y=drug_data2_pivot_df['Placebo'], marker="D", facecolors="black", 
                  edgecolors="black", alpha=0.75)

# Format Legend
legend_df = plt.legend(((pl1,ps1,pe1),(pl2,ps2,pe2),(pl3,ps3,pe3),(pl4,ps4,pe4)), 
                       ('Capomulin','Infubinol','Ketapril','Placebo'), loc='best', title='Drug Type', 
                       frameon=True)
legend_df.get_frame().set_edgecolor('black')

# Format Grid
plt.grid(linestyle = '--', dashes=(5,6))

# Set the axis limits
plt.xlim(-5,drug_data2_pivot_df.index.max()+5)
plt.ylim(-0.5,drug_data2_pivot_df['Placebo'].max()+.5)

# Adjust Chart Size
plt.rcParams["figure.figsize"] = [8,8]

# Make x-axis, y-axis & title labels
ax.set_title("Metastatic Spread During Treatment", fontsize=15)
ax.set_xlabel("Treatment Duration (Days)", fontsize=12)
ax.set_ylabel("Met. Sites", fontsize=12)

plt.show()
```


![png](output_14_0.png)


### Creating a scatter plot that shows the number of mice still alive through the course of treatment (Survival Rate)
#### Survival Rates


```python
combined_data_grouped_df = combined_data_df.groupby(['Drug', 'Timepoint'])
drug_data3_df = pd.DataFrame(combined_data_grouped_df['Mouse ID'].count())
drug_data3_df.head()
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
      <th></th>
      <th>Mouse ID</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Timepoint</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Capomulin</th>
      <th>0</th>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
    </tr>
    <tr>
      <th>10</th>
      <td>25</td>
    </tr>
    <tr>
      <th>15</th>
      <td>24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pivot the df to get drugs as columns and tumor volumes as its values
drug_data3_pivot_df = pd.pivot_table(drug_data3_df, index='Timepoint', columns='Drug', values='Mouse ID')
drug_data3_pivot_df.head()
#drug_data3_pivot_df.columns
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Ceftamin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Naftisol</th>
      <th>Placebo</th>
      <th>Propriva</th>
      <th>Ramicane</th>
      <th>Stelasyn</th>
      <th>Zoniferol</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>0</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>24</td>
      <td>25</td>
      <td>24</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>21</td>
      <td>25</td>
      <td>23</td>
      <td>23</td>
      <td>24</td>
      <td>23</td>
      <td>25</td>
      <td>23</td>
      <td>24</td>
    </tr>
    <tr>
      <th>10</th>
      <td>25</td>
      <td>20</td>
      <td>21</td>
      <td>22</td>
      <td>21</td>
      <td>24</td>
      <td>21</td>
      <td>24</td>
      <td>21</td>
      <td>22</td>
    </tr>
    <tr>
      <th>15</th>
      <td>24</td>
      <td>19</td>
      <td>21</td>
      <td>19</td>
      <td>21</td>
      <td>20</td>
      <td>15</td>
      <td>24</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>20</th>
      <td>23</td>
      <td>18</td>
      <td>20</td>
      <td>19</td>
      <td>20</td>
      <td>19</td>
      <td>15</td>
      <td>23</td>
      <td>19</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
for column in drug_data3_pivot_df.columns:
    drug_data3_pivot_df[column+' Percent'] = (drug_data3_pivot_df[column]/drug_data3_pivot_df[column][0])*100

drug_data3_pivot_df
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Ceftamin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Naftisol</th>
      <th>Placebo</th>
      <th>Propriva</th>
      <th>Ramicane</th>
      <th>Stelasyn</th>
      <th>Zoniferol</th>
      <th>Capomulin Percent</th>
      <th>Ceftamin Percent</th>
      <th>Infubinol Percent</th>
      <th>Ketapril Percent</th>
      <th>Naftisol Percent</th>
      <th>Placebo Percent</th>
      <th>Propriva Percent</th>
      <th>Ramicane Percent</th>
      <th>Stelasyn Percent</th>
      <th>Zoniferol Percent</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>0</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>24</td>
      <td>25</td>
      <td>24</td>
      <td>25</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>21</td>
      <td>25</td>
      <td>23</td>
      <td>23</td>
      <td>24</td>
      <td>23</td>
      <td>25</td>
      <td>23</td>
      <td>24</td>
      <td>100.0</td>
      <td>84.0</td>
      <td>100.0</td>
      <td>92.0</td>
      <td>92.0</td>
      <td>96.0</td>
      <td>95.833333</td>
      <td>100.0</td>
      <td>95.833333</td>
      <td>96.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>25</td>
      <td>20</td>
      <td>21</td>
      <td>22</td>
      <td>21</td>
      <td>24</td>
      <td>21</td>
      <td>24</td>
      <td>21</td>
      <td>22</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>84.0</td>
      <td>88.0</td>
      <td>84.0</td>
      <td>96.0</td>
      <td>87.500000</td>
      <td>96.0</td>
      <td>87.500000</td>
      <td>88.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>24</td>
      <td>19</td>
      <td>21</td>
      <td>19</td>
      <td>21</td>
      <td>20</td>
      <td>15</td>
      <td>24</td>
      <td>21</td>
      <td>21</td>
      <td>96.0</td>
      <td>76.0</td>
      <td>84.0</td>
      <td>76.0</td>
      <td>84.0</td>
      <td>80.0</td>
      <td>62.500000</td>
      <td>96.0</td>
      <td>87.500000</td>
      <td>84.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>23</td>
      <td>18</td>
      <td>20</td>
      <td>19</td>
      <td>20</td>
      <td>19</td>
      <td>15</td>
      <td>23</td>
      <td>19</td>
      <td>17</td>
      <td>92.0</td>
      <td>72.0</td>
      <td>80.0</td>
      <td>76.0</td>
      <td>80.0</td>
      <td>76.0</td>
      <td>62.500000</td>
      <td>92.0</td>
      <td>79.166667</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>22</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>18</td>
      <td>17</td>
      <td>13</td>
      <td>23</td>
      <td>18</td>
      <td>16</td>
      <td>88.0</td>
      <td>72.0</td>
      <td>72.0</td>
      <td>76.0</td>
      <td>72.0</td>
      <td>68.0</td>
      <td>54.166667</td>
      <td>92.0</td>
      <td>75.000000</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>22</td>
      <td>16</td>
      <td>17</td>
      <td>18</td>
      <td>15</td>
      <td>15</td>
      <td>12</td>
      <td>23</td>
      <td>17</td>
      <td>15</td>
      <td>88.0</td>
      <td>64.0</td>
      <td>68.0</td>
      <td>72.0</td>
      <td>60.0</td>
      <td>60.0</td>
      <td>50.000000</td>
      <td>92.0</td>
      <td>70.833333</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>22</td>
      <td>14</td>
      <td>12</td>
      <td>17</td>
      <td>15</td>
      <td>14</td>
      <td>9</td>
      <td>21</td>
      <td>15</td>
      <td>14</td>
      <td>88.0</td>
      <td>56.0</td>
      <td>48.0</td>
      <td>68.0</td>
      <td>60.0</td>
      <td>56.0</td>
      <td>37.500000</td>
      <td>84.0</td>
      <td>62.500000</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>21</td>
      <td>14</td>
      <td>10</td>
      <td>15</td>
      <td>15</td>
      <td>12</td>
      <td>9</td>
      <td>20</td>
      <td>12</td>
      <td>14</td>
      <td>84.0</td>
      <td>56.0</td>
      <td>40.0</td>
      <td>60.0</td>
      <td>60.0</td>
      <td>48.0</td>
      <td>37.500000</td>
      <td>80.0</td>
      <td>50.000000</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>21</td>
      <td>13</td>
      <td>9</td>
      <td>11</td>
      <td>13</td>
      <td>11</td>
      <td>7</td>
      <td>20</td>
      <td>11</td>
      <td>14</td>
      <td>84.0</td>
      <td>52.0</td>
      <td>36.0</td>
      <td>44.0</td>
      <td>52.0</td>
      <td>44.0</td>
      <td>29.166667</td>
      <td>80.0</td>
      <td>45.833333</td>
      <td>56.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Make the Scatter Plot
fig = plt.figure()
ax = fig.add_subplot(111)

# Line plot
pl1, = plt.plot(drug_data3_pivot_df.index, drug_data3_pivot_df['Capomulin Percent'], '.r--', linewidth=0.5, dashes=(17,10))
pl2, = plt.plot(drug_data3_pivot_df.index, drug_data3_pivot_df['Infubinol Percent'], '.b--', linewidth=0.5, dashes=(17,10))
pl3, = plt.plot(drug_data3_pivot_df.index, drug_data3_pivot_df['Ketapril Percent'], '.g--', linewidth=0.5, dashes=(17,10))
pl4, = plt.plot(drug_data3_pivot_df.index, drug_data3_pivot_df['Placebo Percent'], '.--', linewidth=0.5, color='black', 
                dashes=(17,10))

# Scatter Plot
ps1 = plt.scatter(x=drug_data3_pivot_df.index, y=drug_data3_pivot_df['Capomulin Percent'], marker="o", facecolors="red", 
                  edgecolors="black", alpha=0.75)
ps2 = plt.scatter(x=drug_data3_pivot_df.index, y=drug_data3_pivot_df['Infubinol Percent'], marker="^", facecolors="blue", 
                  edgecolors="black", alpha=0.75)
ps3 = plt.scatter(x=drug_data3_pivot_df.index, y=drug_data3_pivot_df['Ketapril Percent'], marker="s", facecolors="green", 
                  edgecolors="black", alpha=0.75)
ps4 = plt.scatter(x=drug_data3_pivot_df.index, y=drug_data3_pivot_df['Placebo Percent'], marker="D", facecolors="black", 
                  edgecolors="black", alpha=0.75)

# Format Legend
legend_df = plt.legend(((pl1,ps1),(pl2,ps2),(pl3,ps3),(pl4,ps4)), 
                       ('Capomulin','Infubinol','Ketapril','Placebo'), loc='best', title='Drug Type', 
                       frameon=True)
legend_df.get_frame().set_edgecolor('black')

# Format Grid
plt.grid(linestyle = '--', dashes=(5,6))

# Set the axis limits
plt.xlim(0,drug_data3_pivot_df.index.max()+5)
plt.ylim(30,drug_data3_pivot_df['Infubinol Percent'].max()+10)

# Adjust Chart Size
plt.rcParams["figure.figsize"] = [8,8]

# Make x-axis, y-axis & title labels
ax.set_title("Survival During Treatment", fontsize=15)
ax.set_xlabel("Time (Days)", fontsize=12)
ax.set_ylabel("Survival Rate (%)", fontsize=12)

plt.show()
```


![png](output_19_0.png)


### Creating a bar graph that compares the total % tumor volume change for each drug across the full 45 days.
#### Summary Bar Graphs


```python
# Get the drug data grouped by drug and timepoint for mean of tumor volume
#drug_data_pivot_df
```


```python
# Calculate the percent chnage in tumor volume for all selected drugs
tumor_change = {}
selected_columns = ['Capomulin', 'Infubinol', 'Ketapril', 'Placebo']
for column in drug_data_pivot_df.columns:
    if column in selected_columns:
        start_value = drug_data_pivot_df[column][0]
        end_value = drug_data_pivot_df[column][45]
        tumor_change[column] = ((end_value-start_value)/start_value)*100
        
tumor_change_df = pd.Series(tumor_change).reset_index()
tumor_change_df = tumor_change_df.rename(columns = {'index':'Drug', 0:'Tumor Percent Change'})
tumor_change_df = tumor_change_df.round(2)
tumor_change_df
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
      <th>Drug</th>
      <th>Tumor Percent Change</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Capomulin</td>
      <td>-19.48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Infubinol</td>
      <td>46.12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ketapril</td>
      <td>57.03</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Placebo</td>
      <td>51.30</td>
    </tr>
  </tbody>
</table>
</div>




```python
# make the Bar Chart
fig = plt.figure()
ax = fig.add_subplot(111)

bars = plt.bar(tumor_change_df['Drug'], tumor_change_df['Tumor Percent Change'], color='red', alpha=0.5, width=1,
       edgecolor='black')

# Format Grid
plt.grid(linestyle = '--', dashes=(5,6))

# Set the axis limits
plt.ylim(-30,70)

# Make x-axis, y-axis & title labels
ax.set_title("Tumor Change Over 45 Days Treatment", fontsize=15)
ax.set_xlabel("Drugs Administered", fontsize=12)
ax.set_ylabel("% Tumor Volume Change", fontsize=12)

# Set bar color per performance
bars = bars[0].set_color('g')

# Print tumor percent change values on individual Bars
rects = ax.patches

# For each bar: Place a label
for rect in rects:
    # Get X and Y placement of label from rect
    y_value = rect.get_height()
    x_value = rect.get_x() + rect.get_width() / 2

    # Number of points between bar and label. Change to your liking.
    space = 5
    # Vertical alignment for positive values
    va = 'bottom'

    # If value of bar is negative: Place label below bar
    if y_value < 0:
        y_value = rect.get_height()
        space *= -1
        # Vertically align label at top
        va = 'top'

    # Use Y value as label and format number with one decimal place
    label = "{:.1f}%".format(y_value)

    # Create annotation
    plt.annotate(
        label,                      # Use `label` as label
        (x_value, y_value),         # Place label at end of the bar
        xytext=(0, space),          # Vertically shift label by `space`
        textcoords="offset points", # Interpret `xytext` as offset in points
        ha='center',                # Horizontally center label
        va=va,
        color='black') 

# Format text in the chart
plt.rcParams.update({'font.size': 15})

plt.show()
```


![png](output_23_0.png)


