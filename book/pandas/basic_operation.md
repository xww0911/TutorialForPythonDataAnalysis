
# 表格基本操作

pandas的操作都比较傻瓜,只要引入包后调用对应类或者函数即可,可以在任何交互界面中执行(当然最推荐的还是Jupyter 即原 ipython)这也是有的人更加喜欢pandas而不喜欢excel的原因.我们以之前的iris作为例子


```python
import pandas as pd
iris_data = pd.read_csv("source/iris.csv")
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



## 数据过滤

就像透视表一样,我们可以有选择性的查看表格


```python
iris_data[iris_data["class"]=="Iris-virginica"][::10]
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
      <th>100</th>
      <td>6.3</td>
      <td>3.3</td>
      <td>6.0</td>
      <td>2.5</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>110</th>
      <td>6.5</td>
      <td>3.2</td>
      <td>5.1</td>
      <td>2.0</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>120</th>
      <td>6.9</td>
      <td>3.2</td>
      <td>5.7</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>130</th>
      <td>7.4</td>
      <td>2.8</td>
      <td>6.1</td>
      <td>1.9</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>140</th>
      <td>6.7</td>
      <td>3.1</td>
      <td>5.6</td>
      <td>2.4</td>
      <td>Iris-virginica</td>
    </tr>
  </tbody>
</table>
</div>




```python
iris_data[iris_data["petal_width"]>iris_data["petal_width"].mean()][::10]
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
      <th>50</th>
      <td>7.0</td>
      <td>3.2</td>
      <td>4.7</td>
      <td>1.4</td>
      <td>Iris-versicolor</td>
    </tr>
    <tr>
      <th>63</th>
      <td>6.1</td>
      <td>2.9</td>
      <td>4.7</td>
      <td>1.4</td>
      <td>Iris-versicolor</td>
    </tr>
    <tr>
      <th>75</th>
      <td>6.6</td>
      <td>3.0</td>
      <td>4.4</td>
      <td>1.4</td>
      <td>Iris-versicolor</td>
    </tr>
    <tr>
      <th>88</th>
      <td>5.6</td>
      <td>3.0</td>
      <td>4.1</td>
      <td>1.3</td>
      <td>Iris-versicolor</td>
    </tr>
    <tr>
      <th>100</th>
      <td>6.3</td>
      <td>3.3</td>
      <td>6.0</td>
      <td>2.5</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>110</th>
      <td>6.5</td>
      <td>3.2</td>
      <td>5.1</td>
      <td>2.0</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>120</th>
      <td>6.9</td>
      <td>3.2</td>
      <td>5.7</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>130</th>
      <td>7.4</td>
      <td>2.8</td>
      <td>6.1</td>
      <td>1.9</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>140</th>
      <td>6.7</td>
      <td>3.1</td>
      <td>5.6</td>
      <td>2.4</td>
      <td>Iris-virginica</td>
    </tr>
  </tbody>
</table>
</div>



## 排序sort

比如我们根据sepal_length做降序排列


```python
biggest5_sl_iris = iris_data.sort('sepal_length',ascending=False)[:5]
```

    C:\Users\Administrator\Anaconda3\lib\site-packages\ipykernel\__main__.py:1: FutureWarning: sort(columns=....) is deprecated, use sort_values(by=.....)
      if __name__ == '__main__':
    


```python
biggest5_sl_iris
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
      <th>131</th>
      <td>7.9</td>
      <td>3.8</td>
      <td>6.4</td>
      <td>2.0</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>135</th>
      <td>7.7</td>
      <td>3.0</td>
      <td>6.1</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>122</th>
      <td>7.7</td>
      <td>2.8</td>
      <td>6.7</td>
      <td>2.0</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>117</th>
      <td>7.7</td>
      <td>3.8</td>
      <td>6.7</td>
      <td>2.2</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>118</th>
      <td>7.7</td>
      <td>2.6</td>
      <td>6.9</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
  </tbody>
</table>
</div>



再把序号排排序


```python
biggest5_sl_iris.sort(ascending=False)
```

    C:\Users\Administrator\Anaconda3\lib\site-packages\ipykernel\__main__.py:1: FutureWarning: sort(....) is deprecated, use sort_index(.....)
      if __name__ == '__main__':
    




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
      <th>135</th>
      <td>7.7</td>
      <td>3.0</td>
      <td>6.1</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>131</th>
      <td>7.9</td>
      <td>3.8</td>
      <td>6.4</td>
      <td>2.0</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>122</th>
      <td>7.7</td>
      <td>2.8</td>
      <td>6.7</td>
      <td>2.0</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>118</th>
      <td>7.7</td>
      <td>2.6</td>
      <td>6.9</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>117</th>
      <td>7.7</td>
      <td>3.8</td>
      <td>6.7</td>
      <td>2.2</td>
      <td>Iris-virginica</td>
    </tr>
  </tbody>
