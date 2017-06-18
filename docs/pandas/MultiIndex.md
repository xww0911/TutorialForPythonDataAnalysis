
# 复合索引

除了常规的索引方式,pandas还可以定义复合索引


```python
import pandas as pd
import numpy as np
```

## 分层索引

分层/多级索引是非常令人兴奋的，因为它打开了一些非常复杂的数据分析和操作的门，尤其是对于更高维数据的处理。
实质上，它使你能够在诸如Series（1d）和DataFrame（2d）的低维数据结构中存储和操作具有任意数量维度的数据。

### 创建分层索引

创建分层索引可以使用

+ `pd.MultiIndex.from_tuples` 从元祖创建

+ `pd.MultiIndex.from_product`当你想要在两个迭代中的每个元素的配对时可以使用

+ 为了方便，可以将数组列表直接传递到Series或DataFrame，以自动构建MultiIndex：


```python
arrays = [['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'],
          ['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']]
```


```python
tuples = list(zip(*arrays))
```


```python
tuples
```




    [('bar', 'one'),
     ('bar', 'two'),
     ('baz', 'one'),
     ('baz', 'two'),
     ('foo', 'one'),
     ('foo', 'two'),
     ('qux', 'one'),
     ('qux', 'two')]




```python
index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])
```


```python
index
```




    MultiIndex(levels=[['bar', 'baz', 'foo', 'qux'], ['one', 'two']],
               labels=[[0, 0, 1, 1, 2, 2, 3, 3], [0, 1, 0, 1, 0, 1, 0, 1]],
               names=['first', 'second'])




```python
s = pd.Series(np.random.randn(8), index=index)
```


```python
s
```




    first  second
    bar    one       1.924541
           two      -2.127108
    baz    one       0.417056
           two      -0.378908
    foo    one       0.780429
           two       1.540210
    qux    one       0.689353
           two       1.101065
    dtype: float64




```python
iterables = [['bar', 'baz', 'foo', 'qux'], ['one', 'two']]

pd.MultiIndex.from_product(iterables, names=['first', 'second'])
```




    MultiIndex(levels=[['bar', 'baz', 'foo', 'qux'], ['one', 'two']],
               labels=[[0, 0, 1, 1, 2, 2, 3, 3], [0, 1, 0, 1, 0, 1, 0, 1]],
               names=['first', 'second'])




```python
arrays = [np.array(['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux']),
          np.array(['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two'])]
s = pd.Series(np.random.randn(8), index=arrays)
s
```




    bar  one   -1.221834
         two   -0.816061
    baz  one    0.134687
         two    0.143186
    foo  one    0.990110
         two    0.208515
    qux  one    1.150871
         two    0.546428
    dtype: float64




```python
df = pd.DataFrame(np.random.randn(8, 4), index=arrays)
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>one</th>
      <td>-1.690183</td>
      <td>0.271540</td>
      <td>-0.519416</td>
      <td>-0.109875</td>
    </tr>
    <tr>
      <th>two</th>
      <td>1.144788</td>
      <td>0.429944</td>
      <td>-0.466847</td>
      <td>-0.475691</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>-1.384935</td>
      <td>0.359243</td>
      <td>0.443673</td>
      <td>0.283476</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.922243</td>
      <td>-0.731361</td>
      <td>0.955463</td>
      <td>-0.315289</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">foo</th>
      <th>one</th>
      <td>-0.725888</td>
      <td>0.455123</td>
      <td>-0.978464</td>
      <td>-0.340143</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.147488</td>
      <td>-0.077069</td>
      <td>0.386868</td>
      <td>-0.910659</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">qux</th>
      <th>one</th>
      <td>-0.360796</td>
      <td>1.773004</td>
      <td>-0.088235</td>
      <td>0.107931</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.107048</td>
      <td>0.659507</td>
      <td>-0.983554</td>
      <td>1.220696</td>
    </tr>
  </tbody>
</table>
</div>



### 将复合索引应用于列


```python
df = pd.DataFrame(np.random.randn(3, 8), index=['A', 'B', 'C'], columns=index)
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>first</th>
      <th colspan="2" halign="left">bar</th>
      <th colspan="2" halign="left">baz</th>
      <th colspan="2" halign="left">foo</th>
      <th colspan="2" halign="left">qux</th>
    </tr>
    <tr>
      <th>second</th>
      <th>one</th>
      <th>two</th>
      <th>one</th>
      <th>two</th>
      <th>one</th>
      <th>two</th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>-0.623886</td>
      <td>-1.210745</td>
      <td>-1.247687</td>
      <td>-1.775733</td>
      <td>0.071501</td>
      <td>-0.559662</td>
      <td>-1.227424</td>
      <td>0.048207</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.539847</td>
      <td>1.144010</td>
      <td>-0.602236</td>
      <td>0.712611</td>
      <td>1.933703</td>
      <td>0.761082</td>
      <td>0.873890</td>
      <td>-0.211337</td>
    </tr>
    <tr>
      <th>C</th>
      <td>-0.360050</td>
      <td>0.518990</td>
      <td>-0.336434</td>
      <td>-1.005913</td>
      <td>1.455619</td>
      <td>-0.156915</td>
      <td>-0.131643</td>
      <td>-1.106066</td>
    </tr>
  </tbody>
