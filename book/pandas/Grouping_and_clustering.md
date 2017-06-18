
# 分组与集聚

我们处理数据有时候会有分组的需求,比如统计的是全年龄段的人的,但我们可能会按年龄分组成老中青三组,可能会分成男女两组.

也有时候我们统计一份问卷,需要将数据转置


```python
import pandas as pd
import numpy as np
```

## 转置

比如我们有这样的一组多选问卷结果统计


```python
d = {'a':[1,0,0,1,0,1,1,0,1,0],
     'b':[0,0,1,1,1,0,1,0,1,0],
     "c":[1,0,0,0,0,1,0,1,0,0],
     "d":[1,0,0,1,0,1,1,0,1,1]}
i = ["no.{n}".format(n=i) for i in range(10)]
```


```python
df = pd.DataFrame(data = d, index = i)
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>no.0</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>no.1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>no.2</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>no.3</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>no.4</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>no.5</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>no.6</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>no.7</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>no.8</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>no.9</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



转置只需要使用T方法


```python
df.T
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>no.0</th>
      <th>no.1</th>
      <th>no.2</th>
      <th>no.3</th>
      <th>no.4</th>
      <th>no.5</th>
      <th>no.6</th>
      <th>no.7</th>
      <th>no.8</th>
      <th>no.9</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>c</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>d</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

## 堆积操作

还是之前的多选问题,如果我们想把它堆积起来成为一个有多重索引的序列,可以使用`stack()`方法


```python
stack = df.stack()
stack
```




    no.0  a    1
          b    0
          c    1
          d    1
    no.1  a    0
          b    0
          c    0
          d    0
    no.2  a    0
          b    1
          c    0
          d    0
    no.3  a    1
          b    1
          c    0
          d    1
    no.4  a    0
          b    1
          c    0
          d    0
    no.5  a    1
          b    0
          c    1
          d    1
    no.6  a    1
          b    1
          c    0
          d    1
    no.7  a    0
          b    0
          c    1
          d    0
    no.8  a    1
          b    1
          c    0
          d    1
    no.9  a    0
          b    0
          c    0
          d    1
    dtype: int64




```python
stack["no.0"]
```




    a    1
    b    0
    c    1
    d    1
    dtype: int64



也可以使用`unstack`汇总每个选项不同题目的结果


```python
unstack = df.unstack()
unstack
```




    a  no.0    1
       no.1    0
       no.2    0
       no.3    1
       no.4    0
       no.5    1
       no.6    1
       no.7    0
       no.8    1
       no.9    0
    b  no.0    0
       no.1    0
       no.2    1
       no.3    1
       no.4    1
       no.5    0
       no.6    1
       no.7    0
       no.8    1
       no.9    0
    c  no.0    1
       no.1    0
       no.2    0
       no.3    0
       no.4    0
       no.5    1
       no.6    0
       no.7    1
       no.8    0
       no.9    0
    d  no.0    1
       no.1    0
       no.2    0
       no.3    1
       no.4    0
       no.5    1
       no.6    1
       no.7    0
       no.8    1
       no.9    1
    dtype: int64




```python
unstack["a"]
```




    no.0    1
    no.1    0
    no.2    0
    no.3    1
    no.4    0
    no.5    1
    no.6    1
    no.7    0
    no.8    1
    no.9    0
    dtype: int64



## groupby

groupby的功能类似SQL的group by关键字: 

Split-Apply-Combine

+ Split,就是按照规则分组 
+ Apply,通过⼀一定的agg函数来获得输⼊入pd.Series返回⼀一个值的效果 
+ Combine,把结果收集起来

Pandas的groupby的灵活性:

+ 分组的关键字可以来⾃自于index,也可以来⾃自于真实的列数据 
+ 分组规则可以通过⼀一列或者多列


```python
iris_data = pd.read_csv("./source/iris.data",header = None,encoding = "utf-8",
                        names=["sepal_length","sepal_width","petal_length","petal_width","class"])
```


```python
iris_data[:5]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
iris_group = iris_data.groupby("class")
```


