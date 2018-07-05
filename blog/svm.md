### SVM的基本思想
____
SVM的基本思想非常直观。设想一个多维平面上散落着正样本和负样本，如果能找到一个超平面，恰好能将正负样本分开，那么这个超平面就可以用来对样本进行分类。如下图所示：
![](http://ww1.sinaimg.cn/large/006IYRZEly1fsxnqguhlcj30e80cbjrp.jpg)
我们的目标是找到图中的H3。
### 推导步骤
___
#### 步骤1
假设空间上的训练数据集： 
\[\begin{array}{l}
 T{\rm{ = \{ (}}{{\rm{x}}_1},{y_1}), \cdots ,({x_n},{y_n})\}  \\ 
 {y_i} = \{  + 1, - 1\}  \\ 
 \end{array}\]
找到分类超平面：\[{w^T}x + b = 0\]
样本空间中任意点x到超平面(w,b)的距离可写为：
\[r = \frac{{|{w^T}x + b|}}{{||w||}}\]
若该超平面能将正负样本分开，也就是正负样本完全被超平面隔离开，该情况从数学的角度看，就等同于:
\[{\mathop{\rm distance}\nolimits} ({x_i},{y_i}) = {y_i}\frac{{|{w^T}x + b|}}{{||w||}} > 0\]
对于所有的正样本，yi=1，distance(xi,yi)大于0意味着：
\[{w^T}x + b > 0\]
同理，对所有的负样本，yi=-1，也有：
\[{w^T}x + b < 0\]
这意味着负样本都在超平面与法向量ω反向的一侧。
假设同时存在两个平行于H的超平面H1和H2：
\[\begin{array}{l}
 {w^T}x + b = 1 \\ 
 {w^T}x + b =  - 1 \\ 
 \end{array}\]
使离H最近的正负样本刚好分别落在H1和H2上，这样的样本就是支持向量。那么其他所有的训练样本都将位于H1和H2之外，也就是满足如下约束：
\[\begin{array}{l}
 {w^T}x + b \ge 1{\rm{        }}\left\{ {{\rm{ }}{y_i} = 1} \right\} \\ 
 {w^T}x + b \le  - 1{\rm{     }}\left\{ {{\rm{ }}{y_i} =  - 1} \right\} \\ 
 \end{array}\]
写成统一的式子就是：
\[{y_i}\left( {{w^T}{x_i} + b} \right) - 1 \ge 0{\rm{   }}\]
而超平面H1和H2的距离可知为：
\[Margin = \frac{2}{{||w||}}\]
SVM的任务就是寻找这样一个超平面H把样本无误地分割成两部分，并且使H1和H2的距离最大。要找到这样的超平面，只需最大化间隔Margin，也就是最小化\[||w|{|^2}\]
于是，SVM的基本型如下：
\[\left\{ \begin{array}{l}
 \min \frac{{||w|{|^2}}}{2} \\ 
 s.t.{\rm{   }}{y_i}({w^T}x + b) \ge 1,{\rm{ }}i = 1,2, \cdots ,m \\ 
 \end{array} \right.\]
对于不等式约束的条件极值问题，可以用拉格朗日方法求解。而拉格朗日方程的构造规则是：用约束方程乘以非负的拉格朗日系数，然后再从目标函数中减去。于是得到拉格朗日方程如下：
\[L(w,b,{a_i}) = \frac{1}{2}||w|{|^2} - \sum\limits_{i = 1}^m {{a_i}(1 - {y_i}({w^T}{x_i} + b))} \]
其中：
\[{a_i} \ge 0\]
那么我们要处理的规划问题就变为：
\[\mathop {min}\limits_{w,b} \mathop {max}\limits_{{a_i} \ge 0} \left( {L(w,b,{a_i})} \right)\]
根据对偶问题做个等价变换：
\[\mathop {max}\limits_{{a_i} \ge 0} \mathop {min}\limits_{w,b} \left( {L(w,b,{a_i})} \right)\mathop {{\rm{ = }}min}\limits_{w,b} \mathop {max}\limits_{{a_i} \ge 0} \left( {L(w,b,{a_i})} \right)\]
其意义是：原凸规划问题可以转化为先对w和b求偏导，令其等于0消掉w和b，然后再对α求L的最大值。
先计算w和b的偏导数有：
\[\begin{array}{l}
 \frac{{\partial \left( {L(w,b,{a_i})} \right)}}{{\partial {\rm{w}}}} = w - \sum\limits_{i = 1}^m {{a_i}{y_i}{x_i}}  = 0 \\ 
 \frac{{\partial \left( {L(w,b,{a_i})} \right)}}{{\partial b}} =  - \sum\limits_{i = 1}^m {{a_i}{y_i}}  = 0 \\ 
 \end{array}\]
于是就有：
\[\begin{array}{l}
 w = \sum\limits_{i = 1}^m {{a_i}{y_i}{x_i}}  \\ 
 \sum\limits_{i = 1}^m {{a_i}{y_i}}  = 0 \\ 
 \end{array}\]
于是：
\[\begin{array}{l}
 \mathop {\min }\limits_{w,b} L(w,b,a) = \frac{1}{2}||w|{|^2} - w\sum\limits_{i = 1}^m {{a_i}{y_i}{x_i}}  - b\sum\limits_{i = 1}^m {{a_i}{y_i} + } \sum\limits_{i = 1}^m {{a_i}}  \\ 
  = \frac{1}{2}||w|{|^2} - {w^2} + \sum\limits_{i = 1}^m {{a_i}}  \\ 
  = \sum\limits_{i = 1}^m {{a_i}}  - \frac{1}{2}||w|{|^2} \\ 
  = \sum\limits_{i = 1}^m {{a_i}}  - \frac{1}{2}\sum\limits_{i = 1}^m {\sum\limits_{j = 1}^m {{a_i}{a_j}{y_i}{y_j}x_i^T{x_j}} }  \\ 
 \end{array}\]
于是可得对偶问题：
\[\left\{ \begin{array}{l}
 \mathop {\max }\limits_a \left\{ {\sum\limits_{i = 1}^m {{a_i}}  - \frac{1}{2}\sum\limits_{i = 1}^m {\sum\limits_{j = 1}^m {{a_i}{a_j}{y_i}{y_j}x_i^T{x_j}} } } \right\} \\ 
 s.t.{\rm{  }}\sum\limits_{i = 1}^m {{a_i}{y_i} = 0}  \\ 
 {a_i} \ge 0 \\ 
 \end{array} \right.\]
上式这个规划问题可以直接从数值方法计算求解。