
# 数据清洗

数据清洗是处理数据的第一步.我们的数据一般都是由不同来源汇总得来,因此难免会有

+ 重复值
+ 缺值
+ 异常值和极端值
+ 插值操作

等问题.数据清洗就是处理这些问题,使原始数据成为可用于进一步分析的数据



```python
import pandas as pd
import numpy as np
```


```python
dirty = pd.read_csv("source/dirty.csv",sep=",")
dirty
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.0</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.0</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



## 重复值处理

+ ### 首先我们要观察数据是否有重复值


```python
dirty.duplicated()
```




    0     False
    1     False
    2     False
    3     False
    4      True
    5     False
    6     False
    7     False
    8     False
    9     False
    10    False
    11    False
    12    False
    13    False
    14    False
    15    False
    dtype: bool



从第4行就有重复

+ ### 去除重复


```python
dirty.drop_duplicates()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.0</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.0</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



这两个方法默认会判断全部列，你也可以指定部分列进行重复项判断。比如我们以names作为key，且只根据key列过滤重复项：


```python
dirty.duplicated(["name"])
```




    0     False
    1     False
    2     False
    3     False
    4      True
    5     False
    6      True
    7      True
    8      True
    9     False
    10    False
    11    False
    12    False
    13    False
    14    False
    15    False
    dtype: bool




```python
dirty.drop_duplicates(["name"])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.0</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



duplicated和drop_duplicates默认保留的是第一个出现的值组合。传入`keep='last'`则保留最后一个：


```python
dirty.drop_duplicates(["name"],keep='last')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.0</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.0</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



## 缺值处理

有的时候读出来的数据是有缺值的,所有缺值都会在pandas中表现为numpy.NaN.

+ ### 使用isnull方法查看


```python
dirty.isnull()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>6</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>7</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>8</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>9</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>10</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>11</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>12</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>13</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>14</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>15</th>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
dirty["age"].isnull()
```




    0     False
    1      True
    2     False
    3     False
    4     False
    5     False
    6     False
    7     False
    8     False
    9     False
    10    False
    11    False
    12    False
    13    False
    14    False
    15     True
    Name: age, dtype: bool



+ ### 用dropna()方法来做删除处理

可以设定how="all"来丢弃全部为空的行,

要丢弃列加入参数axis = 1即可


```python
dirty.dropna()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.0</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



+ ### 缺值填充

有时我们并不想删除,而是会希望用其他数据去填充,这时可以用fillna方法


```python
dirty.fillna(0)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>0.0</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>0.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.0</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>0.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.0</td>
      <td>200</td>
      <td>0.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>0.0</td>
      <td>89</td>
      <td>0.0</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



参数同样可以是一个pandas对象,比如用均值填充


```python
dirty.fillna(dirty.mean())
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.000000</td>
      <td>69</td>
      <td>175.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>152.928571</td>
      <td>89</td>
      <td>195.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.000000</td>
      <td>49</td>
      <td>169.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.000000</td>
      <td>79</td>
      <td>184.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.000000</td>
      <td>69</td>
      <td>175.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.000000</td>
      <td>45</td>
      <td>171.083333</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.000000</td>
      <td>56</td>
      <td>176.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.000000</td>
      <td>44</td>
      <td>149.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.000000</td>
      <td>69</td>
      <td>178.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.000000</td>
      <td>48</td>
      <td>164.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.000000</td>
      <td>57</td>
      <td>162.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.000000</td>
      <td>42</td>
      <td>153.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.000000</td>
      <td>300</td>
      <td>171.083333</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.000000</td>
      <td>200</td>
      <td>171.083333</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.000000</td>
      <td>63</td>
      <td>173.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>152.928571</td>
      <td>89</td>
      <td>171.083333</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



fillna方式中参数可以是一个列为key的字典,来实现不同列填入不同数据


```python
dirty.fillna({"age":dirty["age"].mean(),
             "height":dirty["height"].mean()})
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.000000</td>
      <td>69</td>
      <td>175.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>152.928571</td>
      <td>89</td>
      <td>195.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.000000</td>
      <td>49</td>
      <td>169.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.000000</td>
      <td>79</td>
      <td>184.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.000000</td>
      <td>69</td>
      <td>175.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.000000</td>
      <td>45</td>
      <td>171.083333</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.000000</td>
      <td>56</td>
      <td>176.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.000000</td>
      <td>44</td>
      <td>149.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.000000</td>
      <td>69</td>
      <td>178.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.000000</td>
      <td>48</td>
      <td>164.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.000000</td>
      <td>57</td>
      <td>162.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.000000</td>
      <td>42</td>
      <td>153.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.000000</td>
      <td>300</td>
      <td>171.083333</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.000000</td>
      <td>200</td>
      <td>171.083333</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.000000</td>
      <td>63</td>
      <td>173.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>152.928571</td>
      <td>89</td>
      <td>171.083333</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