```python
iris_group.sum()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
    </tr>
    <tr>
      <th>class</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Iris-setosa</th>
      <td>250.3</td>
      <td>170.9</td>
      <td>73.2</td>
      <td>12.2</td>
    </tr>
    <tr>
      <th>Iris-versicolor</th>
      <td>296.8</td>
      <td>138.5</td>
      <td>213.0</td>
      <td>66.3</td>
    </tr>
    <tr>
      <th>Iris-virginica</th>
      <td>329.4</td>
      <td>148.7</td>
      <td>277.6</td>
      <td>101.3</td>
    </tr>
  </tbody>
</table>
</div>




```python
iris_group.mean()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
    </tr>
    <tr>
      <th>class</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Iris-setosa</th>
      <td>5.006</td>
      <td>3.418</td>
      <td>1.464</td>
      <td>0.244</td>
    </tr>
    <tr>
      <th>Iris-versicolor</th>
      <td>5.936</td>
      <td>2.770</td>
      <td>4.260</td>
      <td>1.326</td>
    </tr>
    <tr>
      <th>Iris-virginica</th>
      <td>6.588</td>
      <td>2.974</td>
      <td>5.552</td>
      <td>2.026</td>
    </tr>
  </tbody>
</table>
</div>




```python
for level,subset in iris_group:
    print(level)
    print(subset[:5])
```

    Iris-setosa
       sepal_length  sepal_width  petal_length  petal_width        class
    0           5.1          3.5           1.4          0.2  Iris-setosa
    1           4.9          3.0           1.4          0.2  Iris-setosa
    2           4.7          3.2           1.3          0.2  Iris-setosa
    3           4.6          3.1           1.5          0.2  Iris-setosa
    4           5.0          3.6           1.4          0.2  Iris-setosa
    Iris-versicolor
        sepal_length  sepal_width  petal_length  petal_width            class
    50           7.0          3.2           4.7          1.4  Iris-versicolor
    51           6.4          3.2           4.5          1.5  Iris-versicolor
    52           6.9          3.1           4.9          1.5  Iris-versicolor
    53           5.5          2.3           4.0          1.3  Iris-versicolor
    54           6.5          2.8           4.6          1.5  Iris-versicolor
    Iris-virginica
         sepal_length  sepal_width  petal_length  petal_width           class
    100           6.3          3.3           6.0          2.5  Iris-virginica
    101           5.8          2.7           5.1          1.9  Iris-virginica
    102           7.1          3.0           5.9          2.1  Iris-virginica
    103           6.3          2.9           5.6          1.8  Iris-virginica
    104           6.5          3.0           5.8          2.2  Iris-virginica
    

由此可见实际上groupby将表格拆分成了一组(分组名,子表)的键值对
之后的操作可以有:

> agg()方法

agg方法 是将由子表构成的序列作为参数操作,要求操作可以每个子表返回一个非序列的返回值,操作完成后生成新的表格


```python
iris_group.agg(lambda x :"好")
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
    </tr>
    <tr>
      <th>class</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Iris-setosa</th>
      <td>好</td>
      <td>好</td>
      <td>好</td>
      <td>好</td>
    </tr>
    <tr>
      <th>Iris-versicolor</th>
      <td>好</td>
      <td>好</td>
      <td>好</td>
      <td>好</td>
    </tr>
    <tr>
      <th>Iris-virginica</th>
      <td>好</td>
      <td>好</td>
      <td>好</td>
      <td>好</td>
    </tr>
  </tbody>
</table>
</div>



>transform()

transform方法对子表序列运算方法,分别运算完后结果放回对应的行,也就是说原来的表什么样算完结构一样但内容不一样了,和map有点像,但运算的时候序列不是1个总序列而是多个分开的子序列


```python
iris_group.transform(lambda x:x - x.mean())[:5]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.094</td>
      <td>0.082</td>
      <td>-0.064</td>
      <td>-0.044</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.106</td>
      <td>-0.418</td>
      <td>-0.064</td>
      <td>-0.044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.306</td>
      <td>-0.218</td>
      <td>-0.164</td>
      <td>-0.044</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.406</td>
      <td>-0.318</td>
      <td>0.036</td>
      <td>-0.044</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.006</td>
      <td>0.182</td>
      <td>-0.064</td>
      <td>-0.044</td>
    </tr>
  </tbody>
</table>
</div>



### Categorical类型

现在pandas可以使用Categorical类型了,


```python
iris_data[:5]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
iris_data["class"] = iris_data["class"].astype("category")
```


```python
iris_data["class"][::20]
```




    0          Iris-setosa
    20         Iris-setosa
    40         Iris-setosa
    60     Iris-versicolor
    80     Iris-versicolor
    100     Iris-virginica
    120     Iris-virginica
    140     Iris-virginica
    Name: class, dtype: category
    Categories (3, object): [Iris-setosa, Iris-versicolor, Iris-virginica]



