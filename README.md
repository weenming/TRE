# 网球拍效应
冯睿骐[^1]
指导老师：白翠琴
## 网球拍效应介绍
TRE指刚体绕第二惯量主轴的不稳定旋转现象。刚体自由转动的动力学方程如下

$\begin{matrix}&{\dot{\mathrm{\Omega}}}_x=\frac{I_{yy}-I_{zz}}{I_{xx}}\mathrm{\Omega}_y\mathrm{\Omega}_z\\&{\dot{\mathrm{\Omega}}}_y=\frac{I_{zz}-I_{xx}}{I_{yy}}\mathrm{\Omega}_z\mathrm{\Omega}_x\\&{\dot{\mathrm{\Omega}}}_z=\frac{I_{xx}-I_{yy}}{I_{zz}}\mathrm{\Omega}_x\mathrm{\Omega}_y\\\end{matrix}$

在$I_{xx}<I_{zz}<I_{yy}$且$\Omega_{0z}\ll \omega_{0x}, \omega_{0y}$的条件下可以观察到刚体除了绕z轴旋转还会翻转。典型的发生、不发生TRE时，$\omega_z$关于时间的变化图像如下图所示：
![](./images/flip-stable_modes_omega.png)

## COMSOL设置
事实上，刚体的动力学方程是3个常微分方程构成的方程组。解ODE有很多数值方法，而且实现起来也不难。但总之我们先来使用COMSOL。本文档中使用的COMSOL版本是5.6。

#### 物理场设置
- 打开COMSOL，新建项目，选择 `Model Wizard`，接着选择`3D`，因为我们的模型是3维的。
- 只需选择`Multibody Dynamics`物理场，`Study`选择`Time Dependent`

#### 刚体设置
- 在`Component 1/Geometry`中添加你想要的几何体。我们将以T型扳手为例
  - 添加两个圆柱体并设置它们的位置和尺寸
- 选择`Component 1/Geometry`中的`Form Union`并在设置中选择`Build All`，从而将添加的几何体组装成一个几何体
  - 在右侧窗口可以看到它们的样子
  - <img src="./images/geometry.png" width=200>
- 选择材料(因为是刚体，事实上只有材料的密度会影响；又因为我们的研究中刚体自由转动，只要密度分布均匀就不会对运动有任何影响)

#### 模拟设置
我们只需要求解刚体运动，所以
- 在`Component 1/Multibody Dynamics`选项下添加`Rigid Domain`并将几何体加入这个刚体域。
  - 如图，右击`Multibody Dynamics`
   <img src="./images/rigidDomain.png" width=200>

  - 在右键菜单中选择添加Rigid Domain
   <img src="./images/rigidDomain2.png" width=200>
- 选择`Study 1/Step 1: Time Dependent`，设置希望使用的步长和模拟时间。这是一个ODE求解，所以不需要设置网格
  - 在左侧菜单中选择
   <img src="./images/solver1.png" width=200>

  - 在设置中调节
   <img src="./images/solver2.png" width=200>
- 在`Multibody Dynamics`中设置初始条件，需要设置第二主轴的初始角速度远大于另两个轴
  - 在Rigid Domain中选择Initial Values为Locally Defined
  <img src="./images/initVal.png" width=400>

  - 在下级菜单中设置初始条件
  <img src="./images/initVal2.png" width=200>
- 开始模拟并在`Results`中查看结果，可以导出数据并进一步分析

## 使用Phyphox进行实验
- 下载Phyphox App
- 选择角速度记录
- 扔手机，使得初始角速度基本沿第二主轴
- 接住手机 **(从而您的手机将会有更小的概率损坏)**
- 导出数据并分析

## 更多资料

- 数据处理的代码在[github](https://github.com/weenming/TRE_simulation)上可以找到
- 我的COMSOL实例在[github](https://github.com/weenming/TRE_simulation/tree/master/COMSOL)上可以找到
- 一个用COMSOL模拟TRE的[示例](https://www.comsol.com/blogs/why-do-tennis-rackets-tumble-the-dzhanibekov-effect-explained/)，我的COMSOL模拟参考了这个示例

[^1]: 由于作者水平有限，本文难免存在疏漏，请读者不吝赐教。邮箱：rfeng20@fudan.edu.cn