如果希望向前或向后填补空白,那么可以加入参数`method=<method>`
可以有这些关键字:

+ 'backfill'和 'bfill'
    使用下一个有效的观察来填补前面的缺口
    
+ 'pad'和'ffill' 
    传播最后一个观察到的有效的数据到下一个
    
使用method,如果我们只希望连续的空白填充到一定数量的数据点，我们可以使用`limit=<n:int>`关键字


```python
dirty.fillna(method='bfill',limit=1 )
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>15.0</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>25555.0</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.0</td>
      <td>200</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



+ ### 异常值和极端值

像`25555.0`,`-25555`,还有weight里面的`200`,`300`这些值可能是一个表示缺失数据的标记值,也可能是录入时候出错了,属于异常值.要将其替换为pandas能够理解的NA值，我们可以利用replace来产生一个新的dataframe


```python
dirty.replace(25555.0, np.nan)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>-25555.0</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



如果你希望一次性替换多个值，可以传入一个由待替换值组成的列表以及一个替换值：


```python
dirty.replace([25555.0,-25555.0], np.nan)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>NaN</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



如果希望对不同的值进行不同的替换，则传入一个由替换关系组成的列表即可：


```python
dirty.replace([25555.0,-25555.0], [np.nan, 0])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>0.0</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



传入的参数也可以是字典：


```python
dirty.replace({25555.0: np.nan, -25555.0: 0})
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>44</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>1977.0</td>
      <td>300</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>0.0</td>
      <td>200</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



更加常见的是针对不同列给出不同的替换方案


```python
nan_dirty = dirty.replace({"age":[25555.0,-25555.0,1977],
              "weight":[300,200]},np.nan)
nan_dirty
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69.0</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>89.0</td>
      <td>195.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.0</td>
      <td>49.0</td>
      <td>169.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.0</td>
      <td>79.0</td>
      <td>184.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.0</td>
      <td>69.0</td>
      <td>175.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.0</td>
      <td>45.0</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.0</td>
      <td>56.0</td>
      <td>176.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>NaN</td>
      <td>44.0</td>
      <td>149.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.0</td>
      <td>69.0</td>
      <td>178.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.0</td>
      <td>48.0</td>
      <td>164.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.0</td>
      <td>57.0</td>
      <td>162.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.0</td>
      <td>42.0</td>
      <td>153.0</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.0</td>
      <td>63.0</td>
      <td>173.0</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>NaN</td>
      <td>89.0</td>
      <td>NaN</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>



更常见的情况是数据非常多,我们无法确定哪些是需要替换的离群点,这种时候我们只能手动的找出是否是离群点


```python
States = ['NY', 'NY', 'NY', 'NY', 'FL', 'FL', 'GA', 'GA', 'FL', 'FL'] 
data = [1.0, 2, 3, 4, 5, 6, 7, 8, 9, 10]
idx = pd.date_range('1/1/2012', periods=10, freq='MS')
df1 = pd.DataFrame(data, index=idx, columns=['Revenue'])
df1['State'] = States
```