## 聚集

聚集操作一般是将多组数据合并到一张表格中

比如连接表格,我们可以使用函数`concat([tables...])`


```python
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                    index=[0, 1, 2, 3])

df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7']},
                    index=[4, 5, 6, 7])
 

df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
                    'B': ['B8', 'B9', 'B10', 'B11'],
                    'C': ['C8', 'C9', 'C10', 'C11'],
                    'D': ['D8', 'D9', 'D10', 'D11']},
                    index=[8, 9, 10, 11])

```


```python
df1
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
  </tbody>
</table>
</div>




```python
df3
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>




```python
result = pd.concat([df1,df2,df3])
```


```python
result
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>



`concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,keys=None, levels=None, names=None,verify_integrity=False,copy=True)`是concat的完整参数说明,

+ objs：一个序列或dict，dataframe，或Panel对象。

+ axis：{ 0，1，…}，默认值0。将沿哪个轴操作。

+ join：{'inner', 'outer'}，默认为'outer'。如何处理其他轴的索引,outer为求并,inner为求交集.

+ ignore_index：布尔值，默认为False。如果是True，则不使用索引值用于连接轴。由此产生的轴将被标记为(0，…，N - 1).常用于连接对象，连接轴为没有意义的索引信息。注意,其他轴上的索引值仍然在联接中有用.

+ join_axes：索引对象的列表。用于其他n - 1轴而不执行内/外集逻辑的特定索引.

+ keys：序列，默认无。使用传递key作为最外层级别构造分层索引.。如果多个层面通过，应包含元组。级别：序列列表，默认没有。具体水平（独特价值）用于构建多指标。否则，他们将从key推断。

+ names：列表，默认无。生成层次索引中的level的名称。

+ verify_integrity：布尔值，默认为false。检查新的连接轴是否包含重复。这相对真实的数据连接开销很大。

+ 拷贝：布尔值，默认值为。如果FALSE，不复制数据

### 为每组添加外层索引


```python
pd.concat([df1,df2,df3], keys=['x', 'y', 'z'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">x</th>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">y</th>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">z</th>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat({'x': df1, 'y': df2, 'z': df3})
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">x</th>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">y</th>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">z</th>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>



### 设置索引


```python
df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
                    'D': ['D2', 'D3', 'D6', 'D7'],
                    'F': ['F2', 'F3', 'F6', 'F7']},
                    index=[2, 3, 6, 7])
```


```python
df4
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>B</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>B2</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>B3</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B6</td>
      <td>D6</td>
      <td>F6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>B7</td>
      <td>D7</td>
      <td>F7</td>
    </tr>
  </tbody>
</table>
</div>



### 横向聚集


```python
pd.concat([df1, df4], axis=1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>B</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>B2</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>B3</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B6</td>
      <td>D6</td>
      <td>F6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B7</td>
      <td>D7</td>
      <td>F7</td>
    </tr>
  </tbody>
</table>
</div>



### 使用交集


```python
pd.concat([df1, df4], axis=1, join='inner')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>B</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>B2</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>B3</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
  </tbody>
</table>
</div>



### 只合并特定的索引


```python
pd.concat([df1, df4], axis=1, join_axes=[df1.index])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>B</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>B2</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>B3</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
  </tbody>
</table>
</div>



### 除了向下连接行,同样可以向右连接列序列


```python
s3 = pd.Series([0, 1, 2, 3], name='foo')

s4 = pd.Series([0, 1, 2, 3])

s5 = pd.Series([0, 1, 4, 5])

pd.concat([s3, s4, s5], axis=1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>foo</th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>3</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat([s3, s4, s5], axis=1, keys=['red','blue','yellow'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>red</th>
      <th>blue</th>
      <th>yellow</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>3</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



### 使用append方法链式集聚

append(other, ignore_index=False, verify_integrity=False)并不想contact那样可以自己设定很多


```python
df1.append(df2).append(df3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.append([df2,df3])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.append(df4)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>B2</td>
      <td>NaN</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>B3</td>
      <td>NaN</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>B6</td>
      <td>NaN</td>
      <td>D6</td>
      <td>F6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>B7</td>
      <td>NaN</td>
      <td>D7</td>
      <td>F7</td>
    </tr>
  </tbody>
</table>
</div>



## 使用joining/merging类似在数据中一样的操作

### merging

有经验的关系型数据库用户都熟悉用于描述连接两个SQL表的术语merging。有几种情况要考虑：

+ 一对一的连接
+ 多对一的连接
+ 多对多联接

`pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None,left_index=False, right_index=False, sort=True,suffixes=('_x', '_y'), copy=True, indicator=False)`

+ on 连接的列名。必须同时在左边和右边的df对象中。

+ left_on/right_on 用左边/右边的某列作为key。可以是列名或长度等于DataFrame长度的列

+ left_index/right_index  如果是真的，使用索引（行标签）从左/右边的key加入。在一个多指标数据帧的情况下，数量必须匹配从右/左变key加入的数量

+ how 如何merging,有“left”，“right”，“outer”，“inner”可选。默认为inner

Merge method|SQL Join Name|Description
---|---|---
left	|LEFT OUTER JOIN	|Use keys from left frame only
right	|RIGHT OUTER JOIN	|Use keys from right frame only
outer	|FULL OUTER JOIN	|Use union of keys from both frames
inner	|INNER JOIN	|Use intersection of keys from both frames

+ sort 将结果排序。默认为true，通常设置为FALSE将大大改善性能
+ suffixes 后缀
+ indicator 用于指示merge的行为,如果是真的，一个范畴类型列为`_merge`将被添加到输出对象需要的值

Observation Origin |	`_merge value`
---|---
Merge key only in 'left' frame	|`left_only`
Merge key only in 'right' frame	|`right_only`
Merge key in both frames	|both

#### 最一般的连接,通过key列连接


```python
left = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                     'A': ['A0', 'A1', 'A2', 'A3'],
                     'B': ['B0', 'B1', 'B2', 'B3']})

```


```python
left
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>K1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>K3</td>
    </tr>
  </tbody>
</table>
</div>




```python
right = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                       'C': ['C0', 'C1', 'C2', 'C3'],
                       'D': ['D0', 'D1', 'D2', 'D3']})
```


```python
right
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>C</th>
      <th>D</th>
      <th>key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>C0</td>
      <td>D0</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>C1</td>
      <td>D1</td>
      <td>K1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C2</td>
      <td>D2</td>
      <td>K2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C3</td>
      <td>D3</td>
      <td>K3</td>
    </tr>
  </tbody>
</table>
</div>




```python
result = pd.merge(left, right, on='key')
```


```python
result
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>K1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>K3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>



#### 不同连接方式的不同结果


```python
left = pd.DataFrame({'key1': ['K0', 'K0', 'K1', 'K2'],
                     'key2': ['K0', 'K1', 'K0', 'K1'],
                     'A': ['A0', 'A1', 'A2', 'A3'],
                     'B': ['B0', 'B1', 'B2', 'B3']}) 
```


```python
left
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key1</th>
      <th>key2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>K0</td>
      <td>K1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>K2</td>
      <td>K1</td>
    </tr>
  </tbody>
</table>
</div>




```python
right = pd.DataFrame({'key1': ['K0', 'K1', 'K1', 'K2'],
                      'key2': ['K0', 'K0', 'K0', 'K0'],
                      'C': ['C0', 'C1', 'C2', 'C3'],
                      'D': ['D0', 'D1', 'D2', 'D3']})
```


```python
right
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>C</th>
      <th>D</th>
      <th>key1</th>
      <th>key2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>C0</td>
      <td>D0</td>
      <td>K0</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>C1</td>
      <td>D1</td>
      <td>K1</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C2</td>
      <td>D2</td>
      <td>K1</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C3</td>
      <td>D3</td>
      <td>K2</td>
      <td>K0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left, right, on=['key1', 'key2'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key1</th>
      <th>key2</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left, right, how='left', on=['key1', 'key2'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key1</th>
      <th>key2</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>K0</td>
      <td>K1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A3</td>
      <td>B3</td>
      <td>K2</td>
      <td>K1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left, right, how='right', on=['key1', 'key2'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key1</th>
      <th>key2</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>K2</td>
      <td>K0</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left, right, how='outer', on=['key1', 'key2'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key1</th>
      <th>key2</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>K0</td>
      <td>K1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A3</td>
      <td>B3</td>
      <td>K2</td>
      <td>K1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>K2</td>
      <td>K0</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left, right, how='inner', on=['key1', 'key2'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key1</th>
      <th>key2</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K1</td>
      <td>K0</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1 = pd.DataFrame({'col1': [0, 1], 'col_left':['a', 'b']})
df1
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col_left</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>b</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame({'col1': [1, 2, 2],'col_right':[2, 2, 2]})
```


```python
df2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col_right</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(df1, df2, on='col1', how='outer', indicator=True)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col_left</th>
      <th>col_right</th>
      <th>_merge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>a</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>b</td>
      <td>2.0</td>
      <td>both</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>right_only</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>right_only</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(df1, df2, on='col1', how='outer', indicator='indicator_column')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col_left</th>
      <th>col_right</th>
      <th>indicator_column</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>a</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>b</td>
      <td>2.0</td>
      <td>both</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>right_only</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>right_only</td>
    </tr>
  </tbody>
</table>
</div>



### join方法

join之于merge就像上面的append之于contact,是一种简便方法


```python
left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
                     'B': ['B0', 'B1', 'B2']},
                     index=['K0', 'K1', 'K2'])

