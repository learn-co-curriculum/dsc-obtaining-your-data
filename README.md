
## Obtaining Our Data

## Introduction
In this lesson, we'll sythesize many of our data loading skills to date in order to merge multiple datasets from various sources.

## Objectives
You will be able to:
* Understand the ETL process and the steps it consists of
* Understand the challenges of working with data from multiple sources 

## Loading SQL DB to DataFrames
<img src="Database-Schema.png">


```python
import sqlite3
import pandas as pd
```


```python
#Create a connection
con = sqlite3.connect('data.sqlite')
#Create a cursor
cur = con.cursor()
#Select some data
cur.execute("""select * from products;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(df.shape)
df.head()
```

    (110, 9)





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
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S10_1949</td>
      <td>1952 Alpine Renault 1300</td>
      <td>Classic Cars</td>
      <td>1:10</td>
      <td>Classic Metal Creations</td>
      <td>Turnable front wheels; steering function; deta...</td>
      <td>7305</td>
      <td>98.58</td>
      <td>214.30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S10_2016</td>
      <td>1996 Moto Guzzi 1100i</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Highway 66 Mini Classics</td>
      <td>Official Moto Guzzi logos and insignias, saddl...</td>
      <td>6625</td>
      <td>68.99</td>
      <td>118.94</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S10_4698</td>
      <td>2003 Harley-Davidson Eagle Drag Bike</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Red Start Diecast</td>
      <td>Model features, official Harley Davidson logos...</td>
      <td>5582</td>
      <td>91.02</td>
      <td>193.66</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S10_4757</td>
      <td>1972 Alfa Romeo GTA</td>
      <td>Classic Cars</td>
      <td>1:10</td>
      <td>Motor City Art Classics</td>
      <td>Features include: Turnable front wheels; steer...</td>
      <td>3252</td>
      <td>85.68</td>
      <td>136.00</td>
    </tr>
  </tbody>
</table>
</div>



## Merging Data

Recall that we can also join data from multiple tables in sql.


```python
#Create a connection
con = sqlite3.connect('data.sqlite')
#Create a cursor
cur = con.cursor()
#Select some data
cur.execute("""select * from products
                        join orderdetails
                        using (productCode);""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(df.shape)
df.head()
```

    (2996, 13)





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
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
      <th>orderNumber</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
      <td>10107</td>
      <td>30</td>
      <td>81.35</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
      <td>10121</td>
      <td>34</td>
      <td>86.13</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
      <td>10134</td>
      <td>41</td>
      <td>90.92</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
      <td>10145</td>
      <td>45</td>
      <td>76.56</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
      <td>10159</td>
      <td>49</td>
      <td>81.35</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>



We can also merge data from a seperate csv file. For example, say we take a seperate data source regarding weather for various cities across various dates in order to compare what impact weather is having on our product sales. We could first generate a view from our database?


