# 导入资源包

![](Asset/HauntedJaunt/2025-12-16-13-06-43.png)

![](Asset/HauntedJaunt/2025-12-16-13-07-41.png)

# 创建角色

- 拖到Hierarchy中，直接就在（0,0,0）的位置
  ![](Asset/HauntedJaunt/2025-12-16-13-12-34.png)
- 进入角色的预制件，添加Animator组件，创建AnimatorController，添加到Animator
- 添加Idle和Walk动画，设置切换条件，取消HasExitTime(了解一下HasExitTime的作用)
  ![](Asset/HauntedJaunt/2025-12-16-13-20-13.png)
- 在预制件中添加刚体，并设置Animator的UpdateMode为AnimatePhysics（对应FixedUpdate），否则角色直接升天
  ![](Asset/HauntedJaunt/2025-12-16-13-26-54.png)
- 添加碰撞体，设置碰撞体大小

# 移动脚本

- 创建脚本，命名为PlayerMovement.cs
- 创建全局变量
  ![](Asset/HauntedJaunt/2025-12-18-12-41-54.png)
- 刚体组件和动画管理者组件
  ![](Asset/HauntedJaunt/2025-12-18-12-44-36.png)
- 获取输入并设置移动
  注意，这里将m_Movement归一化，目的是仅获取它的方向，移动距离由动画来决定，防止移动速度与动画不匹配，造成视觉上的不良体验
  ![](Asset/HauntedJaunt/2025-12-18-12-39-55.png)
- 移动的动画
  ![](Asset/HauntedJaunt/2025-12-18-12-47-32.png)
- 实现移动
  ![](Asset/HauntedJaunt/2025-12-18-12-49-26.png)
- 转向
  在FixedUpdate()中获取旋转的角度，在动画移动事件中旋转
  ![](Asset/HauntedJaunt/2025-12-18-12-52-52.png)
  ![](Asset/HauntedJaunt/2025-12-18-12-57-44.png)

# 场景

- 将场景从预制体拖拽到Hierarchy中
  ![](Asset/HauntedJaunt/2025-12-18-13-00-31.png)
- 点击Gizmos可以吧辅助线隐藏
  ![](Asset/HauntedJaunt/2025-12-18-13-02-46.png)
- 设置光源的颜色，使地面的光为淡淡的蓝色月光
  ![](Asset/HauntedJaunt/2025-12-18-13-14-27.png)
- 设置光照的强度
  ![](Asset/HauntedJaunt/2025-12-18-13-15-50.png)
- 设置光照角度，阴影类型、质量
  ![](Asset/HauntedJaunt/2025-12-18-13-18-01.png)
- 设置项目光源
  新建一个环境光，把它放在Assets目录下即可
  ![](Asset/HauntedJaunt/2025-12-18-13-20-13.png)
  取消勾选
  ![](Asset/HauntedJaunt/2025-12-18-13-20-46.png)
  设置环境光
  ![](Asset/HauntedJaunt/2025-12-18-13-22-09.png)
  ![](Asset/HauntedJaunt/2025-12-18-13-22-35.png)
  ![](Asset/HauntedJaunt/2025-12-18-13-22-59.png)
  ![](Asset/HauntedJaunt/2025-12-18-13-22-49.png)
  ![](Asset/HauntedJaunt/2025-12-18-13-23-08.png)
  ![](Asset/HauntedJaunt/2025-12-18-13-23-23.png)
  保存

# 光照

## 实时光照和烘焙光照

- 实时光照: 指 Unity 在运行时计算光照
- 烘焙光照：指 Unity 提前执行光照计算并将结果保存为光照数据，然后在运行时应用。
- 混合光照：实时光照、烘焙光照两者的混合.

## 项目光源设置

  ![](Asset/HauntedJaunt/2025-12-18-13-10-50.png)

# 导航网格

## 组成

![](Asset/HauntedJaunt/2025-12-18-13-26-26.png)

- 导航网格(即 Navigation Mesh，缩写为 NavMesh)
  是一种数据结构，用于描述游戏世界的可行走表面，并允许在游戏世界中寻找从一个可行走位置到另一个可行走位置的路径。该数据结构是从关卡几何体自动构建或烘焙的。
- 导航网格代理(NavMesh Agent) 组件
  可帮助您创建在朝目标移动时能够彼此避开的角色。代理使用导航网格来推断游戏世界，并知道如何避开彼此以及移动的障碍物。
- 网格链接(Off-Mesh Link) 组件
  允许您合并无法使用可行走表面来表示的导航捷径。例如，跳过沟渠或围栏，或在通过门之前打开门，全都可以描述为网格外链接。
- 导航网格障碍物(NavMesh Obstacle)组件
  可用干描述代理在世界中导航时应避开的障碍物。由物理系统控制的木桶或板条箱便是障碍物的典型例子。障碍物正在移动时代理将尽力避开它，但是障碍物一旦变为静止状态，便会在导航网格中雕刻一个孔，从而使代理能够改变自己的路径来绕过它或者如果静止的障碍物阻挡了路径，则代理可寻找其他不同的路线。