</table>
</div>



## 排名rank


```python
biggest5_sl_iris.rank(method="min",numeric_only = True)
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
      <th>131</th>
      <td>5.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>135</th>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>122</th>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>117</th>
      <td>1.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>118</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



## 选择,切片操作

切片可以用来准确的提取需要的数据

pandas支持多种切片方式

### 间隔切片


```python
iris_data[::20]#每20行取一次
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
      <th>20</th>
      <td>5.4</td>
      <td>3.4</td>
      <td>1.7</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>40</th>
      <td>5.0</td>
      <td>3.5</td>
      <td>1.3</td>
      <td>0.3</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>60</th>
      <td>5.0</td>
      <td>2.0</td>
      <td>3.5</td>
      <td>1.0</td>
      <td>Iris-versicolor</td>
    </tr>
    <tr>
      <th>80</th>
      <td>5.5</td>
      <td>2.4</td>
      <td>3.8</td>
      <td>1.1</td>
      <td>Iris-versicolor</td>
    </tr>
    <tr>
      <th>100</th>
      <td>6.3</td>
      <td>3.3</td>
      <td>6.0</td>
      <td>2.5</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>120</th>
      <td>6.9</td>
      <td>3.2</td>
      <td>5.7</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>140</th>
      <td>6.7</td>
      <td>3.1</td>
      <td>5.6</td>
      <td>2.4</td>
      <td>Iris-virginica</td>
    </tr>
  </tbody>
</table>
</div>



### 连续数据段提取


```python
iris_data[5:10]#取第5到第9行
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
      <th>5</th>
      <td>5.4</td>
      <td>3.9</td>
      <td>1.7</td>
      <td>0.4</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4.6</td>
      <td>3.4</td>
      <td>1.4</td>
      <td>0.3</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>7</th>
      <td>5.0</td>
      <td>3.4</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4.4</td>
      <td>2.9</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4.9</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>



### 提取某一行


```python
iris_data.loc[5] #取第5行的数据
```




    sepal_length            5.4
    sepal_width             3.9
    petal_length            1.7
    petal_width             0.4
    class           Iris-setosa
    Name: 5, dtype: object



或者


```python
iris_data.ix[5] #取第5行的数据
```




    sepal_length            5.4
    sepal_width             3.9
    petal_length            1.7
    petal_width             0.4
    class           Iris-setosa
    Name: 5, dtype: object



## 投影操作

所谓投影和数据库中差不多,就是取列(取属性),简单的方式就是用`[]`圈住需要的列号或者列名


```python
iris_data["sepal_length"][:5]#取某列
```




    0    5.1
    1    4.9
    2    4.7
    3    4.6
    4    5.0
    Name: sepal_length, dtype: float64




```python
iris_data.sepal_length[:5]#同样地取某列
```




    0    5.1
    1    4.9
    2    4.7
    3    4.6
    4    5.0
    Name: sepal_length, dtype: float64




```python
iris_data[["sepal_length","petal_width"]][:5]#取两列
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>petal_width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



## **iloc **位置坐标操作

简单粗暴的直接查看对应坐标,第一位参数是行,第二位是列


```python
iris_data.iloc[5]#取第5行的数据
```




    sepal_length            5.4
    sepal_width             3.9
    petal_length            1.7
    petal_width             0.4
    class           Iris-setosa
    Name: 5, dtype: object




```python
iris_data.iloc[0,2:4]#取第一行第3个数据和第四个数据
```




    petal_length    1.4
    petal_width     0.2
    Name: 0, dtype: object



## 增加一列元素

增加一列只需要在原数据上后面用`[]`填入要新增的元素即可,注意这个操作是对源数据的修改,如果希望源数据不变,先copy再增加


```python
people_fromExcel = pd.read_excel('./source/people.xlsx', u'工作表1', index_col=None, na_values=['NA'])

people_Data = people_fromExcel.append(pd.DataFrame([["Hao",24]],columns = ["name","age"])).reset_index(drop=True)
people_Data
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Michael</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Andy</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Justin</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hao</td>
      <td>24.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
people_Data["nation"] = ["USA","UK","AUS","PRC"]
```


```python
people_Data
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>nation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Michael</td>
      <td>NaN</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Andy</td>
      <td>30.0</td>
      <td>UK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Justin</td>
      <td>19.0</td>
      <td>AUS</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hao</td>
      <td>24.0</td>
      <td>PRC</td>
    </tr>
  </tbody>
</table>
</div>



也可以只输入一个值,这样就全部都是都是它了


```python
people_Data[u"星球"] = u"地球"
people_Data
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>nation</th>
      <th>星球</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Michael</td>
      <td>NaN</td>
      <td>USA</td>
      <td>地球</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Andy</td>
      <td>30.0</td>
      <td>UK</td>
      <td>地球</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Justin</td>
      <td>19.0</td>
      <td>AUS</td>
      <td>地球</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hao</td>
      <td>24.0</td>
      <td>PRC</td>
      <td>地球</td>
    </tr>
  </tbody>