</table>
</div>



MultiIndex的重要性在于，它允许您进行分组，选择和重塑操作，我们将在下面和文档的后续部分中进行描述。正如你将在后面部分看到的，你可以发现自己使用分层索引的数据，而不需要自己创建一个MultiIndex。但是，从文件加载数据时，您可能希望在准备数据集时生成自己的MultiIndex。请注意，如何通过使用pandas.set_printoptions中的multi_sparse选项进行控制来显示索引：


```python
pd.set_option('display.multi_sparse', False)
```


```python
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>first</th>
      <th>bar</th>
      <th>bar</th>
      <th>baz</th>
      <th>baz</th>
      <th>foo</th>
      <th>foo</th>
      <th>qux</th>
      <th>qux</th>
    </tr>
    <tr>
      <th>second</th>
      <th>one</th>
      <th>two</th>
      <th>one</th>
      <th>two</th>
      <th>one</th>
      <th>two</th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>-0.623886</td>
      <td>-1.210745</td>
      <td>-1.247687</td>
      <td>-1.775733</td>
      <td>0.071501</td>
      <td>-0.559662</td>
      <td>-1.227424</td>
      <td>0.048207</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.539847</td>
      <td>1.144010</td>
      <td>-0.602236</td>
      <td>0.712611</td>
      <td>1.933703</td>
      <td>0.761082</td>
      <td>0.873890</td>
      <td>-0.211337</td>
    </tr>
    <tr>
      <th>C</th>
      <td>-0.360050</td>
      <td>0.518990</td>
      <td>-0.336434</td>
      <td>-1.005913</td>
      <td>1.455619</td>
      <td>-0.156915</td>
      <td>-0.131643</td>
      <td>-1.106066</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.set_option('display.multi_sparse', True)
```

## 重建级别标签

方法`get_level_values`将返回特定级别上每个位置的标签的向量


```python
index.get_level_values(0)
```




    Index(['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'], dtype='object', name='first')




```python
index.get_level_values('second')
```




    Index(['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two'], dtype='object', name='second')



## 使用MultiIndex在轴上进行基本索引

分层索引的一个重要特征是您可以通过标识数据中子组的“部分”标签来选择数据。部分选择“丢弃”层次索引的水平在结果中以一种完全类似的方式选择常规DataFrame中的列：


```python
df['bar']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>second</th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>-0.623886</td>
      <td>-1.210745</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.539847</td>
      <td>1.144010</td>
    </tr>
    <tr>
      <th>C</th>
      <td>-0.360050</td>
      <td>0.518990</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['bar', 'one']
```




    A   -0.623886
    B    0.539847
    C   -0.360050
    Name: (bar, one), dtype: float64