## 设置导航网格

- 导航网格应该是static的，所以将Hierarchy中的Level设置的Static，包括其子对象，但天花板不应该设置导航网格，所以将天花板的static取消
  ![img](Asset/HauntedJaunt/2025-12-18-13-32-04.png)
- 打开Navigation
  ![](Asset/HauntedJaunt/2025-12-18-13-32-54.png)
- 修改半径（为什么？），烘焙
  ![](Asset/HauntedJaunt/2025-12-18-13-34-40.png)

# 镜头跟随

## Cinemachine虚拟摄像机简介
- 在场景中创建一个或多个“虚拟"摄像机
- 使用 Follow 属性指定要跟随的 GameObject;
- 使用 Look At 属性指定虚拟相机应该瞄准的游戏对象。
- 这些虚拟摄像机由一个名为 Cinemachine Brain 的组件进行管理。
- Cinemachine Brain 与 Camera 组件连接到相同的游戏对象，默认情况下，这个游戏对象将是 Main Camera 游戏对象。Cinemachine Brain 管理所有虚拟摄像机，并确定实际摄像机应跟随哪个虚拟摄像机(或虚拟摄像机的组合)

## 使用Cinemachine
- 创建虚拟摄像机，如果没有这个选项的话，就在Window中安装
![](Asset/HauntedJaunt/2025-12-18-23-23-33.png)

- 此时在MainCamera中会出现Cinemachine Brain组件
![](Asset/HauntedJaunt/2025-12-18-23-37-18.png)

- 添加角色到虚拟摄像机的Follow
![](Asset/HauntedJaunt/2025-12-18-23-26-11.png)

- 设置与玩家的距离
- Aim表示瞄准，可以用来旋转，先选择Do Nothing
![](Asset/HauntedJaunt/2025-12-18-23-30-29.png)

- 设置MainCamera的旋转角度
![](Asset/HauntedJaunt/2025-12-18-23-29-28.png)

# 后期特效
## 抗锯齿
- 添加Layer
![](Asset/HauntedJaunt/2025-12-18-23-44-18.png)

- 在MainCamera中添加组件，没有的话可以在packageManage中下载
  设置层为上一步添加的层，选择快速，这个模式性能较高，效果一般
![](Asset/HauntedJaunt/2025-12-18-23-48-07.png)

## 颜色分级
- 在Hierarchy中新建一个空对象，将其Layer设置为后期处理层，添加Post-process Volume组件
![](Asset/HauntedJaunt/2025-12-19-13-04-19.png)

- new 一个 profile,他会保存在Assets中Scene的目录下
![](Asset/HauntedJaunt/2025-12-19-12-54-10.png)
![](Asset/HauntedJaunt/2025-12-19-12-54-47.png)

- 添加 **颜色分级** 效果
![](Asset/HauntedJaunt/2025-12-19-12-56-40.png)

- 打开 **色调映射**，对比一下开和不开的区别，如果不生效，勾选上Is Global，
![](Asset/HauntedJaunt/2025-12-19-12-57-34.png)

- **曝光** 设为1
![](Asset/HauntedJaunt/2025-12-19-12-58-45.png)

- 色调
  Lift 会影响阴影的颜色
  Gain 会更改更亮的高光
  Gamma 涵盖图像颜色中间（或中间范围）的所有内容
  将Lift 和 Gamma 略微拖向蓝色，Gain略微拖向黄色，增加阴影的深度并增加光照的温暖度
  ![](Asset/HauntedJaunt/2025-12-19-13-10-49.png)

### 泛光效果
- 添加Bloom
![](Asset/HauntedJaunt/2025-12-19-13-05-36.png)

- 点All全部打开
![](Asset/HauntedJaunt/2025-12-19-13-06-37.png)

- 强度设为2.5，就能看到泛光效果了
  Intensity 强度
  Threshold 阈值，表示光强大于这个值的光才能使用泛光效果
![](Asset/HauntedJaunt/2025-12-19-13-11-51.png)
  能够看到这个位置泛光效果比较明显
![](Asset/HauntedJaunt/2025-12-19-13-12-13.png)

### 环境光遮挡
- 添加效果，打开后阴影会更加明显
![](Asset/HauntedJaunt/2025-12-19-13-14-11.png)

### 渐晕特效
- 使屏幕周围出现阴影
  Intensity 决定阴影效果在屏幕上散步的距离
  Smoothness 决定这种效果朝着屏幕中心淡入淡出的距离
![](Asset/HauntedJaunt/2025-12-19-13-16-11.png)
![](Asset/HauntedJaunt/2025-12-19-13-16-22.png)

### 镜头失真特效
- 左右滑动Intensity会有一张凸透镜凹透镜的效果
![](Asset/HauntedJaunt/2025-12-19-13-19-37.png)