```python
df1
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
    </tr>
  </tbody>
</table>
</div>




```python
data2 = [10.0, 10.0, 9, 9, 8, 8, 7, 7, 6, 6]
idx2 = pd.date_range('1/1/2013', periods=10, freq='MS')
df2 = pd.DataFrame(data2, index=idx2, columns=['Revenue'])
df2['State'] = States
df2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.concat([df1,df2])
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
    </tr>
  </tbody>
</table>
</div>



以上就是我们的例子使用的源数据,接着我们来根据他的均值和标准差来判断是不是离群点

+ 方法一

    使用针对整体的统计特点,最终确定是否是离群点


```python
newdf = df.copy()
```


```python
newdf['x-Mean'] = abs(newdf['Revenue'] - newdf['Revenue'].mean())
newdf
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
      <th>x-Mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
      <td>5.75</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
      <td>4.75</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
      <td>3.75</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
      <td>2.75</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
      <td>1.75</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
      <td>1.25</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
      <td>2.25</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>2.25</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>2.25</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.25</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.25</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
newdf['1.96*std'] = 1.96*newdf['Revenue'].std()  
newdf
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
      <th>x-Mean</th>
      <th>1.96*std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
      <td>5.75</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
      <td>4.75</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
      <td>3.75</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
      <td>2.75</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
      <td>1.75</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
      <td>1.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
      <td>2.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
      <td>3.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>3.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>3.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>2.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>2.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>5.200273</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>5.200273</td>
    </tr>
  </tbody>
</table>
</div>




```python
newdf['Outlier'] = abs(newdf['Revenue'] - newdf['Revenue'].mean()) > 1.96*newdf['Revenue'].std()
newdf
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
      <th>x-Mean</th>
      <th>1.96*std</th>
      <th>Outlier</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
      <td>5.75</td>
      <td>5.200273</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
      <td>4.75</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
      <td>3.75</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
      <td>2.75</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
      <td>1.75</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
      <td>1.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
      <td>2.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
      <td>3.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>3.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>3.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>2.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>2.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>5.200273</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



+ 方法二
    
    使用groupby+transform,针对各个组别的统计特点,确定是否是离群点


```python
newdf = df.copy()
```


```python
State = newdf.groupby('State')
State.groups
```




    {'FL': DatetimeIndex(['2012-05-01', '2012-06-01', '2012-09-01', '2012-10-01',
                    '2013-05-01', '2013-06-01', '2013-09-01', '2013-10-01'],
                   dtype='datetime64[ns]', freq=None),
     'GA': DatetimeIndex(['2012-07-01', '2012-08-01', '2013-07-01', '2013-08-01'], dtype='datetime64[ns]', freq=None),
     'NY': DatetimeIndex(['2012-01-01', '2012-02-01', '2012-03-01', '2012-04-01',
                    '2013-01-01', '2013-02-01', '2013-03-01', '2013-04-01'],
                   dtype='datetime64[ns]', freq=None)}




```python
newdf['Outlier'] = State.transform( lambda x: abs(x-x.mean()) > 1.96*x.std() )
newdf
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
      <th>Outlier</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
newdf['x-Mean'] = State.transform( lambda x: abs(x-x.mean()) )
newdf['1.96*std'] = State.transform( lambda x: 1.96*x.std() )
newdf
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
      <th>Outlier</th>
      <th>x-Mean</th>
      <th>1.96*std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
      <td>False</td>
      <td>5.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
      <td>False</td>
      <td>4.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
      <td>False</td>
      <td>3.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
      <td>False</td>
      <td>2.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
      <td>False</td>
      <td>2.25</td>
      <td>3.434996</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>False</td>
      <td>1.25</td>
      <td>3.434996</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>False</td>
      <td>0.25</td>
      <td>0.980000</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
      <td>False</td>
      <td>0.75</td>
      <td>0.980000</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
      <td>False</td>
      <td>1.75</td>
      <td>3.434996</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
      <td>False</td>
      <td>2.75</td>
      <td>3.434996</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>False</td>
      <td>4.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>False</td>
      <td>4.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>False</td>
      <td>3.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>False</td>
      <td>3.00</td>
      <td>7.554813</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>False</td>
      <td>0.75</td>
      <td>3.434996</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>False</td>
      <td>0.75</td>
      <td>3.434996</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>False</td>
      <td>0.25</td>
      <td>0.980000</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>False</td>
      <td>0.25</td>
      <td>0.980000</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>False</td>
      <td>1.25</td>
      <td>3.434996</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>False</td>
      <td>1.25</td>
      <td>3.434996</td>
    </tr>
  </tbody>
</table>
</div>



+ 方法三

    使用Group by item,通过groupby+apply,根据分组的统计特征判断是否是离群点
   


```python
newdf = df.copy()

