# Handling Missing Values in Pandas

In real life, many datasets arrive with missing data either because it exists and was not collected or it never existed.

In Pandas missing data is represented by three values:

* `None`: It is a `keyword` that refers to empty or none.
* `NaN`: Acronym for `Not a Number`.
* `NaT`: Acronym for `Not a Time`.

There are several useful functions for detecting, removing, and replacing null values in Pandas DataFrame:

1. `isnull()`:          Detect missing values for an array-like object. This function takes a scalar or array-like object and indicates whether values are missing (`NaN` in numeric arrays, `None` or `NaN` in object arrays, `NaT` in date-timelike). This function returns `True` if the value is missing.
2. `notnull()`:          It detects values that are not missing. Unlike `isnull()`, this function returns `True` if the value is **not** missing.
3. `dropna()`:          This function is used to remove the null values present in our dataset. 
4. `fillna()`:          This method replaces the null values with a specified value.
5. `replace()`:          This method replaces a specified value present in our dataset with another specified value.

We will learn more about these functions/methods in the following sections.

## 1. Checking for missing values using `isnull()` and `notnull()`

Let's import pandas and our fancy car-sales dataset having some missing values.

```python
import pandas as pd

car_sales_missing_df = pd.read_csv("Datasets/car-sales-missing-data.csv")
print(car_sales_missing_df)
```

         Make Colour  Odometer  Doors    Price
    0  Toyota  White  150043.0    4.0   $4,000
    1   Honda    Red   87899.0    4.0   $5,000
    2  Toyota   Blue       NaN    3.0   $7,000
    3     BMW  Black   11179.0    5.0  $22,000
    4  Nissan  White  213095.0    4.0   $3,500
    5  Toyota  Green       NaN    4.0   $4,500
    6   Honda    NaN       NaN    4.0   $7,500
    7   Honda   Blue       NaN    4.0      NaN
    8  Toyota  White   60000.0    NaN      NaN
    9     NaN  White   31600.0    4.0   $9,700
    

#### Implementing `isnull()`:-
Syntax : 
```python
print(dataset_name.isnull())
```

Example :-
```python

print(car_sales_missing_df.isnull())
```

        Make  Colour  Odometer  Doors  Price
    0  False   False     False  False  False
    1  False   False     False  False  False
    2  False   False      True  False  False
    3  False   False     False  False  False
    4  False   False     False  False  False
    5  False   False      True  False  False
    6  False    True      True  False  False
    7  False   False      True  False   True
    8  False   False     False   True   True
    9   True   False     False  False  False
    

**Note:** In the output, for each numerical value in our dataset, the output has `True` or `False` as the values. Here,
* `True` represents that `NaN` value is present.
* `False` represents that no `Nan` value is present.

To find the number of missing values in each column, we use `isnull().sum()`.


```python
print(car_sales_missing_df.isnull().sum())
```

    Make        1
    Colour      1
    Odometer    4
    Doors       1
    Price       2
    dtype: int64
    

We can also check the presence of null values in a single column in the following manner:-
Syntax : 
```python
print(dataset_name["Column_name"].isnull())
```

Example :-
```python
print(car_sales_missing_df["Odometer"].isnull())
```

    0    False
    1    False
    2     True
    3    False
    4    False
    5     True
    6     True
    7     True
    8    False
    9    False
    Name: Odometer, dtype: bool
    

#### Implementing `notnull()`:-
Syntax : 
```python
print(dataset_name.notnull())
```

Example :-
```python

print(car_sales_missing_df.notnull())
```

        Make  Colour  Odometer  Doors  Price
    0   True    True      True   True   True
    1   True    True      True   True   True
    2   True    True     False   True   True
    3   True    True      True   True   True
    4   True    True      True   True   True
    5   True    True     False   True   True
    6   True   False     False   True   True
    7   True    True     False   True  False
    8   True    True      True  False  False
    9  False    True      True   True   True

    
**Note:** In the output, for each numerical value in our dataset, the output has `True` or `False` as the values. Here,
* `True` represents that no `NaN` value is present.
* `False` represents that `Nan` value is present.


## 2. Filling missing values using `fillna()`and `replace()`.
#### Implementing `fillna()`:-
Syntax : 
```python
print(dataset_name.fillna(value_to_replace_null))
```

Example :-
```python
## Filling missing values  with a single value using `fillna`
print(car_sales_missing_df.fillna(0))
```

         Make Colour  Odometer  Doors    Price
    0  Toyota  White  150043.0    4.0   $4,000
    1   Honda    Red   87899.0    4.0   $5,000
    2  Toyota   Blue       0.0    3.0   $7,000
    3     BMW  Black   11179.0    5.0  $22,000
    4  Nissan  White  213095.0    4.0   $3,500
    5  Toyota  Green       0.0    4.0   $4,500
    6   Honda      0       0.0    4.0   $7,500
    7   Honda   Blue       0.0    4.0        0
    8  Toyota  White   60000.0    0.0        0
    9       0  White   31600.0    4.0   $9,700
    

