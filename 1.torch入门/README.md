# 一、torch入门

```py
from __future__ import print_function
from torch.utils.data import Dataset  #从常用的工具中导入Dataset
import torch
```

#### 分别使用dir和help函数查看utils信息与Dataset函数介绍

```py
dir(torch.utils)
```

```py
help(Dataset)
```

```py
# 创建一个 5*3 的矩阵
x = torch.empty(5, 3)
print(x)

# 创建一个随机初始化的 5*3 矩阵
rand_x = torch.rand(5, 3)
print(rand_x)

# 创造一个数值皆为0的矩阵，类型为long
zero_x = torch.zeros(5, 3, dtype=torch.long)
print(zero_x)

#数值为[]内容
tensor1 = torch.tensor( [5.5,3,6,7] )
print(tensor1)
```


# 针对tensor的操作

```py
#生成x随机矩阵
x = torch.rand(5,3)
print(x)

#相关操作
x+4
x/3

#生成ytensor
y = torch.tensor([ 5, 4, 3 ])
print(y)

```

#### 由于x是一个5行3列的张量，而y是一个长度为3的张量，根据广播（broadcasting）规则，PyTorch会将y自动扩展为与x具有相同的形状，即5行3列。然后，对应位置上的元素进行乘法运算。

#### 在PyTorch中，*运算符用于逐元素乘法（element-wise multiplication）。当使用*运算符对两个张量进行操作时，它将对应位置的元素进行相乘。注意！矩阵运算符是@或torch.matmul()

tensor([[5, 4, 3],
        [5, 4, 3],
        [5, 4, 3],
        [5, 4, 3],
        [5, 4, 3]])
        
```py
#当使用*运算符对两个张量进行操作时，它将对应位置的元素进行相乘。
x*y

#tensor([[3.7075, 1.1558, 0.3126],
#        [1.8899, 0.4737, 1.4580],
#        [1.8725, 3.6073, 0.2970],
#        [1.3425, 0.5328, 1.5070],
#        [1.4471, 3.0835, 1.9708]])
```

**在PyTorch中，不同的数据类型（或称为张量类型）可以表示不同的数值精度和范围。以下是常见的一些张量类型及其特点：**

- torch.float32（默认类型）：也称为单精度浮点型，使用32位来表示浮点数。它可以表示较大范围的数值，并具有合理的数值精度。大多数情况下，使用torch.float32已经足够满足计算需求。

- torch.float64：也称为双精度浮点型，使用64位来表示浮点数。它提供更高的数值精度，但相应地会占用更多的内存空间。

- torch.int32（或torch.int）：也称为整型，使用32位来表示整数。它适用于整数运算，并且通常在计算中可以提供较好的性能。

- torch.int64：也称为长整型，使用64位来表示整数。它可以表示更大范围的整数，但相应地会占用更多的内存空间。


**在PyTorch中，整型（例如torch.int32、torch.int64）和浮点型（例如torch.float32、torch.float64）之间有几个关键区别：**

- 表示范围：整型可以表示离散的整数值，而浮点型可以表示连续的小数值。整型通常有一个固定的范围，如32位整型（torch.int32）可以表示的范围为约-2,147,483,648到2,147,483,647，64位整型（torch.int64）可以表示的范围更大。浮点型则可以表示更广范围的数值，包括小于1和大于1的值，并且可以具有更高的精度。

- 数值精度：浮点型通常具有更高的数值精度。例如，32位浮点型（torch.float32）使用单精度浮点数表示，提供大约7位有效数字。而64位浮点型（torch.float64）使用双精度浮点数表示，提供大约15位有效数字。相比之下，整型是精确的，不会存在舍入误差。

- 内存占用：浮点型通常占用更多的内存空间。例如，32位浮点型和64位浮点型相比，存储同样数量的元素时会占用两倍的内存空间。整型通常需要较少的内存空间来存储相同数量的元素。

- 运算规则：整型和浮点型在进行数学运算时的规则略有不同。整型运算通常遵循整数运算规则，例如整数相除会向下取整。浮点型运算则遵循浮点数运算规则，包括舍入、溢出等。

- 在选择使用整型还是浮点型时，需要根据具体的应用场景和需求来进行选择。如果需要表示离散的整数值或者要求高精度的小数运算，可以选择整型或浮点型。如果数值范围较大或者需要处理连续的小数值，通常会选择浮点型。

**这些不同的数据类型可以在张量创建时指定，或者使用.type()方法进行类型转换。选择适当的数据类型取决于您的具体需求，需要权衡数值精度和内存消耗之间的平衡。**

**在下述实例中，当矩阵X的数据类型为torch.float32时，为了进行矩阵乘法，需要将向量v的数据类型与之匹配，以确保数据类型一致性，从而避免类型不匹配的错误。**

- torch.rand函数生成的张量默认是torch.float32类型，而torch.tensor默认使用的是torch.int64（Long）类型。在进行矩阵乘法时，张量的类型需要一致。


```py
X = torch.rand(5, 3)  # 5行3列的矩阵
v = torch.tensor([1, 2, 3]).float()  # 长度为3的向量，转换为torch.float32类型(匹配数据类型)

result = torch.matmul(X, v)
print(result)
#tensor([2.9234, 1.6135, 3.7668, 3.4017, 4.8242])

#获取X的类型
#X.type()   与   X.dtype
print( X.type(),"      ", X.dtype )
#torch.FloatTensor        torch.float32

#提取第三列的数据
x[ : , 2 ]
#tensor([0.1042, 0.4860, 0.0990, 0.5023, 0.6569])

x.size()
#torch.Size([5, 3])

#使用 .item() 来获得tensor的数值，一次只能一个
x[ 1, 2 ].item()
#0.48600661754608154
```