right = pd.DataFrame({'C': ['C0', 'C2', 'C3'],
                      'D': ['D0', 'D2', 'D3']},
                      index=['K0', 'K2', 'K3'])
```


```python
left
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <td>A0</td>
      <td>B0</td>
    </tr>
    <tr>
      <th>K1</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
  </tbody>
</table>
</div>




```python
right
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>K3</th>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
left.join(right)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K1</th>
      <td>A1</td>
      <td>B1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
  </tbody>
</table>
</div>




```python
left.join(right, how='outer')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K1</th>
      <td>A1</td>
      <td>B1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>K3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
left.join(right, how='inner')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
  </tbody>
</table>
</div>



除了使用index索引作为key外,join也可以指定key

`left.join(right, on=key_or_keys)`


```python
left = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                     'B': ['B0', 'B1', 'B2', 'B3'],
                     'key': ['K0', 'K1', 'K0', 'K1']})
```


```python
right = pd.DataFrame({'C': ['C0', 'C1'],
                      'D': ['D0', 'D1']},
                     index=['K0', 'K1'])
```


```python
left
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>K1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>K1</td>
    </tr>
  </tbody>
</table>
</div>




```python
right
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K1</th>
      <td>C1</td>
      <td>D1</td>
    </tr>
  </tbody>