</table>
</div>



## pandas的函数操作

由于python本身对函数式编程的支持,以及pandas底层依赖的numpy优秀的向量化计算能力,pandas可以使用类似Universal  Function的方式向量化的求值.
例:求出iris三类的信息熵


```python
import scipy as sp
```


```python
slogs = lambda x:sp.log(x)*x
entropy = lambda x:sp.exp((slogs(x.sum())-x.map(slogs).sum())/x.sum())
iris_data.groupby("class").agg(entropy)
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
      <td>49.878745</td>
      <td>49.695242</td>
      <td>49.654909</td>
      <td>45.810069</td>
    </tr>
    <tr>
      <th>Iris-versicolor</th>
      <td>49.815081</td>
      <td>49.680665</td>
      <td>49.694505</td>
      <td>49.452305</td>
    </tr>
    <tr>
      <th>Iris-virginica</th>
      <td>49.772059</td>
      <td>49.714500</td>
      <td>49.761700</td>
      <td>49.545918</td>
    </tr>
  </tbody>
</table>
</div>



### 广播

所谓广播就是一个矢量和一个标量的运算,所有矢量中元素都被同样的操作,pandas可以支持这种操作


```python
data1 = iris_data[:10].copy()
data1
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
    <tr>
      <th>5</th>
      <td>5.4</td>
      <td>3.9</td>
      <td>1.7</td>
      <td>0.4</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4.6</td>
      <td>3.4</td>
      <td>1.4</td>
      <td>0.3</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>7</th>
      <td>5.0</td>
      <td>3.4</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4.4</td>
      <td>2.9</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4.9</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
data1*10
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
      <td>51.0</td>
      <td>35.0</td>
      <td>14.0</td>
      <td>2.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>49.0</td>
      <td>30.0</td>
      <td>14.0</td>
      <td>2.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>47.0</td>
      <td>32.0</td>
      <td>13.0</td>
      <td>2.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>46.0</td>
      <td>31.0</td>
      <td>15.0</td>
      <td>2.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50.0</td>
      <td>36.0</td>
      <td>14.0</td>
      <td>2.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>54.0</td>
      <td>39.0</td>
      <td>17.0</td>
      <td>4.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>46.0</td>
      <td>34.0</td>
      <td>14.0</td>
      <td>3.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>50.0</td>
      <td>34.0</td>
      <td>15.0</td>
      <td>2.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>44.0</td>
      <td>29.0</td>
      <td>14.0</td>
      <td>2.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>49.0</td>
      <td>31.0</td>
      <td>15.0</td>
      <td>1.0</td>
      <td>Iris-setosaIris-setosaIris-setosaIris-setosaIr...</td>
    </tr>
  </tbody>
</table>
</div>




```python
data1["sepal_length"]*10
```




    0    51.0
    1    49.0
    2    47.0
    3    46.0
    4    50.0
    5    54.0
    6    46.0
    7    50.0
    8    44.0
    9    49.0
    Name: sepal_length, dtype: float64



### universal functiion


```python
import numpy as np
```


```python
np.exp(data1["sepal_length"])
f_npexp = np.frompyfunc(lambda x :np.exp(x)+1,1,1)
f_npexp(data1["sepal_length"])
```




    0    165.022
    1     135.29
    2    110.947
    3    100.484
    4    149.413
    5    222.406
    6    100.484
    7    149.413
    8    82.4509
    9     135.29
    Name: sepal_length, dtype: object



## 基本的统计功能

### pandas内置基本的统计功能

函数|作用
---|---
count|非NA值数量
describe|汇总统计
mean|求均值
min/max|最小最大值
argmin/argmax|获取最小最大值的index位置
idxmin/idxmax|获取最小最大值的index
quantile|计算分位数
sum|求和
median|中位数
mad|根据均值计算平局绝对离差
var|方差
std|标准差
skew|偏度(三阶矩)
kurt|锋度(四阶矩)
cumsum|累计和
cummin/cummax|累计最小值累计最大值
cumprod|累计积
diff|一阶差分(对时间序列很有用)
pct_change|百分数变化
corr|相关系数
cov|协方差


