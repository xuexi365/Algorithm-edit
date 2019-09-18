# 超参数

&emsp; &emsp; 超参数是在开始学习过程之前设置值的参数，而不是通过训练得到的参数数据。这些数字实际上控制了最后的参数$$W$$和$$b$$的值，所以它们被称作超参数。通常情况下，需要对超参数进行优化，通过设置不同的值，训练不同的模型和选择更好的测试值来决定。  

<br>

### $$a$$学习率(learning rate)  
&emsp; &emsp; 学习速率可以认为是最重要的超参数。  

&emsp; &emsp; **手动设置学习率**  
&emsp; &emsp; 假设你怀疑其值最小是0.0001或最大是1, 那取值方式不应该是对整个区间随机均匀取值，而是先以0.0001，0.001，0.01，0.1，1为端点划分几个小区间，在这几个小区间上均匀随机取值。在Python中可以这样做，使r=-4*np.random.rand()，即$$r \in [4,0]$$，　然后$$a$$随机取值，$$ a =10^{r}$$，即$$ a \in [10^{-4},10^{0}] $$  

&emsp; &emsp; **学习率衰减**  
&emsp; &emsp; 加快学习算法的一个办法就是随时间慢慢减少学习率，我们将之称为学习率衰减。以下是常见的几种方法（其中,$$epoch\_num$$为代数，$$\alpha_{0}$$为初始学习率）:  
&emsp; &emsp; $$\alpha = \frac{1}{1 + decayrate * {epoch\_num} } \alpha_{0}$$  (decayrate称为衰减率)  
&emsp; &emsp; $$\alpha = {0.95}^{epoch\_num} \cdot \alpha_0$$  
&emsp; &emsp; $$\alpha =\frac{k}{\sqrt{epoch\_num}} \alpha_{0}$$  
&emsp; &emsp; 有时人们也会用一个离散下降的学习率，也就是某个步骤有某个学习率，一会之后，学习率减少了一半，一会儿减少一半，一会儿又一半，这就是离散下降（discrete stair cease）的意思。

<br>

### $$L$$（隐藏层数目）   
### $$n^{[l]}$$（隐藏层单元数目）    

<br>

### 激活函数（choice of activation function）   
&emsp; &emsp; 为什么神经网络需要非线性激活函数?因为这多个线性函数的组合本身也是线性函数，如果全部使用线性激活函数，那么无论神经网络有多少层一直在做的只是计算线性函数，还不如直接去掉全部隐藏层。常见激活函数有：  

**sigmoid函数:** $$ a = \sigma(z) = \frac{1}{ {1+e}^{-z} } $$  
<div style="margin-left:30px">  

优点：<br>    
<ul>
<li>输出为0到1之间的连续实值，此输出范围和概率范围一致，因此可以用概率的方式解释输出  
<li>将线性函数转变为非线性函数 
</ul> 

缺点：<br>
<ul>
<li>容易出现梯度消失 
<li>函数输出并不是zero-centered 
<li>幂运算相对来讲比较耗时  
</ul>
</div>

**tanh函数:**  $$ a= tanh(z) = \frac{e^{z} - e^{- z}}{e^{z} + e^{- z}}$$  
<div style="margin-left:30px">  

事实上，tanh函数是sigmoid的向下平移和伸缩后的结果, $$tanh(z) = 2 \cdot \sigma(2x) - 1 $$  <br>   
优点：<br>  
<ul>
<li>对比sigmoid和tanh两者导数输出可知，tanh函数的导数比sigmoid函数导数值更大，即梯度变化更快，也就是在训练过程中收敛速度更快。  
<li>输出范围为-1到1之间，这样可以使得输出均值为0，这个性质可以提高BP训练的效率，具体原因参考文献 http://yann.lecun.com/exdb/publis/pdf/lecun-98b.pdf  
<li>将线性函数转变为非线性函数  
</ul>
缺点：<br>  
<ul>
<li>容易出现梯度消失  
<li>幂运算相对来讲比较耗时  
</ul>
</div>

**ReLu函数:** $$ a =max(0,z) $$  
<div style="margin-left:30px">  