State = newdf.groupby('State')

def s(group):
    group['x-Mean'] = abs(group['Revenue'] - group['Revenue'].mean())
    group['1.96*std'] = 1.96*group['Revenue'].std()  
    group['Outlier'] = abs(group['Revenue'] - group['Revenue'].mean()) > 1.96*group['Revenue'].std()
    return group

Newdf2 = State.apply(s)
Newdf2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
      <th>x-Mean</th>
      <th>1.96*std</th>
      <th>Outlier</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
      <td>5.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
      <td>4.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
      <td>3.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
      <td>2.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
      <td>2.25</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>1.25</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>0.980000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
      <td>0.75</td>
      <td>0.980000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
      <td>1.75</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
      <td>2.75</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>4.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>4.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>3.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>3.00</td>
      <td>7.554813</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>0.75</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>0.980000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.25</td>
      <td>0.980000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>1.25</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>1.25</td>
      <td>3.434996</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



根据多个item观察


```python
newdf = df.copy()

StateMonth = newdf.groupby(['State', lambda x: x.month])

StateMonth.groups
```




    {('FL',
      5): DatetimeIndex(['2012-05-01', '2013-05-01'], dtype='datetime64[ns]', freq=None),
     ('FL',
      6): DatetimeIndex(['2012-06-01', '2013-06-01'], dtype='datetime64[ns]', freq=None),
     ('FL',
      9): DatetimeIndex(['2012-09-01', '2013-09-01'], dtype='datetime64[ns]', freq=None),
     ('FL',
      10): DatetimeIndex(['2012-10-01', '2013-10-01'], dtype='datetime64[ns]', freq=None),
     ('GA',
      7): DatetimeIndex(['2012-07-01', '2013-07-01'], dtype='datetime64[ns]', freq=None),
     ('GA',
      8): DatetimeIndex(['2012-08-01', '2013-08-01'], dtype='datetime64[ns]', freq=None),
     ('NY',
      1): DatetimeIndex(['2012-01-01', '2013-01-01'], dtype='datetime64[ns]', freq=None),
     ('NY',
      2): DatetimeIndex(['2012-02-01', '2013-02-01'], dtype='datetime64[ns]', freq=None),
     ('NY',
      3): DatetimeIndex(['2012-03-01', '2013-03-01'], dtype='datetime64[ns]', freq=None),
     ('NY',
      4): DatetimeIndex(['2012-04-01', '2013-04-01'], dtype='datetime64[ns]', freq=None)}




