# 基本符号定义和推导  
$$ x $$ ：表示输入/特征变量是一个维度为(n,1)的数据, 可记为列向量 $$[x_1, x_2, ..., x_n]^T$$ ；  
$$X=[x^{(1)}, x^{(2)}, ..., x^{(m)}]$$：表示所有的训练数据集的输入值，放在一个 $$ n \times m $$ 的矩阵中，其中：  
$$m$$是样本数量，上标 $$(i)$$ 表示第i个样本;  
&emsp; $$X$$展开即： $$ X = \begin{bmatrix} \\
   {x_1}^{(1)} & {x_1}^{(2)} & ... & {x_1}^{(m)} \\
   {x_2}^{(1)} & {x_2}^{(2)} & ... & {x_2}^{(m)} \\
   ... & ... & ... & ... \\
   {x_n}^{(1)} & {x_n}^{(2)} & ... & {x_n}^{(m)} \\
\end{bmatrix} $$

$$y$$ ：表示输出结果，取值为 $$(0,1)$$  ；  
$$ Y=[y^{(1)}, y^{(2)}, ..., y^{(m)}] $$ ：对应表示所有训练数据集的输出值，维度为 $$ 1 \times m $$。  
$$ (x^{(i)}, y^{(i)}) $$ ：表示第 $$i$$ 组数据，是一个单独的样本。是训练数据或测试数据，此处默认为训练数据；  
$$M_{train}$$ 表示训练样本的个数。  
$$M_{test}$$ 表示测试集的样本数

<br>

$$w$$ ：维度跟$$x$$一样是(n,1)的列向量：$$[w_1, w_2, ..., w_n]^T$$   

$$ z = w^T x +b $$  

$$\hat{y}$$：表示对输出结果的预测，表示 $$y$$ 等于1的一种可能性或者是机会。  
逻辑回归 $$\hat{y} = a = \sigma(z) = \sigma ( w^T + b ) = \frac{1}{1+e^{-(w^T x +b)}} $$  
注：a表示激活函数，这里用$$\sigma$$表示sigmoid函数 即$$\sigma(z)= \frac{1}{1+e^{-z}} $$    

$$w$$：特征权重，维度与输入特征向量$X$的维度相同）。  
$$b$$：偏置项参数。

<br>

$$L$$：损失函数Loss function，用来衡量预测输出值和实际值有多接近$$ L( \hat{y},y ) $$  
&emsp; 逻辑回归中用到的损失函数是：  
&emsp; $$ L(\hat{y},y) = -y\log(\hat{y})-(1-y)\log(1-\hat{y}) $$  
&emsp; 或表示为：   
&emsp; $$ L(a,y) = -y\log(a)-(1-y)\log(1-a) $$  
&emsp; 【注意：这里的log是以自然对数e为底的。】  
 

Cost：成本函数/代价函数（ 有时候记为J ），是对$$m$$个样本的损失函数求和然后除以$$m$$。  
&emsp; 逻辑回归损失函数  
&emsp; $$ J( w,b )=\frac{1}{m} \sum\limits_{i=1}^{m} L(a^{(i)}, y^{(i)}) = \frac{1}{m} \sum\limits_{i=1}^{m} [-{y^{(i)}}\log a^{(i)}-(1-y^{(i)})\log (1-a^{(i)})] $$  
**【注：损失函数只适用于单个训练样本，而代价函数是参数的总代价】**

