
###### Heroes Of Pymoli Data Analysis
* Of the 576 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

* The peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).  



### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
file_to_load = "Resources/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)
```

## Player Count

* Display the total number of players



```python
#Get the total number of players
total_players = len(purchase_data["SN"].unique())
print (f"Total number of players is {total_players}.")
```

    Total number of players is 576.


## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
#Get number of unique items, average purchase price, number of purchases, total revenue
unique_items = len(purchase_data["Item ID"].unique())
average_purchase_price = purchase_data["Price"].mean()
total_purchases = len(purchase_data["Purchase ID"].unique())
total_revenue = purchase_data["Price"].sum()
purchasing_analysis = pd.DataFrame({"Number of Unique Items ": [unique_items],
                                  "Average Purchase Price": [average_purchase_price],
                                  "Total Number of Purchases": [total_purchases],
                                  "Total Revenue": [total_revenue]})
purchasing_analysis["Average Purchase Price"]=purchasing_analysis["Average Purchase Price"].map("${:.2f}".format)
purchasing_analysis["Total Revenue"]=purchasing_analysis["Total Revenue"].map("${:,.2f}".format)
purchasing_analysis
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed




```python
#Get the number of players for each gender 
gender_group = purchase_data.groupby(["Gender"])
gender_counts = gender_group["SN"].nunique()
sum_gender_count = gender_counts.sum()

#Get the percentage and count of female players
female_count = gender_counts["Female"]
female_percent = (female_count/sum_gender_count) * 100

#Get the percentage and count of male players
male_count = gender_counts["Male"]
male_percent = (male_count/sum_gender_count) * 100

#Get the percentage and count of players of other/ nondisclosed gender
other_count = gender_counts["Other / Non-Disclosed"]
other_percent = (other_count/sum_gender_count) * 100

#Put data into a DataFrame and format
gender_demo_df = pd.DataFrame(gender_counts)
gender_demo_df["Total Count"] = [female_count, male_count, other_count]
gender_demo_df['Percentage of Players'] = [female_percent, male_percent, other_percent]
gender_demo_df['Percentage of Players'] = gender_demo_df['Percentage of Players'].map("{:.2f}".format)
del gender_demo_df["SN"]

#Display the DataFrame
gender_demo_df = gender_demo_df.sort_values("Percentage of Players",ascending=False)
gender_demo_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>484</td>
      <td>84.03</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>14.06</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
#Get purchase count, average purchase price, total purchase value by gender
purchase_count = gender_group["Purchase ID"].count()
avg_purchase_price = gender_group["Price"].mean()
total_purchase_value = gender_group["Price"].sum()

#Get average purchase total per person by gender
avg_price_pp_gender = total_purchase_value/gender_counts

