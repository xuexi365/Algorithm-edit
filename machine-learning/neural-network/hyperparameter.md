# 超参数

&emsp; &emsp; 超参数是在开始学习过程之前设置值的参数，而不是通过训练得到的参数数据。这些数字实际上控制了最后的参数$$W$$和$$b$$的值，所以它们被称作超参数。通常情况下，需要对超参数进行优化，通过设置不同的值，训练不同的模型和选择更好的测试值来决定。  

### $$a$$学习率(learning rate)  
### $$L$$（隐藏层数目）   
### $$n^{[l]}$$（隐藏层单元数目）    
### 激活函数（choice of activation function）   
&emsp; &emsp; 为什么神经网络需要非线性激活函数?因为这多个线性函数的组合本身也是线性函数，如果全部使用线性激活函数，那么无论神经网络有多少层一直在做的只是计算线性函数，还不如直接去掉全部隐藏层。常见激活函数有：  

**sigmoid函数:** $$ a = \sigma(z) = \frac{1}{ {1+e}^{-z} } $$  
<div style="margin-left:30px">  

优点：  
* 输出为0到1之间的连续实值，此输出范围和概率范围一致，因此可以用概率的方式解释输出。  
* 将线性函数转变为非线性函数  

缺点：  
* 容易出现梯度消失
* 函数输出并不是zero-centered
* 幂运算相对来讲比较耗时  
</div>

**tanh函数:**  $$ a= tanh(z) = \frac{e^{z} - e^{- z}}{e^{z} + e^{- z}}$$  
<div style="margin-left:30px">  

事实上，tanh函数是sigmoid的向下平移和伸缩后的结果, $$tanh(z) = 2 \cdot \sigma(2x) - 1 $$   
优点：  
* 对比sigmoid和tanh两者导数输出可知，tanh函数的导数比sigmoid函数导数值更大，即梯度变化更快，也就是在训练过程中收敛速度更快。  
* 输出范围为-1到1之间，这样可以使得输出均值为0，这个性质可以提高BP训练的效率，具体原因参考文献 http://yann.lecun.com/exdb/publis/pdf/lecun-98b.pdf  
* 将线性函数转变为非线性函数  

缺点：  
* 容易出现梯度消失  
* 幂运算相对来讲比较耗时  
</div>

**ReLu函数:** $$ a =max(0,z) $$  
<div style="margin-left:30px">  

ReLU函数代表的的是“修正线性单元”。
当x <= 0时，激活值为0，对应的导数也为0；  
当x > 0时，激活等于输入值，对应的导数为1。  
优点：  
* 解决了梯度消失问题 (在正区间)  
* 计算速度非常快，只需要判断输入是否大于0
* 收敛速度远快于sigmoid和tanh

缺点：  
* 不是zero-centered
* 某些神经元可能永远不会被激活

</div>

**LRelu函数（Leaky Relu函数）：**  $$a = max( leaky*z,z) $$ leak是一个预习设定的很小的常数。     
<div style="margin-left:30px">  

Leaky ReLU是给所有负值赋予一个非零斜率，而不是像ReLU将所有的负值都设为零。 

LReLu变体   
>  
>**PReLU函数：**  
PReLU即“参数化修正线性单元”，负值部分的斜率是根据数据来定的，而非预先定义的。
>
>**RReLU函数：**  
RReLU即“随机纠正线性单元”，负值部分的斜率在训练中是随机的，在之后的测试中就变成了固定的了。  
>**CReLu函数**  
</div>

**ELU函数**  
**SELU函数**    

### iterations(梯度下降法循环的数量)    
### momentum  
### mini batch size  
### regularization parameters  