</table>
</div>




```python
left.join(right, on='key')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>key</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>K1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>K1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
  </tbody>
</table>
</div>



#### 针对多索引的join


```python
left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
                     'B': ['B0', 'B1', 'B2']},
                     index=pd.Index(['K0', 'K1', 'K2'], name='key'))

index = pd.MultiIndex.from_tuples([('K0', 'Y0'), ('K1', 'Y1'),
                                   ('K2', 'Y2'), ('K2', 'Y3')],
                                    names=['key', 'Y'])
right = pd.DataFrame({'C': ['C0', 'C1', 'C2', 'C3'],
                      'D': ['D0', 'D1', 'D2', 'D3']},
                      index=index)
```


```python
left
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
    <tr>
      <th>key</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <td>A0</td>
      <td>B0</td>
    </tr>
    <tr>
      <th>K1</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
  </tbody>
</table>
</div>




```python
right
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>C</th>
      <th>D</th>
    </tr>
    <tr>
      <th>key</th>
      <th>Y</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <th>Y0</th>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K1</th>
      <th>Y1</th>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">K2</th>
      <th>Y2</th>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>Y3</th>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
index
```




    MultiIndex(levels=[['K0', 'K1', 'K2'], ['Y0', 'Y1', 'Y2', 'Y3']],
               labels=[[0, 1, 2, 2], [0, 1, 2, 3]],
               names=['key', 'Y'])




```python
left.join(right, how='inner')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
    <tr>
      <th>key</th>
      <th>Y</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K0</th>
      <th>Y0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K1</th>
      <th>Y1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">K2</th>
      <th>Y2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>Y3</th>
      <td>A2</td>
      <td>B2</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
index = pd.MultiIndex.from_tuples([('K0', 'X0'), ('K0', 'X1'),
                                   ('K1', 'X2')],
                                   names=['key', 'X'])
left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
                     'B': ['B0', 'B1', 'B2']},
                      index=index)
```