# 游戏结束UI
## 创建UI
- 在Hierarchy中创建UI->Image，它会自动创建一个Canvas，并在Canvas下创建Image，以及一个EventSystem，用于触发UI显示，暂时用不到，删掉，重命名Canvas
![](Asset/HauntedJaunt/2025-12-19-21-45-10.png)

- 缩放组件，用不上，为了提高游戏性能，删掉
![](Asset/HauntedJaunt/2025-12-19-21-43-42.png)

- 交互相关组件，删掉
![](Asset/HauntedJaunt/2025-12-19-21-44-31.png)

- 切到2D模式，把特效关掉
![](Asset/HauntedJaunt/2025-12-19-21-46-35.png)

- 设置锚点为0-1，strength中的Top表示到上面那条边的距离，设为0，其他三个同理，使Image填充Canvas
![](Asset/HauntedJaunt/2025-12-19-21-47-56.png)
![](Asset/HauntedJaunt/2025-12-19-21-49-19.png)

- 背景设为黑色
![](Asset/HauntedJaunt/2025-12-19-21-51-02.png)

- 以上是背景，在背景下再添加一个Image，选择图像，设置锚点和拉伸，使其填充，为保持长宽比例，勾选PreServe Aspect(保留)
![](Asset/HauntedJaunt/2025-12-19-21-53-45.png)

- 选择背景，添加Canvas Group组件，设置其透明度为0，在游戏结束时再通过代码设置让其显示
![](Asset/HauntedJaunt/2025-12-19-21-57-10.png)

## 游戏结束逻辑
- 新建一个EmptyObject，增加碰撞体，勾选IsTrigger，放到出口的位置
![](Asset/HauntedJaunt/2025-12-19-22-00-28.png)

- 新建一个脚本 GameEnding.cs
![](Asset/HauntedJaunt/2025-12-19-22-05-16.png)
![](Asset/HauntedJaunt/2025-12-19-22-12-30.png)
![](Asset/HauntedJaunt/2025-12-19-22-16-36.png)

- 结束游戏
![](Asset/HauntedJaunt/2025-12-19-22-15-28.png)

# 敌人
## 石像鬼
### 预制体
- 拖拽预制件到Hierarchy
![](Asset/HauntedJaunt/2025-12-19-22-20-13.png)

- 把它拖回自己的预制件文件夹，生成预制件，并进入预制件编辑模式
![](Asset/HauntedJaunt/2025-12-19-22-21-42.png)

- 创建石像鬼的动画控制器
![](Asset/HauntedJaunt/2025-12-19-22-23-05.png)

- 将石像鬼的Idle动画拖到动画控制器中，石像鬼是不会移动的，仅需要这个动画即可
![](Asset/HauntedJaunt/2025-12-19-22-24-12.png)

- 添加到石像鬼的动画师中
![](Asset/HauntedJaunt/2025-12-19-22-25-45.png)

- 添加一个碰撞体，包裹石像鬼的身体，但不设置触发
![](Asset/HauntedJaunt/2025-12-19-22-28-05.png)

- 在石像鬼下创建一个空对象，调整位置和旋转角度，模拟它的视角原点
![](Asset/HauntedJaunt/2025-12-19-22-31-11.png)

- 确保是Local状态
![](Asset/HauntedJaunt/2025-12-19-22-33-12.png)

- 给视角添加一个碰撞体，设置为触发器，设置尺寸，并选择拉伸方向为Z轴
![](Asset/HauntedJaunt/2025-12-19-22-35-18.png)

### 脚本
- 创建脚本，添加到石像鬼预制件的视线对象下
![](Asset/HauntedJaunt/2025-12-19-22-43-52.png)

- Update()
![](Asset/HauntedJaunt/2025-12-19-22-45-54.png)

- 在GameEnding中，记得将Image传进来
![](Asset/HauntedJaunt/2025-12-19-23-00-21.png)
![](Asset/HauntedJaunt/2025-12-19-22-47-15.png)
![](Asset/HauntedJaunt/2025-12-19-22-47-56.png)
![](Asset/HauntedJaunt/2025-12-19-22-48-21.png)
![](Asset/HauntedJaunt/2025-12-19-22-50-34.png)

## 被抓住的UI
- 复制结束的部分来修改
![](Asset/HauntedJaunt/2025-12-19-22-39-32.png)
![](Asset/HauntedJaunt/2025-12-19-22-40-13.png)

- 另一种方法
![](Asset/HauntedJaunt/2025-12-19-22-53-43.png)
![](Asset/HauntedJaunt/2025-12-19-22-54-48.png)

- 要使用Resources.Load，必须创建一个Resources文件夹，并把资源放进去
![](Asset/HauntedJaunt/2025-12-19-22-56-06.png)

- Update 修改传入的参数
![](Asset/HauntedJaunt/2025-12-19-22-57-45.png)

![](Asset/HauntedJaunt/2025-12-19-22-58-47.png)