#### 求导推算：  
$$ dz = \frac{dL}{dz} = \frac{dL(a,y)}{dz} = ( \frac{dL(a,y)}{da} ) \cdot (\frac{da}{dz} ) = \frac{d\ (-y\log(a)-(1-y)\log(1-a))}{da} \cdot {\frac{d\ (\frac{1}{1+e^{-z}})}{dz}} $$  
$$ = ( - \frac{y}{a} + \frac{(1 - y)}{(1 - a)}) \cdot {\frac{d\ (\frac{1}{1+e^{-z}})}{dz}} = ( - \frac{y}{a} + \frac{(1 - y)}{(1 - a)}) \cdot  ( - \frac{1}{(1+e^{-z})^2} \cdot e^{-z} \cdot (-1) ) $$  
$$ = ( - \frac{y}{a} + \frac{(1 - y)}{(1 - a)}) \cdot ( \frac{1}{1+e^{-z}} - \frac{1}{(1+e^{-z})^2}  ) =  ( - \frac{y}{a} + \frac{(1 - y)}{(1 - a)}) \cdot (a - a^2) ) $$  
$$ =  ( - \frac{y}{a} + \frac{(1 - y)}{(1 - a)}) \cdot a(1 - a) = -y(1-a) + a(1-y) $$  
$$ = a - y $$  
$$ dw_1 = \frac{dL}{dw_1} = \frac{dL}{dz} \cdot \frac{dz}{dw_1} = dz \cdot \frac{d\ (w^T x + b)}{dw_1} = dz \cdot \frac{d\ (w_1x_1+ w_2 x_2 + ... + b)}{dw_1} = dz \cdot x_1 = x_1 \cdot dz$$  
$$ dw_2 = x_2 \cdot dz $$  
...  
$$ dw_n = x_n \cdot dz $$  
$$ dw = [dw_1, dw_2, ..., dw_n]^T = [x_1 \cdot dz, x_2 \cdot dz, ...,x_n \cdot dz]^T = [x_1, x_2, ..., x_n]^T \cdot dz $$  
$$ = x \cdot dz $$   
$$ db = \frac{dL}{db} = \frac{dL}{dz} \cdot \frac{dz}{db} =  dz \cdot \frac{d\ (w^T x + b)}{db} = dz $$  

#### 单样本梯度下降   
$$ w_n := w_n - \alpha dw_n := w_n - \alpha \cdot x_n \cdot dz $$  
$$ w := w - \alpha dw := w - \alpha \cdot x \cdot dz $$  
$$ b := b - \alpha db := b - \alpha \cdot dz $$  
其中 $$\alpha$$ 是表示学习率（learning rate），用来控制步长。  

#### m个样本的梯度下降  
全局代价函数，实际上是1到$$m$$个样本各个损失的平均，所以它表明全局代价函数对$$w$$的微分，也同样是各项损失对$$w$$微分的平均。  
$$ \frac{\partial J(w, b)}{\partial w_1} = \frac{\partial ( \frac{1}{m} \sum\limits_{i=1}^{m} L(a^{(i)}, y^{(i)}) )} {\partial w_1} = \frac{1}{m} \sum\limits_{i=1}^{m} {x_1}^{(i)} dz^{(i)} $$  
$$ \frac{\partial J(w, b)}{\partial w_2} = \frac{1}{m} \sum\limits_{i=1}^{m} {x_2}^{(i)} dz^{(i)} $$  
...   
$$ \frac{\partial J(w, b)}{\partial w_n} = \frac{1}{m} \sum\limits_{i=1}^{m} {x_n}^{(i)} dz^{(i)} $$  
 

$$ \frac{\partial J(w, b)}{\partial b} = \frac{1}{m} \sum\limits_{i=1}^{m} dz^{(i)} $$  
所以：   
$$ w_n := w_n - \alpha \frac{\partial J(w_n, b)}{\partial w_n} := w_n - \alpha \frac{1}{m} \sum\limits_{i=1}^{m} dz^{(i)}  {x_n}^{(i)}  $$  
$$ b := b - \alpha \frac{\partial J(w, b)}{\partial b} := b - \alpha \frac{1}{m} \sum\limits_{i=1}^{m} dz^{(i)} $$   


**在同一时间内完成一个所有m个训练样本的向量化计算:**   