```python
iris_data.describe()
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
      <th>count</th>
      <td>150.000000</td>
      <td>150.000000</td>
      <td>150.000000</td>
      <td>150.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>5.843333</td>
      <td>3.054000</td>
      <td>3.758667</td>
      <td>1.198667</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.828066</td>
      <td>0.433594</td>
      <td>1.764420</td>
      <td>0.763161</td>
    </tr>
    <tr>
      <th>min</th>
      <td>4.300000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>0.100000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>5.100000</td>
      <td>2.800000</td>
      <td>1.600000</td>
      <td>0.300000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5.800000</td>
      <td>3.000000</td>
      <td>4.350000</td>
      <td>1.300000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>6.400000</td>
      <td>3.300000</td>
      <td>5.100000</td>
      <td>1.800000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>7.900000</td>
      <td>4.400000</td>
      <td>6.900000</td>
      <td>2.500000</td>
    </tr>
  </tbody>
</table>
</div>




```python
iris_data[["sepal_length","sepal_width","petal_length","petal_width"]].pct_change()[1:].tail()
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
      <th>145</th>
      <td>0.000000</td>
      <td>-0.090909</td>
      <td>-0.087719</td>
      <td>-0.080000</td>
    </tr>
    <tr>
      <th>146</th>
      <td>-0.059701</td>
      <td>-0.166667</td>
      <td>-0.038462</td>
      <td>-0.173913</td>
    </tr>
    <tr>
      <th>147</th>
      <td>0.031746</td>
      <td>0.200000</td>
      <td>0.040000</td>
      <td>0.052632</td>
    </tr>
    <tr>
      <th>148</th>
      <td>-0.046154</td>
      <td>0.133333</td>
      <td>0.038462</td>
      <td>0.150000</td>
    </tr>
    <tr>
      <th>149</th>
      <td>-0.048387</td>
      <td>-0.117647</td>
      <td>-0.055556</td>
      <td>-0.217391</td>
    </tr>
  </tbody>
</table>
</div>




```python
iris_data[["sepal_length","sepal_width","petal_length","petal_width"]].pct_change()[1:].sepal_length.corr(iris_data.petal_length)
```




    0.15569820981689295




```python
iris_data[["sepal_length","sepal_width","petal_length","petal_width"]].corr()
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
      <th>sepal_length</th>
      <td>1.000000</td>
      <td>-0.109369</td>
      <td>0.871754</td>
      <td>0.817954</td>
    </tr>
    <tr>
      <th>sepal_width</th>
      <td>-0.109369</td>
      <td>1.000000</td>
      <td>-0.420516</td>
      <td>-0.356544</td>
    </tr>
    <tr>
      <th>petal_length</th>
      <td>0.871754</td>
      <td>-0.420516</td>
      <td>1.000000</td>
      <td>0.962757</td>
    </tr>
    <tr>
      <th>petal_width</th>
      <td>0.817954</td>
      <td>-0.356544</td>
      <td>0.962757</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
iris_data[["sepal_length","sepal_width","petal_length","petal_width"]].cov()
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
      <th>sepal_length</th>
      <td>0.685694</td>
      <td>-0.039268</td>
      <td>1.273682</td>
      <td>0.516904</td>
    </tr>
    <tr>
      <th>sepal_width</th>
      <td>-0.039268</td>
      <td>0.188004</td>
      <td>-0.321713</td>
      <td>-0.117981</td>
    </tr>
    <tr>
      <th>petal_length</th>
      <td>1.273682</td>
      <td>-0.321713</td>
      <td>3.113179</td>
      <td>1.296387</td>
    </tr>
    <tr>
      <th>petal_width</th>
      <td>0.516904</td>
      <td>-0.117981</td>
      <td>1.296387</td>
      <td>0.582414</td>
    </tr>
  </tbody>
</table>
</div>



### 抽样

抽样的话,pandas提供了sample()方法可以做简单的抽样
你可以选择是有放回还是无放回的


```python
iris_data_test=iris_data.sample(frac=0.4)
iris_data_test = iris_data_test.sort()
iris_data_test[:5]
```

    C:\Users\Administrator\Anaconda3\lib\site-packages\ipykernel\__main__.py:2: FutureWarning: sort(....) is deprecated, use sort_index(.....)
      from ipykernel import kernelapp as app
    




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
      <th>5</th>
      <td>5.4</td>
      <td>3.9</td>
      <td>1.7</td>
      <td>0.4</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>



剩余的数据可以这样得到


```python
iris_data_train=iris_data.drop(iris_data_test.index)
iris_data_train[:5]
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
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>7</th>
      <td>5.0</td>
      <td>3.4</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4.4</td>
      <td>2.9</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4.9</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5.4</td>
      <td>3.7</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>



也可以设定别的你自己的抽样方式,比如我觉得我希望用每行数据摇色子的方式确定是否进入样本,那么可以这样



```python
import random
```


```python
temp = iris_data.copy()
temp["cc"]=[random.random() for i in range(len(iris_data))]
len(iris_data[temp["cc"]>0.3])
```




    110




```python
len(iris_data[temp["cc"]<=0.3])
```




    40


