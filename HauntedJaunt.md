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