$$X=[x^{(1)}, x^{(2)}, ..., x^{(m)}]$$  (前面已定义过)   
$$Y=[y^{(1)}, y^{(2)}, ..., y^{(m)}]$$  
$$ Z=[z^{(1)}, z^{(2)}, ..., z^{(m)}] = [w^{T}x^{(1)}+b,w^{T}x^{(2)}+b, ..., w^{T}x^{(m)}+b] = w^{T}X+[b, b, ..., b] $$  
$$ =w^{T}X+b$$  
$$A=[a^{(1)}, a^{(2)}, ..., a^{(m)}] =[\sigma (z^{(1)}), \sigma (z^{(2)}), ..., \sigma (z^{(m)}) ]$$  
$$ =\sigma (Z)$$  
$$dZ=[dz^{(1)},dz^{(2)}, ..., dz^{(m)}]=[a^{(1)}-y^{(1)}, a^{(2)}-y^{(2)}, ..., a^{(m)}-y^{(m)}]$$ 
$$ = A-Y $$  

$$ dw = 
\begin{bmatrix}  
   dw_1 \\ dw_2 \\ ... \\ dw_n 
\end{bmatrix} = 
\begin{bmatrix} 
   \frac{\partial J(w, b)}{\partial w_1} \\
   \frac{\partial J(w, b)}{\partial w_2} \\
   ...  \\
   \frac{\partial J(w, b)}{\partial w_n} 
\end{bmatrix} =  
\begin{bmatrix} 
   \frac{1}{m} \sum\limits_{i=1}^{m} dz^{(i)} {x_1}^{(i)} \\
   \frac{1}{m} \sum\limits_{i=1}^{m} dz^{(i)} {x_2}^{(i)} \\
   ... \\
   \frac{1}{m} \sum\limits_{i=1}^{m} dz^{(i)} {x_n}^{(i)} 
\end{bmatrix}  $$  
$$ = \frac{1}{m} \cdot 
\begin{bmatrix} 
   dz^{(1)} {x_1}^{(1)} + dz^{(2)} {x_1}^{(2)} + ... + dz^{(m)} {x_1}^{(m)} \\
   dz^{(1)} {x_2}^{(1)} + dz^{(2)} {x_2}^{(2)} + ... + dz^{(m)} {x_2}^{(m)} \\
   ... \\
   dz^{(1)} {x_n}^{(1)} + dz^{(2)} {x_n}^{(2)} + ... + dz^{(m)} {x_n}^{(m)} 
\end{bmatrix}  = \frac{1}{m} \cdot 
\begin{bmatrix} 
   {x_1}^{(1)} & {x_1}^{(2)} & ... & {x_1}^{(m)} \\
   {x_2}^{(1)} & {x_2}^{(2)} & ... & {x_2}^{(m)} \\
   ... & ... & ... & ... \\
   {x_n}^{(1)} & {x_n}^{(2)} & ... & {x_n}^{(m)} 
\end{bmatrix} 
\begin{bmatrix}  
   dz^{(1)} \\ dz^{(2)} \\ ... \\ dz^{(m)}  
\end{bmatrix} $$  
$$ = \frac{1}{m} \cdot X \ dZ^T $$  

$$ db = \frac{1}{m} \sum\limits_{i=1}^{m} dz^{(i)} = \frac{1}{m} ( dz^{(1)} + dz^{{2}} + ... + dz^{{m}} ) $$  
$$ \frac{1}{m} * np.sum(dZ) $$  --python  
$$ w_n := w_n - \alpha dw_n  $$  
$$ w := w - \alpha dw $$  


<br>

### 神经网络  

&emsp; <img src="images/base-symbol-1.png" width="300px"/>  

**神经网络的层数**从左到右，由0开始定义，比如上图最左边的$$ x_1, x_2, x_3 $$ 是第0层，也叫做神经网络的**输入层**，这层右边的是第1层，由此类推。最后一层是**输出层**，它负责产生预测值。输入层和输出层中间的都是**隐藏层**，如上图是3个隐藏层的神经网络。  

用$$L$$表示层数，上图$$L=5$$。

