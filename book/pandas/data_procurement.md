
# 数据获取与保存

数据的来源途径无非几种

+ 从网上直接爬取,这个属于写爬虫,不是本文范围
+ 从原生的Python数据结构中获取
+ 从json中获取
+ 数据库中调取,
+ 从excel中读取,
+ 从csv文本文件中获得.
+ 通过pickle序列化数据

而存储数据也无非以下几种方式:

+ 通过pickle序列化数据
+ 保存为json
+ 保存到数据库
+ 保存为excel
+ 保存为csv


pandas针对上面的每种获取途径,都提供了方便的获取方式


```python
import pandas as pd
```

## 从原生Python数据结构中获取

### 从二维列表转换

pandas可以将二维列表以表格的形式组合


```python
names = ['Bob','Jessica','Mary','John','Mel']
births = [1968, 1955, 1977,1978, 1973]
weight = [69,89,76,90,78]
table_o = list(zip(names,births,weight))
```


```python
table_o
```




    [('Bob', 1968, 69),
     ('Jessica', 1955, 89),
     ('Mary', 1977, 76),
     ('John', 1978, 90),
     ('Mel', 1973, 78)]




```python
pd.DataFrame(table_o,columns =["name","births","weight"])# columns指定列标签
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>births</th>
      <th>weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>1968</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jessica</td>
      <td>1955</td>
      <td>89</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mary</td>
      <td>1977</td>
      <td>76</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John</td>
      <td>1978</td>
      <td>90</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Mel</td>
      <td>1973</td>
      <td>78</td>
    </tr>
  </tbody>
</table>
</div>



### 从字典中直接生成

pandas也允许将数据放在字典内,这样key是每列的标题,value就会按顺序填入


```python
table_dict = {"names":['Bob','Jessica','Mary','John','Mel'],
    "births":[1968, 1955, 1977,1978, 1973],
    "weight":[69,89,76,90,78]
}
table_dict
```




    {'births': [1968, 1955, 1977, 1978, 1973],
     'names': ['Bob', 'Jessica', 'Mary', 'John', 'Mel'],
     'weight': [69, 89, 76, 90, 78]}




```python
pd.DataFrame(table_dict)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>births</th>
      <th>names</th>
      <th>weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1968</td>
      <td>Bob</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1955</td>
      <td>Jessica</td>
      <td>89</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1977</td>
      <td>Mary</td>
      <td>76</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1978</td>
      <td>John</td>
      <td>90</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1973</td>
      <td>Mel</td>
      <td>78</td>
    </tr>
  </tbody>
</table>
</div>



### 从包裹字典的列表中获取

另一种则是按行获取,每个字典是表格中的一行


```python
table_row = [{"names":'Bob',
    "births":1968,
    "weight":69
},
{"names":'Jessica',
    "births":1955,
    "weight":89
},
{"names":'Mary',
    "births":1977,
    "weight":76
},
{"names":'John',
    "births":1978,
    "weight":90
},
{"names":'Mel',
    "births": 1973,
    "weight":78
}]
table_row
```




    [{'births': 1968, 'names': 'Bob', 'weight': 69},
     {'births': 1955, 'names': 'Jessica', 'weight': 89},
     {'births': 1977, 'names': 'Mary', 'weight': 76},
     {'births': 1978, 'names': 'John', 'weight': 90},
     {'births': 1973, 'names': 'Mel', 'weight': 78}]




```python
pd.DataFrame(table_row)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>births</th>
      <th>names</th>
      <th>weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1968</td>
      <td>Bob</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1955</td>
      <td>Jessica</td>
      <td>89</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1977</td>
      <td>Mary</td>
      <td>76</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1978</td>
      <td>John</td>
      <td>90</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1973</td>
      <td>Mel</td>
      <td>78</td>
    </tr>
  </tbody>
</table>
</div>



## 从csv文件中读取和保存

所谓csv文件是指使用特定符号分隔数据属性,换行分隔不同数据的文本文件.
这次的例子我们主要用的数据便来自于此.

我们惯例的用[iris](https://archive.ics.uci.edu/ml/machine-learning-databases/iris/)来作为是数据集.下载后放入`./source/`

让我先看看该数据是什么样子


```python
with open("./source/iris.data") as f:
    print(f.readline())