```python
def s(group):
    group['x-Mean'] = abs(group['Revenue'] - group['Revenue'].mean())
    group['1.96*std'] = 1.96*group['Revenue'].std()  
    group['Outlier'] = abs(group['Revenue'] - group['Revenue'].mean()) > 1.96*group['Revenue'].std()
    return group

Newdf2 = StateMonth.apply(s)
Newdf2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Revenue</th>
      <th>State</th>
      <th>x-Mean</th>
      <th>1.96*std</th>
      <th>Outlier</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>1.0</td>
      <td>NY</td>
      <td>4.5</td>
      <td>12.473364</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-02-01</th>
      <td>2.0</td>
      <td>NY</td>
      <td>4.0</td>
      <td>11.087434</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-03-01</th>
      <td>3.0</td>
      <td>NY</td>
      <td>3.0</td>
      <td>8.315576</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-04-01</th>
      <td>4.0</td>
      <td>NY</td>
      <td>2.5</td>
      <td>6.929646</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-05-01</th>
      <td>5.0</td>
      <td>FL</td>
      <td>1.5</td>
      <td>4.157788</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-06-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>1.0</td>
      <td>2.771859</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-08-01</th>
      <td>8.0</td>
      <td>GA</td>
      <td>0.5</td>
      <td>1.385929</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-09-01</th>
      <td>9.0</td>
      <td>FL</td>
      <td>1.5</td>
      <td>4.157788</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2012-10-01</th>
      <td>10.0</td>
      <td>FL</td>
      <td>2.0</td>
      <td>5.543717</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>4.5</td>
      <td>12.473364</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-02-01</th>
      <td>10.0</td>
      <td>NY</td>
      <td>4.0</td>
      <td>11.087434</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-03-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>3.0</td>
      <td>8.315576</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-04-01</th>
      <td>9.0</td>
      <td>NY</td>
      <td>2.5</td>
      <td>6.929646</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-05-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.5</td>
      <td>4.157788</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-06-01</th>
      <td>8.0</td>
      <td>FL</td>
      <td>1.0</td>
      <td>2.771859</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-07-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-08-01</th>
      <td>7.0</td>
      <td>GA</td>
      <td>0.5</td>
      <td>1.385929</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-09-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>1.5</td>
      <td>4.157788</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-10-01</th>
      <td>6.0</td>
      <td>FL</td>
      <td>2.0</td>
      <td>5.543717</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



## 插值

Series和Dataframe对象有一个插值方法`interpolate()`，默认情况下，可以在Nan的数据点位置进行线性插值。

插值是用来填补空缺值得,一般是估计来的值,个人认为最好不要用

可以使用预设的几种方法插值:

+ 'linear': 忽略索引，并将值视为等间隔的。这是支持多指标的唯一方法。

+ 'time': 针对每日并且高频率数据,插值给给定区间的长度

+ 'index', 'values': 用索引的数值

+ 'nearest', 'zero', 'slinear', 'quadratic', 'cubic','barycentric', 'polynomial','krogh', 'piecewise_polynomial', 'spline', 'pchip'和 'akima','piecewise_polynomial'都是scipy中的对应方法


```python
nan_dirty.interpolate()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>weight</th>
      <th>height</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>12.000000</td>
      <td>69.0</td>
      <td>175.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>13.500000</td>
      <td>89.0</td>
      <td>195.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>15.000000</td>
      <td>49.0</td>
      <td>169.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>18.000000</td>
      <td>79.0</td>
      <td>184.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bob</td>
      <td>12.000000</td>
      <td>69.0</td>
      <td>175.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mel</td>
      <td>11.000000</td>
      <td>45.0</td>
      <td>175.500000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mary</td>
      <td>14.000000</td>
      <td>56.0</td>
      <td>176.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jessica</td>
      <td>16.000000</td>
      <td>44.0</td>
      <td>149.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bob</td>
      <td>18.000000</td>
      <td>69.0</td>
      <td>178.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Marila</td>
      <td>17.000000</td>
      <td>48.0</td>
      <td>164.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Johana</td>
      <td>16.000000</td>
      <td>57.0</td>
      <td>162.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melenda</td>
      <td>15.000000</td>
      <td>42.0</td>
      <td>153.000000</td>
      <td>f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Maryre</td>
      <td>15.333333</td>
      <td>49.0</td>
      <td>159.666667</td>
      <td>f</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jeson</td>
      <td>15.666667</td>
      <td>56.0</td>
      <td>166.333333</td>
      <td>m</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bobee</td>
      <td>16.000000</td>
      <td>63.0</td>
      <td>173.000000</td>
      <td>m</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Jesa</td>
      <td>16.000000</td>
      <td>89.0</td>
      <td>173.000000</td>
      <td>m</td>
    </tr>
  </tbody>
</table>
</div>