#Create dataframe and display summary
gender_summary_df = pd.DataFrame(purchase_count)
gender_summary_df = gender_summary_df.rename(columns={"Purchase ID": "Purchase Count"})
gender_summary_df["Average Purchase Price"] = avg_purchase_price
gender_summary_df["Average Purchase Price"] = gender_summary_df["Average Purchase Price"].map("${:,.2f}".format)
gender_summary_df["Total Purchase Value"] = total_purchase_value
gender_summary_df["Total Purchase Value"] = gender_summary_df["Total Purchase Value"].map("${:,.2f}".format)
gender_summary_df["Avg Total Purchase per Person"] = avg_price_pp_gender
gender_summary_df["Avg Total Purchase per Person"] = gender_summary_df["Avg Total Purchase per Person"].map("${:,.2f}".format)
gender_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
#Categorize age groups
bins = [0, 9, 14, 19, 24, 29, 34, 39, 1000]
group_labels=["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
purchase_data["Age Group"] = pd.cut(purchase_data["Age"], bins, labels=group_labels)

#Get the count and percentage of players by age group
age_group = purchase_data.groupby(["Age Group"])
age_counts = age_group["SN"].nunique()
age_group_percent = (age_counts/total_players)*100

age_demo_summary_df = pd.DataFrame(age_counts)
age_demo_summary_df = age_demo_summary_df.rename(columns={"SN":"Total Count"})
age_demo_summary_df["Percentage of Players"] = age_group_percent
age_demo_summary_df["Percentage of Players"] = age_demo_summary_df["Percentage of Players"].map("{:,.2f}".format)
age_demo_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
#Get the purchase count, average purchase price, total purchase value by age group
purchase_count_age_group = age_group["Purchase ID"].nunique()
avg_purchase_price_age_group = age_group["Price"].mean()
purchase_value_age_group = age_group["Price"].sum()

#Get the average purchase value per person by age group 
avg_price_pp_age_group = purchase_value_age_group/age_counts

#Create dataframe and display summary
agegroup_summary_df = pd.DataFrame(purchase_count_age_group)
agegroup_summary_df = agegroup_summary_df.rename(columns={"Purchase ID": "Purchase Count"})
agegroup_summary_df["Average Purchase Price"] = avg_purchase_price_age_group
agegroup_summary_df["Average Purchase Price"] = agegroup_summary_df["Average Purchase Price"].map("${:,.2f}".format)
agegroup_summary_df["Total Purchase Value"] = purchase_value_age_group
agegroup_summary_df["Total Purchase Value"] = agegroup_summary_df["Total Purchase Value"].map("${:,.2f}".format)
agegroup_summary_df["Avg Total Purchase per Person"] = avg_price_pp_age_group
agegroup_summary_df["Avg Total Purchase per Person"] = agegroup_summary_df["Avg Total Purchase per Person"].map("${:,.2f}".format)
agegroup_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
#Get the Purchase Count, average purchae price, total purchase value
total_purchase_by_player = purchase_data.groupby("SN")
amt_spent_by_player = total_purchase_by_player["Price"].sum()
num_of_purchase_by_player = total_purchase_by_player["Purchase ID"].count()
avg_purchase_price = amt_spent_by_player/num_of_purchase_by_player

#Put the results into a df
player_spending_df = pd.DataFrame(amt_spent_by_player)
player_spending_df = player_spending_df.rename(columns={"Price":"Total Purchase Value"})
player_spending_df["Purchase Count"] = num_of_purchase_by_player
player_spending_df["Average Purchase Price"]=avg_purchase_price
player_spending_df["Average Purchase Price"]=player_spending_df["Average Purchase Price"].map("${:,.2f}".format)
player_spending_df=player_spending_df[["Purchase Count","Average Purchase Price","Total Purchase Value"]]

#Get the Top Five Spenders
player_spending_df = player_spending_df.sort_values("Total Purchase Value", ascending=False)
player_spending_df["Total Purchase Value"]=player_spending_df["Total Purchase Value"].map("${:,.2f}".format)
top_five_spenders = player_spending_df.iloc[0:5,:]
top_five_spenders
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
#Get the Item ID, Item Name, and Item Price columns
item_df = purchase_data[["Item ID", "Item Name", "Price"]]

#Group by Item ID and Item Name
item_group = item_df.groupby(["Item ID", "Item Name"])
purchase_count = item_group.count()
total_purchase_value = item_group.sum()
item_price = total_purchase_value / purchase_count

#Summary Data Frame
item_summary_df = pd.DataFrame(purchase_count)
item_summary_df = item_summary_df.rename(columns={"Price":"Purchase Count"})
item_summary_df["Item Price"] = item_price
item_summary_df["Item Price"] = item_summary_df["Item Price"].map("${:,.2f}".format)
item_summary_df["Total Purchase Value"]=total_purchase_value

#Get the Most Popular Items
pop_item_summary_df = item_summary_df.sort_values("Purchase Count", ascending=False)
pop_item_summary_df["Total Purchase Value"]=pop_item_summary_df["Total Purchase Value"].map("${:,.2f}".format)
top_five_items = pop_item_summary_df.iloc[0:5,:]
top_five_items
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
#Get the Most Profitable Items
prof_item_summary_df = item_summary_df.sort_values("Total Purchase Value", ascending=False)
prof_item_summary_df["Total Purchase Value"]=prof_item_summary_df["Total Purchase Value"].map("${:,.2f}".format)
top_five_most_profitable_items = prof_item_summary_df.iloc[0:5,:]
top_five_most_profitable_items
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <th>Final Critic</th>
      <td>8</td>
      <td>$4.88</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```