```python
left
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
    <tr>
      <th>key</th>
      <th>X</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">K0</th>
      <th>X0</th>
      <td>A0</td>
      <td>B0</td>
    </tr>
    <tr>
      <th>X1</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>K1</th>
      <th>X2</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left.reset_index(), right.reset_index(),
                   on=['key'], how='inner').set_index(['key','X','Y']) 
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
    <tr>
      <th>key</th>
      <th>X</th>
      <th>Y</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">K0</th>
      <th>X0</th>
      <th>Y0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>X1</th>
      <th>Y0</th>
      <td>A1</td>
      <td>B1</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>K1</th>
      <th>X2</th>
      <th>Y1</th>
      <td>A2</td>
      <td>B2</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
  </tbody>
</table>
</div>



## "打补丁"

另一个相当普遍的情况是有两个像索引（或类似的索引）系列或数据帧的对象，一个想在另一个上"打补丁",把空值填上,这时候可以使用`.combine_first`方法


```python
df1 = pd.DataFrame([[np.nan, 3., 5.], 
                    [-4.6, np.nan, np.nan],
                    [np.nan, 7., np.nan]])
```


```python
df2 = pd.DataFrame([[-42.6, np.nan, -8.2],
                    [-5., 1.6, 4]],
                   index=[1, 2])
```


```python
df1
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>3.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-4.6</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>7.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>-42.6</td>
      <td>NaN</td>
      <td>-8.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-5.0</td>
      <td>1.6</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.combine_first(df2)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>3.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-4.6</td>
      <td>NaN</td>
      <td>-8.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-5.0</td>
      <td>7.0</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



注意这时候原来有值得地方并不会被替换

也可以使用`update`方法用df2的值替换df1中的对应值,这时是修改df1而不是生成新的表


```python
df1.update(df2)
```


```python
df1
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>3.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-42.6</td>
      <td>NaN</td>
      <td>-8.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-5.0</td>
      <td>1.6</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



## 时间序列处理

### merge_ordered

`merge_ordered()`函数允许组合时间序列和其他有序数据。特别是它有一个可选的fill_method关键字来填充/内插缺失的数据：



```python
left = pd.DataFrame({'k': ['K0', 'K1', 'K1', 'K2'],
                     'lv': [1, 2, 3, 4],
                    's': ['a', 'b', 'c', 'd']})
```


```python
right = pd.DataFrame({'k': ['K1', 'K2', 'K4'], 
                      'rv': [1, 2, 3]})
```


```python
left
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>k</th>
      <th>lv</th>
      <th>s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K0</td>
      <td>1</td>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K1</td>
      <td>2</td>
      <td>b</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K1</td>
      <td>3</td>
      <td>c</td>
    </tr>
    <tr>
      <th>3</th>
      <td>K2</td>
      <td>4</td>
      <td>d</td>
    </tr>
  </tbody>
</table>
</div>




```python
right
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>k</th>
      <th>rv</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K4</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge_ordered(left, right, fill_method='ffill', left_by='s')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>k</th>
      <th>lv</th>
      <th>s</th>
      <th>rv</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K0</td>
      <td>1.0</td>
      <td>a</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K1</td>
      <td>1.0</td>
      <td>a</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K2</td>
      <td>1.0</td>
      <td>a</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>K4</td>
      <td>1.0</td>
      <td>a</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>K1</td>
      <td>2.0</td>
      <td>b</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>K2</td>
      <td>2.0</td>
      <td>b</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>K4</td>
      <td>2.0</td>
      <td>b</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>K1</td>
      <td>3.0</td>
      <td>c</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>K2</td>
      <td>3.0</td>
      <td>c</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>K4</td>
      <td>3.0</td>
      <td>c</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>K1</td>
      <td>NaN</td>
      <td>d</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>K2</td>
      <td>4.0</td>
      <td>d</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>K4</td>
      <td>4.0</td>
      <td>d</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>



### merge_asof
`merge_asof()`在行为上,除了匹配最相近的值而不是相等的值这点外,类似left-join.

asof合并可以执行分组合并。除了on键上最接近的匹配之外，这与by键地位相似。


```python
trades = pd.DataFrame({
    'time': pd.to_datetime(['20160525 13:30:00.023',
                            '20160525 13:30:00.038',
                            '20160525 13:30:00.048',
                            '20160525 13:30:00.048',
                            '20160525 13:30:00.048']),
     'ticker': ['MSFT', 'MSFT',
               'GOOG', 'GOOG', 'AAPL'],
     'price': [51.95, 51.95,
              720.77, 720.92, 98.00],
     'quantity': [75, 155,
                100, 100, 100]},
      columns=['time', 'ticker', 'price', 'quantity'])