```python
df['bar']['one']
```




    A   -0.623886
    B    0.539847
    C   -0.360050
    Name: one, dtype: float64




```python
s['qux']
```




    one    1.150871
    two    0.546428
    dtype: float64




```python
df.columns
```




    MultiIndex(levels=[['bar', 'baz', 'foo', 'qux'], ['one', 'two']],
               labels=[[0, 0, 1, 1, 2, 2, 3, 3], [0, 1, 0, 1, 0, 1, 0, 1]],
               names=['first', 'second'])




```python
df[['foo','qux']].columns
```




    MultiIndex(levels=[['bar', 'baz', 'foo', 'qux'], ['one', 'two']],
               labels=[[2, 2, 3, 3], [0, 1, 0, 1]],
               names=['first', 'second'])



这样做是为了避免重新计算水平以便使切片具有高性能。如果你想看到实际使用的水平。


```python
df[['foo','qux']].columns.values
```




    array([('foo', 'one'), ('foo', 'two'), ('qux', 'one'), ('qux', 'two')], dtype=object)




```python
df[['foo','qux']].columns.get_level_values(0)
```




    Index(['foo', 'foo', 'qux', 'qux'], dtype='object', name='first')




```python
pd.MultiIndex.from_tuples(df[['foo','qux']].columns.values)
```




    MultiIndex(levels=[['foo', 'qux'], ['one', 'two']],
               labels=[[0, 0, 1, 1], [0, 1, 0, 1]])



## 数据对齐和使用reindex

在轴上具有多索引的不同索引对象之间的操作将如期望地工作;数据对齐将像元组索引一样工作：


```python
s + s[:-2]
```




    bar  one   -2.443668
         two   -1.632123
    baz  one    0.269374
         two    0.286373
    foo  one    1.980220
         two    0.417029
    qux  one         NaN
         two         NaN
    dtype: float64




```python
 s + s[::2]
```




    bar  one   -2.443668
         two         NaN
    baz  one    0.269374
         two         NaN
    foo  one    1.980220
         two         NaN
    qux  one    2.301741
         two         NaN
    dtype: float64



reindex可以用另一个MultiIndex或甚至一个元组的列表或数组调用：


```python
s.reindex(index[:3])
```




    first  second
    bar    one      -1.221834
           two      -0.816061
    baz    one       0.134687
    dtype: float64




```python
s.reindex([('foo', 'two'), ('bar', 'one'), ('qux', 'one'), ('baz', 'one')])
```




    foo  two    0.208515
    bar  one   -1.221834
    qux  one    1.150871
    baz  one    0.134687
    dtype: float64



## 高级索引与层次索引

使用.loc / .ix在高级索引中语法集成MultiIndex有点具有挑战性，但我们已尽一切努力这样做。例如下面的工作，你会期望：


```python
df = df.T
```


```python
df
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
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>one</th>
      <td>-0.623886</td>
      <td>0.539847</td>
      <td>-0.360050</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.210745</td>
      <td>1.144010</td>
      <td>0.518990</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>-1.247687</td>
      <td>-0.602236</td>
      <td>-0.336434</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.775733</td>
      <td>0.712611</td>
      <td>-1.005913</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">foo</th>
      <th>one</th>
      <td>0.071501</td>
      <td>1.933703</td>
      <td>1.455619</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.559662</td>
      <td>0.761082</td>
      <td>-0.156915</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">qux</th>
      <th>one</th>
      <td>-1.227424</td>
      <td>0.873890</td>
      <td>-0.131643</td>
    </tr>
    <tr>
      <th>two</th>
      <td>0.048207</td>
      <td>-0.211337</td>
      <td>-1.106066</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc['bar']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
    <tr>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>-0.623886</td>
      <td>0.539847</td>
      <td>-0.36005</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.210745</td>
      <td>1.144010</td>
      <td>0.51899</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc['bar', 'two']
```




    A   -1.210745
    B    1.144010
    C    0.518990
    Name: (bar, two), dtype: float64