```python
#Create a connection
con = sqlite3.connect('data.sqlite')
#Create a cursor
cur = con.cursor()
#Select some data
cur.execute("""select * from customers
                        join orders
                        using(customerNumber);""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(df.shape)
df.head()
```

    (326, 19)





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
      <th>customerNumber</th>
      <th>customerName</th>
      <th>contactLastName</th>
      <th>contactFirstName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>addressLine2</th>
      <th>city</th>
      <th>state</th>
      <th>postalCode</th>
      <th>country</th>
      <th>salesRepEmployeeNumber</th>
      <th>creditLimit</th>
      <th>orderNumber</th>
      <th>orderDate</th>
      <th>requiredDate</th>
      <th>shippedDate</th>
      <th>status</th>
      <th>comments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>103</td>
      <td>Atelier graphique</td>
      <td>Schmitt</td>
      <td>Carine</td>
      <td>40.32.2555</td>
      <td>54, rue Royale</td>
      <td></td>
      <td>Nantes</td>
      <td></td>
      <td>44000</td>
      <td>France</td>
      <td>1370</td>
      <td>21000.00</td>
      <td>10123</td>
      <td>2003-05-20</td>
      <td>2003-05-29</td>
      <td>2003-05-22</td>
      <td>Shipped</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>103</td>
      <td>Atelier graphique</td>
      <td>Schmitt</td>
      <td>Carine</td>
      <td>40.32.2555</td>
      <td>54, rue Royale</td>
      <td></td>
      <td>Nantes</td>
      <td></td>
      <td>44000</td>
      <td>France</td>
      <td>1370</td>
      <td>21000.00</td>
      <td>10298</td>
      <td>2004-09-27</td>
      <td>2004-10-05</td>
      <td>2004-10-01</td>
      <td>Shipped</td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>103</td>
      <td>Atelier graphique</td>
      <td>Schmitt</td>
      <td>Carine</td>
      <td>40.32.2555</td>
      <td>54, rue Royale</td>
      <td></td>
      <td>Nantes</td>
      <td></td>
      <td>44000</td>
      <td>France</td>
      <td>1370</td>
      <td>21000.00</td>
      <td>10345</td>
      <td>2004-11-25</td>
      <td>2004-12-01</td>
      <td>2004-11-26</td>
      <td>Shipped</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>112</td>
      <td>Signal Gift Stores</td>
      <td>King</td>
      <td>Jean</td>
      <td>7025551838</td>
      <td>8489 Strong St.</td>
      <td></td>
      <td>Las Vegas</td>
      <td>NV</td>
      <td>83030</td>
      <td>USA</td>
      <td>1166</td>
      <td>71800.00</td>
      <td>10124</td>
      <td>2003-05-21</td>
      <td>2003-05-29</td>
      <td>2003-05-25</td>
      <td>Shipped</td>
      <td>Customer very concerned about the exact color ...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>112</td>
      <td>Signal Gift Stores</td>
      <td>King</td>
      <td>Jean</td>
      <td>7025551838</td>
      <td>8489 Strong St.</td>
      <td></td>
      <td>Las Vegas</td>
      <td>NV</td>
      <td>83030</td>
      <td>USA</td>
      <td>1166</td>
      <td>71800.00</td>
      <td>10278</td>
      <td>2004-08-06</td>
      <td>2004-08-16</td>
      <td>2004-08-09</td>
      <td>Shipped</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



And then load a datafile regarding weather for these cities and dates:


```python
#Insert weather file once generated
weather = pd.read_csv()
weather.head()
```


```python
merged = pd.merge(df, weather)
```

## Checking Merged Data

It's always good practice to check assumptions and preview transformed data views throughout your process. Let's take a look:


```python
merged.head()
```

Whoa!You might notice that we ended up with two date fields. This is problematic and indicates that our join had some unintentional consequences. We intended for our data files to be merged on City and Date fields. Unlike SQL joins , the built in Pandas merge method conveniently uses common column names between the dataframes. Unfortunately, columns that are not identically named beforehand will not work with this convenience method. Additionally, it is imperitive to check the formatting of the join keys between the tables. A number formatted as a string can often ruin joins, and seperate formatting conventions such as 'U.S.' versus 'USA' are also important preprocessing considerations before merging data files from various sources. With that, let's retrace our steps in a more diligent and thoughtful manner. To start, let's preview the table schemas we are about to join:


```python
df.info()
```


```python
weather.info()
```

Roughly we're probably looking to join on:
* city
* state
* country
* date

Important considerations to successfully making a meaningful join will then be ensuring that the formatting and naming conventions of these fields is identical (or as close as reasonably possible) between the two files. Depending on the size of the files, and the complexity of the data, this can be a grueling process. Let's investigate just a bit further:

* For each dataframe, what are the top values for each field we'll be joining on?
* Do these have identical matches between tables?
* Create a mapping between seperate formatting conventions


```python

```


```python
Ou
```


```python
## Objective 3 content
```

## Preprocessing for Merges


```python

```

## Saving Transformed Data to File


```python
df.to_csv('')
```

## Summary
Well done! In this lesson we review merges, as well as potential pitfalls in merging datasets from different sources. In the next lab, you'll get some practice doing this as an initial step to a regression task.