```


```python
quotes = pd.DataFrame({
    'time': pd.to_datetime(['20160525 13:30:00.023',
                           '20160525 13:30:00.023',
                           '20160525 13:30:00.030',
                           '20160525 13:30:00.041',
                           '20160525 13:30:00.048',
                            '20160525 13:30:00.049',
                            '20160525 13:30:00.072',
                            '20160525 13:30:00.075']),
    'ticker': ['GOOG', 'MSFT', 'MSFT',
               'MSFT', 'GOOG', 'AAPL', 'GOOG',
               'MSFT'],
    'bid': [720.50, 51.95, 51.97, 51.99,
            720.50, 97.99, 720.50, 52.01],
    'ask': [720.93, 51.96, 51.98, 52.00,
            720.93, 98.01, 720.88, 52.03]},
   columns=['time', 'ticker', 'bid', 'ask'])

```


```python
trades
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>time</th>
      <th>ticker</th>
      <th>price</th>
      <th>quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-05-25 13:30:00.023</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>75</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-05-25 13:30:00.038</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>155</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.77</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.92</td>
      <td>100</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>AAPL</td>
      <td>98.00</td>
      <td>100</td>
    </tr>
  </tbody>
</table>
</div>




```python
quotes
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>time</th>
      <th>ticker</th>
      <th>bid</th>
      <th>ask</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-05-25 13:30:00.023</td>
      <td>GOOG</td>
      <td>720.50</td>
      <td>720.93</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-05-25 13:30:00.023</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>51.96</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-05-25 13:30:00.030</td>
      <td>MSFT</td>
      <td>51.97</td>
      <td>51.98</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-05-25 13:30:00.041</td>
      <td>MSFT</td>
      <td>51.99</td>
      <td>52.00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.50</td>
      <td>720.93</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2016-05-25 13:30:00.049</td>
      <td>AAPL</td>
      <td>97.99</td>
      <td>98.01</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2016-05-25 13:30:00.072</td>
      <td>GOOG</td>
      <td>720.50</td>
      <td>720.88</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2016-05-25 13:30:00.075</td>
      <td>MSFT</td>
      <td>52.01</td>
      <td>52.03</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge_asof(trades, quotes,
              on='time',
              by='ticker')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>time</th>
      <th>ticker</th>
      <th>price</th>
      <th>quantity</th>
      <th>bid</th>
      <th>ask</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-05-25 13:30:00.023</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>75</td>
      <td>51.95</td>
      <td>51.96</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-05-25 13:30:00.038</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>155</td>
      <td>51.97</td>
      <td>51.98</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.77</td>
      <td>100</td>
      <td>720.50</td>
      <td>720.93</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.92</td>
      <td>100</td>
      <td>720.50</td>
      <td>720.93</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>AAPL</td>
      <td>98.00</td>
      <td>100</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge_asof(trades, quotes,
              on='time',
              by='ticker',
              tolerance=pd.Timedelta('2ms'))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>time</th>
      <th>ticker</th>
      <th>price</th>
      <th>quantity</th>
      <th>bid</th>
      <th>ask</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-05-25 13:30:00.023</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>75</td>
      <td>51.95</td>
      <td>51.96</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-05-25 13:30:00.038</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>155</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.77</td>
      <td>100</td>
      <td>720.50</td>
      <td>720.93</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.92</td>
      <td>100</td>
      <td>720.50</td>
      <td>720.93</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>AAPL</td>
      <td>98.00</td>
      <td>100</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge_asof(trades, quotes,
               on='time',
               by='ticker',
               tolerance=pd.Timedelta('10ms'),
               allow_exact_matches=False)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>time</th>
      <th>ticker</th>
      <th>price</th>
      <th>quantity</th>
      <th>bid</th>
      <th>ask</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-05-25 13:30:00.023</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>75</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-05-25 13:30:00.038</td>
      <td>MSFT</td>
      <td>51.95</td>
      <td>155</td>
      <td>51.97</td>
      <td>51.98</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.77</td>
      <td>100</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>GOOG</td>
      <td>720.92</td>
      <td>100</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-05-25 13:30:00.048</td>
      <td>AAPL</td>
      <td>98.00</td>
      <td>100</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


