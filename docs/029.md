# NumPy 备忘单—用于数据科学的 Python

> 原文：<https://www.dataquest.io/blog/numpy-cheat-sheet/>

April 13, 2017NumPy is the library that gives Python its ability to work with data at speed. Originally, launched in 1995 as ‘Numeric,’ NumPy is the foundation on which many important Python data science libraries are built, including Pandas, SciPy and scikit-learn. It’s common when first learning NumPy to have trouble remembering all the functions and methods that you need, and while at [Dataquest](https://www.dataquest.io) we advocate getting used to consulting the [NumPy documentation](https://docs.scipy.org/doc/numpy/), sometimes it’s nice to have a handy reference, so we’ve put together this cheat sheet to help you out! If you’re interested in learning NumPy, you can consult our [NumPy tutorial](https://www.dataquest.io/blog/numpy-tutorial-python/) blog post, or you can signup for free and start learning NumPy through our interactive [Python data science course](https://www.dataquest.io/course/python-for-data-science-intermediate/). [Download a Printable PDF of this Cheat Sheet](https://s3.amazonaws.com/dq-blog-files/numpy-cheat-sheet.pdf)

## 密钥和导入

在本备忘单中，我们使用以下简写方式:

`arr` |一个 numpy 数组对象你还需要导入 NumPy 来开始:

```
import numpy as np
```

## 导入/导出

`np.loadtxt('file.txt')` |从文本文件`np.genfromtxt('file.csv',delimiter=',')` |从 CSV 文件`np.savetxt('file.txt',arr,delimiter=' ')` |写入文本文件`np.savetxt('file.csv',arr,delimiter=',')` |写入 CSV 文件

## 创建数组

`np.array([1,2,3])` |一维数组`np.array([(1,2,3),(4,5,6)])` |二维数组`np.zeros(3)` |长度为`3`的 1D 数组`0` `np.ones((3,4))` | `3` x `4`数组`1` `np.eye(5)` | `5` x `5`数组`0`对角线上有`1`的数组【单位矩阵】`np.linspace(0,100,6)` |从`0`到`100` `np.arange(0,10,3)`的`6`数组|从`0`到小于`10`的值数组`3` `2` x `3`数组包含所有值`8` `np.random.rand(4,5)` | `4` x `5`数组包含`0`–`1``np.random.rand(6,7)*100`|`6`x`7`数组包含`0`–`100`–`np.random.randint(5,size=(2,3))`|`2`x`3`数组包含`0`–`4`之间的随机整数

## 检查属性

`arr.size` |返回`arr` `arr.shape`中的元素个数】|返回`arr`(行、列)`arr.dtype`的尺寸|返回`arr` `arr.astype(dtype)`中的元素类型|将`arr`元素转换为类型`dtype` `arr.tolist()` |将`arr`转换为 Python 列表`np.info(np.eye)` |查看`np.eye`的文档

## 复制/分类/整形

`np.copy(arr)` |将`arr`复制到新内存`arr.view(dtype)` |创建类型为`dtype` `arr.sort()`的`arr`元素的视图|排序`arr` `arr.sort(axis=0)` |排序`arr` `two_d_arr.flatten()`的特定轴|将 2D 数组`two_d_arr`变平为 1D `arr.T` |转置`arr`(行变成列，反之亦然)`arr.reshape(3,4)` |将`arr`重新整形为`3`行，`4`列不改变数据`arr.resize((5,6))` |将`arr`形状更改为`5` x `6`和

## 添加/删除元素

`np.append(arr,values)` |将值追加到`arr` `np.insert(arr,2,values)`的末尾|将值插入到索引`2` `np.delete(arr,3,axis=0)`之前的`arr`|删除`arr` `np.delete(arr,4,axis=1)`的索引`3`上的行|删除`arr`的索引`4`上的列

## 合并/拆分

`np.concatenate((arr1,arr2),axis=0)` |将`arr2`作为行添加到`arr1`的末尾`np.concatenate((arr1,arr2),axis=1)` |将`arr2`作为列添加到`arr1`的末尾`np.split(arr,3)` |将`arr`拆分成`3`子数组`np.hsplit(arr,5)` |在第`5`个索引上水平拆分`arr`

## 索引/切片/子集化

`arr[5]` |返回索引`5` `arr[2,5]`处的元素。|返回索引`[2][5]` `arr[1]=4`处的 2D 数组元素。|给索引`1`处的数组元素赋值`4` `arr[1,3]=10` |给索引`[1][3]`处的数组元素赋值`10` `arr[0:3]` |返回索引`0,1,2`处的元素(在 2D 数组中:返回行`0,1,2` ) `arr[0:3,4]` |返回行`0,1,2`处列`4` `arr[:2]`处的元素。| 返回索引`0,1`处的元素(在 2D 数组中:返回行`0,1` ) `arr[:,1]` |返回所有行`arr<5`中索引`1`处的元素|返回布尔值数组`(arr1<3) & (arr2>5)` |返回布尔值数组`~arr` |反转布尔值数组`arr[arr<5]` |返回小于`5`的数组元素

## 标量数学

`np.add(arr,1)` |每个数组元素加`1``np.subtract(arr,2)`|每个数组元素减`2``np.multiply(arr,3)`|每个数组元素乘`3` `np.divide(arr,4)` |每个数组元素除`4`(返回`np.nan`除以零)`np.power(arr,5)` |每个数组元素的`5`次幂

## 向量数学

`np.add(arr1,arr2)` |逐元素地将`arr2`加到`arr1` `np.subtract(arr1,arr2)` |逐元素地从`arr1` `np.multiply(arr1,arr2)`中减去`arr2`|逐元素地将`arr1`乘以`arr2` `np.divide(arr1,arr2)` |逐元素地将`arr1`除以`arr2` `np.power(arr1,arr2)` |逐元素地提升`arr1`到`arr2` `np.array_equal(arr1,arr2)`的幂|如果数组具有相同的元素和形状则返回`True``np.sqrt(arr)`|数组中每个元素的平方根`np.sin(arr)` |数组中每个元素的正弦`np.log(arr)` | 数组中每个元素的自然对数`np.abs(arr)` |数组中每个元素的绝对值`np.ceil(arr)` |上舍入到最近的整数`np.floor(arr)` |下舍入到最近的整数`np.round(arr)` |上舍入到最近的整数

## 统计数据

`np.mean(arr,axis=0)` |返回特定轴的平均值`arr.sum()` |返回`arr`和`arr.min()` |返回`arr`的最小值`arr.max(axis=0)` |返回特定轴的最大值`np.var(arr)` |返回数组的方差`np.std(arr,axis=1)` |返回特定轴的标准差`arr.corrcoef()` |返回数组的相关系数

## 下载此备忘单的可打印版本

如果你想下载这个备忘单的打印版本，你可以在下面做。

[下载此备忘单的可打印 PDF 文件](https://s3.amazonaws.com/dq-blog-files/numpy-cheat-sheet.pdf)