```python
df.loc['baz':'foo']
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
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>-1.247687</td>
      <td>-0.602236</td>
      <td>-0.336434</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.775733</td>
      <td>0.712611</td>
      <td>-1.005913</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">foo</th>
      <th>one</th>
      <td>0.071501</td>
      <td>1.933703</td>
      <td>1.455619</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.559662</td>
      <td>0.761082</td>
      <td>-0.156915</td>
    </tr>
  </tbody>
</table>
</div>



你可以通过提供一个元组的切片，使用一个“范围”的值。


```python
df.loc[('baz', 'two'):('qux', 'one')]
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
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>baz</th>
      <th>two</th>
      <td>-1.775733</td>
      <td>0.712611</td>
      <td>-1.005913</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">foo</th>
      <th>one</th>
      <td>0.071501</td>
      <td>1.933703</td>
      <td>1.455619</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.559662</td>
      <td>0.761082</td>
      <td>-0.156915</td>
    </tr>
    <tr>
      <th>qux</th>
      <th>one</th>
      <td>-1.227424</td>
      <td>0.873890</td>
      <td>-0.131643</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[('baz', 'two'):'foo']
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
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>baz</th>
      <th>two</th>
      <td>-1.775733</td>
      <td>0.712611</td>
      <td>-1.005913</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">foo</th>
      <th>one</th>
      <td>0.071501</td>
      <td>1.933703</td>
      <td>1.455619</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.559662</td>
      <td>0.761082</td>
      <td>-0.156915</td>
    </tr>
  </tbody>
</table>
</div>



传递标签或元组的列表与重建索引类似：


```python
df.ix[[('bar', 'two'), ('qux', 'one')]]
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
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>bar</th>
      <th>two</th>
      <td>-1.210745</td>
      <td>1.14401</td>
      <td>0.518990</td>
    </tr>
    <tr>
      <th>qux</th>
      <th>one</th>
      <td>-1.227424</td>
      <td>0.87389</td>
      <td>-0.131643</td>
    </tr>
  </tbody>
</table>
</div>



## 使用swaplevel（）交换级别


```python
df[:5]
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
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>one</th>
      <td>-0.623886</td>
      <td>0.539847</td>
      <td>-0.360050</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.210745</td>
      <td>1.144010</td>
      <td>0.518990</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>-1.247687</td>
      <td>-0.602236</td>
      <td>-0.336434</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.775733</td>
      <td>0.712611</td>
      <td>-1.005913</td>
    </tr>
    <tr>
      <th>foo</th>
      <th>one</th>
      <td>0.071501</td>
      <td>1.933703</td>
      <td>1.455619</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[:5].swaplevel(0, 1, axis=0)
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
    </tr>
    <tr>
      <th>second</th>
      <th>first</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <th>bar</th>
      <td>-0.623886</td>
      <td>0.539847</td>
      <td>-0.360050</td>
    </tr>
    <tr>
      <th>two</th>
      <th>bar</th>
      <td>-1.210745</td>
      <td>1.144010</td>
      <td>0.518990</td>
    </tr>
    <tr>
      <th>one</th>
      <th>baz</th>
      <td>-1.247687</td>
      <td>-0.602236</td>
      <td>-0.336434</td>
    </tr>
    <tr>
      <th>two</th>
      <th>baz</th>
      <td>-1.775733</td>
      <td>0.712611</td>
      <td>-1.005913</td>
    </tr>
    <tr>
      <th>one</th>
      <th>foo</th>
      <td>0.071501</td>
      <td>1.933703</td>
      <td>1.455619</td>
    </tr>
  </tbody>
</table>
</div>



## 使用reorder_levels（）重新排序级别


