## import
```python
import pandas as pd
```

## reading data
### read_csv(file_path)
```python
data = pd.read_csv(file_path)
```

### Options:
* index_col: 
### describe()
```python
data.describe()
```
#### [Interpreting Data Description](https://www.kaggle.com/code/dansbecker/basic-data-exploration#Interpreting-Data-Description)
The results show 8 numbers for each column in your original dataset. The first number, the **count**, shows how many rows have non-missing values.

Missing values arise for many reasons. For example, the size of the 2nd bedroom wouldn't be collected when surveying a 1 bedroom house. We'll come back to the topic of missing data.

The second value is the **mean**, which is the average. Under that, **std** is the standard deviation, which measures how numerically spread out the values are.

To interpret the **min**, **25%**, **50%**, **75%** and **max** values, imagine sorting each col umn from lowest to highest value. The first (smallest) value is the min. If you go a quarter way through the list, you'll find a number that is bigger than 25% of the values and smaller than 75% of the values. That is the **25%** value (pronounced "25th percentile"). The 50th and 75th percentiles are defined analogously, and the **max** is the largest number.

### head()

### info()

### sample()
Ähnlich wie head(), aber über den gesamten Datensatz.  
### Parameter
* n: Größe der Sample
* frac: Prozentuale Größe von dem Sample
* random_state: Seed für den Zufallsgenerator; Sorgt für die Reproduziertbarkeit
### Beispiele
7 Datensätze:

```python
data.sample(n=7, random_state=42)
```
2% der Datensätze:
```python
data.sample(frac=0.02, random_state=42)
```

### set_index()
data.set_index("new_index")

### describe()

### mean()
### unique()
### value_counts()
### map()
Returns new data set!
### apply()
Returns new data set!
```python
def callback(row_data):
	...
	return row_data

data_new = data.apply(callback, axis='columns/index')
```
### shape
```python
data.shape
```
Output: 
```
(<# of rows>, <# of columns>)
```

### (i)loc 
#### Index-based selection[](https://www.kaggle.com/code/residentmario/indexing-selecting-assigning#Index-based-selection)
Both `loc` and `iloc` are row-first, column-second. This is the opposite of what we do in native Python, which is column-first, row-second.

In case of doubt switch between both configurations.

loc: Like native Python \[0;10 \] => 0, ..., 9
iloc: \[0;10\] => 0, ..., 10
```python
data.loc[[colums],[rows]]
```
#### Beispiele:
First row of data in a DataFrame
```python
reviews.iloc[0]
```
First colum of data in a DataFrame
```python
reviews.iloc[:, 0]
```


### rename()
Column:
```python
data.rename(columns={'old': 'new'})
```
Row:
```python
data.rename(index={'old': 'new'})
```

### concat() 
### join() 
left 
right
```python
left.join(right, lsuffix=\'\_CAN\', rsuffix=\'\_UK\')
```
### merge()
## Data Formats

### DataFrame

#### Selection
```python
data.(i)loc[(data.var1 == "Text") | ((data.var2 > 5) & (data.var2 < 1000))]
```
| == or
& == and

##### isin()
```python
data.(i)loc[data.var1.isin([crit1, crit2])]
```
##### isnotnull()

#### Assigning
```python
data['new_text'] = 'everyone'
data['inversed_index'] = range(len(data), 0, -1)
```
### Series
```python

```

#### Options
* name: String
* 