_There is a method called `ffill()`. This method replaces the null values with the preceding value in that column. It is used in the following manner:-_
```python
print(car_sales_missing_df.ffill())
```

         Make Colour  Odometer  Doors    Price
    0  Toyota  White  150043.0    4.0   $4,000
    1   Honda    Red   87899.0    4.0   $5,000
    2  Toyota   Blue   87899.0    3.0   $7,000
    3     BMW  Black   11179.0    5.0  $22,000
    4  Nissan  White  213095.0    4.0   $3,500
    5  Toyota  Green  213095.0    4.0   $4,500
    6   Honda  Green  213095.0    4.0   $7,500
    7   Honda   Blue  213095.0    4.0   $7,500
    8  Toyota  White   60000.0    4.0   $7,500
    9  Toyota  White   31600.0    4.0   $9,700
    

_There is another method called `bfill()`. This method replaces the null values with the succeeding value in that column. It is used in the following manner:-_
```python
print(car_sales_missing_df.bfill())
```

         Make Colour  Odometer  Doors    Price
    0  Toyota  White  150043.0    4.0   $4,000
    1   Honda    Red   87899.0    4.0   $5,000
    2  Toyota   Blue   11179.0    3.0   $7,000
    3     BMW  Black   11179.0    5.0  $22,000
    4  Nissan  White  213095.0    4.0   $3,500
    5  Toyota  Green   60000.0    4.0   $4,500
    6   Honda   Blue   60000.0    4.0   $7,500
    7   Honda   Blue   60000.0    4.0   $9,700
    8  Toyota  White   60000.0    4.0   $9,700
    9     NaN  White   31600.0    4.0   $9,700
    

#### Implementing `replace()` method
To use this method, it is must to use Numpy library.
Syntax (To replace NaN values with a specified value) : 
```python
print(dataset_name.replace(to_replace=np.nan, value=value_to_replace_null))
```
Example:-
We want to replace all `NaN` values in the data frame with the value -125.
This can be achieved using the `replace()` function as shown in the code below.

```python
import numpy as np

print(car_sales_missing_df.replace(to_replace = np.nan, value = -125))
```

         Make Colour  Odometer  Doors    Price
    0  Toyota  White  150043.0    4.0   $4,000
    1   Honda    Red   87899.0    4.0   $5,000
    2  Toyota   Blue    -125.0    3.0   $7,000
    3     BMW  Black   11179.0    5.0  $22,000
    4  Nissan  White  213095.0    4.0   $3,500
    5  Toyota  Green    -125.0    4.0   $4,500
    6   Honda   -125    -125.0    4.0   $7,500
    7   Honda   Blue    -125.0    4.0     -125
    8  Toyota  White   60000.0 -125.0     -125
    9    -125  White   31600.0    4.0   $9,700
    

## 3. Dropping missing values using `dropna()`

To drop null values from a dataframe, we use `dropna()` function. This function drops Rows/Columns of datasets with Null values in the following ways:-

#### Dropping rows with at least 1 null value:-


```python
print(car_sales_missing_df.dropna(axis = 0))  ##Now we drop rows with at least one Nan value (Null value) 
```

         Make Colour  Odometer  Doors    Price
    0  Toyota  White  150043.0    4.0   $4,000
    1   Honda    Red   87899.0    4.0   $5,000
    3     BMW  Black   11179.0    5.0  $22,000
    4  Nissan  White  213095.0    4.0   $3,500
    

#### Dropping rows if all values in that row are missing:-


```python
print(car_sales_missing_df.dropna(how = 'all',axis = 0))  ## If not have leave the row as it is
```

         Make Colour  Odometer  Doors    Price
    0  Toyota  White  150043.0    4.0   $4,000
    1   Honda    Red   87899.0    4.0   $5,000
    2  Toyota   Blue       NaN    3.0   $7,000
    3     BMW  Black   11179.0    5.0  $22,000
    4  Nissan  White  213095.0    4.0   $3,500
    5  Toyota  Green       NaN    4.0   $4,500
    6   Honda    NaN       NaN    4.0   $7,500
    7   Honda   Blue       NaN    4.0      NaN
    8  Toyota  White   60000.0    NaN      NaN
    9     NaN  White   31600.0    4.0   $9,700
    

#### Dropping columns with at least 1 null value:-


```python
print(car_sales_missing_df.dropna(axis = 1))
```

    Empty DataFrame
    Columns: []
    Index: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    

Now we drop columns that have at least 1 missing value.

Here the dataset becomes empty after `dropna()` because each column has at least 1 null value, so, it removes those columns resulting in an empty dataframe.