```python
df[:5].reorder_levels([1,0], axis=0)
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
    </tr>
    <tr>
      <th>second</th>
      <th>first</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <th>bar</th>
      <td>-0.623886</td>
      <td>0.539847</td>
      <td>-0.360050</td>
    </tr>
    <tr>
      <th>two</th>
      <th>bar</th>
      <td>-1.210745</td>
      <td>1.144010</td>
      <td>0.518990</td>
    </tr>
    <tr>
      <th>one</th>
      <th>baz</th>
      <td>-1.247687</td>
      <td>-0.602236</td>
      <td>-0.336434</td>
    </tr>
    <tr>
      <th>two</th>
      <th>baz</th>
      <td>-1.775733</td>
      <td>0.712611</td>
      <td>-1.005913</td>
    </tr>
    <tr>
      <th>one</th>
      <th>foo</th>
      <td>0.071501</td>
      <td>1.933703</td>
      <td>1.455619</td>
    </tr>
  </tbody>
</table>
</div>



# CategoricalIndex

我们介绍一个CategoricalIndex，一种新类型的索引对象，用于支持索引与重复。这是围绕分类（在v0.15.0中引入）的容器，并且允许对具有大量重复元素的索引进行有效的索引和存储。在0.16.1之前，使用类别dtype设置DataFrame / Series的索引会将其转换为常规的基于对象的Index。


```python
df = pd.DataFrame({'A': np.arange(6),
                   'B': list('aabbca')})


df['B'] = df['B'].astype('category', categories=list('cab'))

```


```python
df
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
      <th>0</th>
      <td>0</td>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>a</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>b</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>b</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>c</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>a</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.dtypes
```




    A       int32
    B    category
    dtype: object




```python
df.B.cat.categories
```




    Index(['c', 'a', 'b'], dtype='object')



设置索引，将创建一个CategoricalIndex


```python
df2 = df.set_index('B')
```


```python
df2.index
```




    CategoricalIndex(['a', 'a', 'b', 'b', 'c', 'a'], categories=['c', 'a', 'b'], ordered=False, name='B', dtype='category')




```python
df2.loc['a']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
    </tr>
    <tr>
      <th>B</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0</td>
    </tr>
    <tr>
      <th>a</th>
      <td>1</td>
    </tr>
    <tr>
      <th>a</th>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



这些保留了分类索引


```python
df2.loc['a'].index
```




    CategoricalIndex(['a', 'a', 'a'], categories=['c', 'a', 'b'], ordered=False, name='B', dtype='category')




```python
df2.sort_index()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
    </tr>
    <tr>
      <th>B</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>c</th>
      <td>4</td>
    </tr>
    <tr>
      <th>a</th>
      <td>0</td>
    </tr>
    <tr>
      <th>a</th>
      <td>1</td>
    </tr>
    <tr>
      <th>a</th>
      <td>5</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
    </tr>
    <tr>
      <th>b</th>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



索引上的Groupby操作也将保留索引本质


```python
df2.groupby(level=0).sum()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
    </tr>
    <tr>
      <th>B</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>c</th>
      <td>4</td>
    </tr>
    <tr>
      <th>a</th>
      <td>6</td>
    </tr>
    <tr>
      <th>b</th>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.groupby(level=0).sum().index
```




    CategoricalIndex(['c', 'a', 'b'], categories=['c', 'a', 'b'], ordered=False, name='B', dtype='category')



重索引操作将根据传递的索引器的类型返回一个结果索引，这意味着传递一个列表将返回一个普通的索引;使用分类索引将返回CategoricalIndex，根据PASSED分类类型的类别索引。这允许任意索引这些甚至与不在类别中的值，类似于您可以重新索引任何pandas索引。


```python
df2.reindex(['a','e'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
    </tr>
    <tr>
      <th>B</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>a</th>
      <td>1.0</td>
    </tr>
    <tr>
      <th>a</th>
      <td>5.0</td>
    </tr>
    <tr>
      <th>e</th>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.reindex(['a','e']).index
```




    Index(['a', 'a', 'a', 'e'], dtype='object', name='B')




```python
df2.reindex(pd.Categorical(['a','e'],categories=list('abcde')))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
    </tr>
    <tr>
      <th>B</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>a</th>
      <td>1.0</td>
    </tr>
    <tr>
      <th>a</th>
      <td>5.0</td>
    </tr>
    <tr>
      <th>e</th>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