```

    5.1,3.5,1.4,0.2,Iris-setosa
    
    

在看看各个列代表的意思吧


```python
with open("./source/iris.names","r") as names:
    lines = names.readlines()
    print("".join(lines[49:58]))

```

    7. Attribute Information:
       1. sepal length in cm
       2. sepal width in cm
       3. petal length in cm
       4. petal width in cm
       5. class: 
          -- Iris Setosa
          -- Iris Versicolour
          -- Iris Virginica
    
    

可见它没有头部说明,所以我们这样读取该文件:


```python
iris_data = pd.read_csv("./source/iris.data",header = None,encoding = "utf-8",
                        names=["sepal_length","sepal_width","petal_length","petal_width","class"])
iris_data[:5]#取前5行
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



### 保存到csv


```python
pd.DataFrame(table_row).to_csv("source/people.csv",index=False)
```


```python
iris_data.to_csv("source/iris.csv",index=False)# 记得不要把序号写进去
```


```python
new_iris_data = pd.read_csv("source/iris.csv")
```


```python
new_iris_data[:5]
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



## 从json中获取和保存

先随便来个json格式的文件,我们就自己写个例子`people.json`为例好了,该文件放在`./data/`文件夹下,看下内容:


```python
with open("./source/people.json") as f:
    print(f.readline())
```

    [{"name":"Michael"},{"name":"Andy", "age":30},{"name":"Justin", "age":19}]
    
    


```python
people_from_jsonfile = pd.read_json("./source/people.json")
people_from_jsonfile
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>Michael</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30.0</td>
      <td>Andy</td>
    </tr>
    <tr>
      <th>2</th>
      <td>19.0</td>
      <td>Justin</td>
    </tr>
  </tbody>
</table>
</div>



### 保存为json

要保存为json格式,也只需要是用`to_json`方法即可


```python
people_from_jsonfile.to_json()
```




    '{"age":{"0":null,"1":30.0,"2":19.0},"name":{"0":"Michael","1":"Andy","2":"Justin"}}'




```python
people_from_jsonfile.to_json("./source/people_cp.json")
```


```python
new_peoplefrom_jsonfile = pd.read_json("./source/people_cp.json")
new_peoplefrom_jsonfile
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>Michael</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30.0</td>
      <td>Andy</td>
    </tr>
    <tr>
      <th>2</th>
      <td>19.0</td>
      <td>Justin</td>
    </tr>
  </tbody>
</table>
</div>



## 数据库中读取 (需要SQLAlchemy库)和保存

我们以python自带的sqlite3来作测试

先创建一个数据库,还是用我们的`people.json`中的数据,如何制作具体看

制作好的`people.db`依然放在`./source`文件夹下


```python
from sqlalchemy import create_engine
conn = create_engine("sqlite:///source/people.db")
conn
```




    Engine(sqlite:///source/people.db)



### 读取数据库中的表


```python
people_from_db = pd.read_sql('people', conn)
people_from_db
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
  </tbody>
</table>
</div>



### 使用查询语句获得数据


```python
peole_from_db_query = pd.read_sql_query('SELECT * FROM people', conn)
peole_from_db_query
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
  </tbody>
</table>
</div>



### 保存到数据库


```python
peole_from_db_query.to_sql('new_people3', conn)
```


```python
new_people_from_db = pd.read_sql('new_people3', conn)
```


```python
new_people_from_db
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>name</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Michael</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Andy</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Justin</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>
</div>



## 从excel中读取数据(需要xlrd)和保存

一样的我们还是拿`people`作为数据,创建一个excel文件放入`./source`


```python
# using the ExcelFile class
xls = pd.ExcelFile('./source/people.xlsx')
data_fromExcel = xls.parse(u'工作表1', index_col=None, na_values=['NA'])
data_fromExcel
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
  </tbody>
</table>
</div>




```python
people_fromExcel = pd.read_excel('./source/people.xlsx', u'工作表1', index_col=None, na_values=['NA'])
people_fromExcel
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
  </tbody>
</table>
</div>



保存到excel


```python
iris_data.to_excel('source/iris.xlsx', sheet_name='Sheet1',index=False)
```

## 通过pickle序列化数据

要将表格序列化只需要使用`to_pickle`方法就好


```python
iris_data.to_pickle("source/iris.pickle")
```

读取也是只要`pd.read_pickle(path)`即可


```python
iris_data_copy = pd.read_pickle("source/iris.pickle")
```


```python
iris_data_copy[:5]
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