ReLU函数代表的的是“修正线性单元”。 <br>
当x <= 0时，激活值为0，对应的导数也为0； <br>  
当x > 0时，激活等于输入值，对应的导数为1。 <br> 
优点：<br>  
<ul>
<li>解决了梯度消失问题 (在正区间)  
<li>计算速度非常快，只需要判断输入是否大于0
<li>收敛速度远快于sigmoid和tanh
</ul>
缺点：<br>  
<ul>
<li>不是zero-centered
<li>某些神经元可能永远不会被激活
</ul>
</div>

**LRelu函数（Leaky Relu函数）：**  $$a = max(leaky*z,z)$$  其中 $$ leak$$ 是一个预习设定的很小的常数。     
<div style="margin-left:30px">  
Leaky ReLU是给所有负值赋予一个非零斜率，而不是像ReLU将所有的负值都设为零。 
<br>
LReLu变体:<br>   
<strong>PReLU函数：</strong>  
PReLU即“参数化修正线性单元”，负值部分的斜率是根据数据来定的，而非预先定义的。 <br>
<strong>RReLU函数：</strong> 
RReLU即“随机纠正线性单元”，负值部分的斜率在训练中是随机的，在之后的测试中就变成了固定的了。 <br> 
<strong>CReLu函数</strong>  
</div>

**ELU函数**  
**SELU函数**    

<br>

### iterations(梯度下降法循环的数量)    
### momentum  
### mini batch size  

<br>
<br>


### 正则化参数    
&emsp; &emsp; 深度学习可能存在过拟合问题——高方差，有两个解决方法，一个是准备更多的数据，另一个是正则化。  
**L1正则化**：$$\frac{\lambda}{2m}$$乘以$$w$$的L1范数。即：$$\frac{\lambda}{2m} \cdot \|w\|_1 $$，等于$$\frac{\lambda}{2m} \cdot \displaystyle\sum_{j=1}^{n_x}|w_j| $$  
&emsp; &emsp;如果用的是L1正则化，$$w$$最终会是稀疏的，也就是说$$w$$向量中有很多0。人们在训练网络时，越来越倾向于使用L2正则化。  
**L2正则化**：$$\frac{\lambda}{2m}$$乘以$$w$$的L2范数的平方。即：$$\frac{\lambda}{2m} \cdot {(\|w\|_2)}^2$$，等于$$\frac{\lambda}{2m} \cdot \displaystyle\sum_{j=1}^{n_x}(w_j)^2 $$，也等于为$$\frac{\lambda}{2m} \cdot w^{T}w$$。 

&emsp; &emsp;为什么只正则化参数$$w$$？忽略参数$$b$$ 呢？因为$$w$$通常是一个高维参数矢量，已经可以表达高偏差问题，$$w$$可能包含有很多参数，而$$b$$只是单个数字，所以$$w$$几乎涵盖所有参数，而没必要再加上$$b$$了。  

**Dropout正则化**：也叫随机失活正则化。  
&emsp; &emsp;它是这样运作的：在一次循环中我们先以概率p随机选择神经层中的一些单元并将其临时隐藏，然后再进行该次循环中神经网络的训练和优化过程。在下一次循环中，我们又以概率p将隐藏另外一些神经元，如此直至训练结束。  
&emsp; &emsp;在训练时由于舍弃了一些神经元,因此在测试时需要在激励的结果中乘上因子p进行缩放.但是这样需要需要对测试的代码进行更改并增加了测试时的计算量，非常影响测试性能。通常为了提高测试的性能(减少测试时的运算时间),可以将缩放的工作转移到训练阶段，而测试阶段与不使用dropout时相同,称为inverted dropout，也就是将前向传播dropout时保留下来的神经元的权重乘上1/p（看做惩罚项，使权重扩大为原来的1/p倍,这样测试时不用再缩小权重）。  
&emsp; &emsp;dropout在计算机视觉中应用得比较频繁。  
&emsp; &emsp;dropout缺点：每一次都会随机删除节点，也就是说每一次训练的网络都是不同的，损失函数不再被明确地定义，在某种程度上很难计算，我们失去了调试工具。