使用上标$$[l]$$来标示第$$l$$层，如神经元个数$$n^{[0]}=3$$表示第0层神经元个数为3，$$n^{[1]}=5$$表示第1层隐藏层神经元个数为5，同理，$$n^{[2]}=5$$，$$n^{[3]}=2$$，$$n^{[4]}=n^{[L]}=1$$  
**向前传播过程：**  
$$a$$表示神经元激活值，作为下一层的输入特征。对于每层$$l$$都用$$a^{[l]}$$来记作第$$l$$层激活后结果，即：  
&emsp; $$a{[l]}=\begin{bmatrix} {a_1}^{[l]} \\ {a_2}^{[l]} \\ ... \\ {a_n}^{[l]}\end{bmatrix}$$  输入层 $$x$$也是第0层的激活值即$$x=a^{[0]}$$  ，而输出层也就是最后一层的激活值$$a^{[L]}$$。  

用激活函数$$g$$计算$$z^{[l]}$$，激活函数也被索引为层数$$l$$。同样的，用$$w^{[l]}$$和$$b^{[l]}$$记作在第$$l$$层计算$$z^{[l]}$$值的权重和偏置项。  
$$ z^{[l]} = w^{[l]T} \cdot a^{[l-1]} + b^{[l]} $$  
$$ a^{[l]} = g^{[l]} (z^{[l]}) $$   

m个样本的向前传播向量化实现过程可以写成：  
$$ Z^{[l]} = W^{[l]T} \cdot A^{[l-1]} + b^{[l]} $$  
​$$ A^{[l]} = g^{[l]}(Z^{[l]}) $$  
需要喂入$${A}^{[0]}$$也就是$$X$$，来初始化。  
**向后传播过程：**  
$$ d{z^{[l]}} = d{a^{[l]}} * {g^{[l]}}'({z^{[l]}})$$  
$$ d{w^{[l]}} = d{z^{[l]}} \cdot {a^{[l-1]}} $$  
$$ d{b^{[l]}} = d{z^{[l]}} $$  
$$ d{a^{[l-1]}} = {w^{[l]T}} \cdot {dz^{[l]}} $$  
$$ d{a^{[l-1]}} = \frac{dL}{dz^{[l]}} \cdot \frac{dz^{[l]}}{da^{[l-1]}} = dz^{[l]} \cdot \frac{ w^{[l]T} \cdot a^{[l-1]} + b^{[l]}  }{da^{[l-1]}} = {w^{[l]T}} \cdot {dz^{[l]}} $$  

m个样本的向后传播向量化实现过程可以写成：  
$$ d{Z^{[l]}} = d{A^{[l]}} * {g^{[l]}}'(Z^{[l]}) $$  
$$ d{W^{[l]}} = \frac{1}{m} \cdot d{Z^{[l]}} \cdot {A^{[l-1]T}} $$  
$$ d{b^{[l]}} = \frac{1}{m} \cdot np.sum(d{z^{[l]}},axis=1,keepdims=True) $$  --python  
$$ d{A^{[l-1]}} = {W^{[l]T}} \cdot d{Z^{[l]}} $$  

#### 核对参数的维度    
$$w$$的维度是（下一层的维数，前一层的维数），即$$w^{[l]}$$: ($$n^{[l]},n^{[l-1]}$$)；  
$$b$$的维度是（下一层的维数，1），即:$$b^{[l]}$$ : ($$n^{[l]},1)$$；  
$$z^{[l]}$$,$$a^{[l]}$$的维度都是 : $$(n^{[l]},1)$$;  
$$dw^{[l]}$$和$$w^{[l]}$$维度相同，$$db^{[l]}$$和$$b^{[l]}$$维度相同。  
$$w$$和$$b$$向量化维度不变；
但$$z$$,$$a$$以及$$x$$的维度会向量化后发生变化。  
$$Z^{[l]}$$可以看成由每一个单独的$$Z^{[l]}$$叠加而得到，$$Z^{[l]}=(z^{[l](1)}, z^{[l](2)}, z^{[l](3)}, ..., z^{[l](m)})$$，$$m$$为训练集大小，所以$$Z^{[l]}$$的维度不再是$$(n^{[l]},1)$$，而是$$(n^{[l]},m)$$。  
同理，$$A^{[l]}$$的维度是$$(n^{[l]},m)$$，$$A^{[0]}=X$$ 的维度是$$(n^{[0]},m)$$





