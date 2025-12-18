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
![](Asset/HauntedJaunt/2025-12-18-13-32-04.png)

- 打开Navigation
![](Asset/HauntedJaunt/2025-12-18-13-32-54.png)

- 修改半径（为什么？），烘焙
![](Asset/HauntedJaunt/2025-12-18-13-34-40.png)