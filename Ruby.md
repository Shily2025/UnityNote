
# Tilemap 瓦片地图
## 创建瓦片地图
### 单格瓦片
1. 在Hierarchy中添加一个 Tilemap

![](Asset/Ruby/2025-11-21-20-08-31.png)

2. 打开Tile Palette 窗口
   
![](Asset/Ruby/2025-11-21-20-13-49.png)

3. 创建Tile Palette
   
![](Asset/Ruby/2025-11-21-20-14-58.png)

4. 保存到Assets/Tiles文件夹
   
5. 在 Assets/Tiles 文件夹中创建 Tile, 并更名为BrickTile
   
![](Asset/Ruby/2025-11-21-20-18-16.png)

6. 添加默认的精灵
   
![](Asset/Ruby/2025-11-21-20-20-38.png)

7. 拖拽到Tile Palette窗口
   
![](Asset/Ruby/2025-11-22-23-04-18.png)   

8. 在Scene中绘制
   
![](Asset/Ruby/2025-11-22-23-05-54.png)

9. 填充不满方格时，是因为spirit中设置的每个单元填充的像素为100，但是每个Spirit的图片资源像素是64，所以要修改一下
   
![](Asset/Ruby/2025-11-22-23-08-05.png)

10. 设置瓦片地图的Layer为-10，防止将玩家覆盖
    
![](Asset/Ruby/2025-11-22-23-11-19.png)

### 多格瓦片
1. 切割素材，点击slice，随后会生成9个sprite
   
![](Asset/Ruby/2025-11-23-16-37-09.png)

2. 再把它拖进tilepallete
   
![](Asset/Ruby/2025-11-23-16-41-48.png)

3. 这样就可以想要哪个部分来画图就选哪个部分了
   
![](Asset/Ruby/2025-11-23-16-44-25.png)

### 带规则的多格瓦片
1. 新建一个RuleTile
   
![](Asset/Ruby/2025-11-23-16-49-13.png)

2. 把多格瓦片生成的spirit添加进来
   
![](Asset/Ruby/2025-11-23-16-53-10.png)

3. 设置每个格子的规则
   
![](Asset/Ruby/2025-11-23-16-54-55.png)

4. 将RuleTile拖到TilePalette调色板
   
![](Asset/Ruby/2025-11-23-16-56-50.png)
 
### Rule Override Tile 规则覆盖瓦片
 可以基于旧的规则生成新的Rule Tile
1. 创建新的RuleTile，将已经做好的多格瓦片拖过来
   
![](Asset/Ruby/2025-11-23-20-59-46.png)

2. 把新的Spirit拖过来，规则将与旧的规则一样，不过换了皮肤
   
![](Asset/Ruby/2025-11-23-21-02-20.png)

3. 拖到调色板
   
![](Asset/Ruby/2025-11-23-21-04-18.png)

## Advance Rule Override Tile 高级规则覆盖瓦片插件
功能与Rule Override Tile 一样，能再次编辑规则

## 图层排序方式
当玩家和精灵位于同一图层时，需要区分谁在前面谁在后面，比如玩家和树，当玩家的Y轴大于树时，树应当在玩家前面，遮住玩家，捅过Project Settings中的Graphics来设置，由Y轴来决定排序方式

![](Asset/Ruby/2025-11-23-23-02-35.png)
但有时候精灵的轴心有可能在中心点位置，我们需要将它设置到最底下，才能达到想要的效果，按住Ctrl可以吸附

![](Asset/Ruby/2025-11-23-23-06-58.png)

# 碰撞
## 玩家和障碍物的碰撞
为了方便后期操作，先把Ruby拖到预制体中

![](Asset/Ruby/2025-11-23-23-21-45.png)

添加刚体组件，重力设为0，否则会往下掉

![](Asset/Ruby/2025-11-23-23-23-39.png)

添加2D盒状碰撞体，设置碰撞体的范围大概在下半身的位置

![](Asset/Ruby/2025-11-23-23-30-02.png)
设置Ruby的刚体冻结Z轴旋转，否则发生碰撞时，Ruby可能会绕Z轴旋转，形成被推到的效果

![](Asset/Ruby/2025-11-23-23-32-58.png)

将盒子、树等精灵也拖到预制体中，并给它加上2D盒状碰撞体，两个物体发生碰撞且阻碍运动时，只需要其中一个有刚体组件即可，多了没用，反而给CPU负担

![](Asset/Ruby/2025-11-23-23-31-47.png)

直接控制玩家的transform时，碰撞到障碍物后会出现玩家抖动的现象，实际上是先执行玩家的移动，让他进入到障碍物内部，然后触发碰撞，被障碍物推出
要解决这个问题，可以在Start中获取到刚体组件，在fixedUpdata中改变刚体的transform

![](Asset/Ruby/2025-11-23-23-43-09.png)

## 瓦片地图的碰撞
给所有的瓦片地图添加碰撞体, 添加成功后场景中的瓦片地图出现绿色方格

![](Asset/Ruby/2025-11-23-23-59-58.png)

将不是障碍的Tile的碰撞体设置为None

![](Asset/Ruby/2025-11-23-23-57-57.png)

障碍的部分，如水面，设置为Sprite

![](Asset/Ruby/2025-11-24-00-02-41.png)

如果使用的是Rule Tiles,那就打开规则，设置相应的碰撞体

![](Asset/Ruby/2025-11-24-00-04-22.png)

![](Asset/Ruby/2025-11-24-00-06-53.png)
注意，此时就要用到Advance Override Rule Tile了，使用Override Rule Tile的话，会直接继承Rule Tile的规则，包括碰撞体，所以不方便去修改

由于每个瓦片地图都有自己的碰撞体，当场景很大时，会导致运算量非常大，添加组合碰撞体组件，可以使相邻且同属性的碰撞体合并

![](Asset/Ruby/2025-11-24-00-13-30.png)

另外，将其刚体组件设置成Static，也能提高游戏性能

# 添加房子和树
在Sprite的资源中设置房子和树的轴心，房子的放在门的位置，注意：**要先设置轴心，否则设置碰撞体的时候，碰撞体的位置是基于轴心的，修改轴心位置会导致碰撞体移动**

![](Asset/Ruby/2025-11-24-12-50-52.png)
Sprite Sort Mode 要修改为Pivot否则层级排序不生效

![](Asset/Ruby/2025-11-24-12-52-57.png)
把包中的房子和树等资源拖到Hierarchy窗口中，并把他们拖到预制体文件夹

![](Asset/Ruby/2025-11-24-12-44-46.png)
给预制体添加2D碰撞体组件，双击预制体，编辑碰撞体大小

![](Asset/Ruby/2025-11-24-12-47-44.png)

# 简单的生命系统
- 在Start()中给currentHealth赋值maxHealth

![](Asset/Ruby/2025-11-24-12-57-40.png)

![](Asset/Ruby/2025-11-24-12-59-51.png)

- 回血道具
从包中将回血道具添加到Hierarchy窗口，调整尺寸，随后将其拖到预制体窗口

![](Asset/Ruby/2025-11-24-13-03-57.png)

编辑预制体，添加2D碰撞体组件，设置IsTriggr为true，编辑碰撞体范围

![](Asset/Ruby/2025-11-24-13-06-31.png)

给草莓挂上脚本

![](Asset/Ruby/2025-11-24-13-16-14.png)

![](Asset/Ruby/2025-11-24-13-17-00.png)

- 创建减血尖刺预制体，然后添加碰撞体，设置IsTrigger为True

![](Asset/Ruby/2025-11-24-13-19-49.png)

挂载掉血脚本

![](Asset/Ruby/2025-11-24-13-31-26.png)

设置Ruby的刚体的SleepMode为NeverSleep

![](Asset/Ruby/2025-11-24-13-25-26.png)

![](Asset/Ruby/2025-11-24-13-24-55.png)
- 
设置玩家的无敌时间
为了防止玩家踩在尖刺上一直掉血，要设置玩家调一次血无敌一段时间

![](Asset/Ruby/2025-11-24-13-28-05.png)

![](Asset/Ruby/2025-11-24-13-28-52.png)

![](Asset/Ruby/2025-11-24-13-29-44.png)

- 平铺尖刺
将资源里面的尖刺MeshType修改为FullRect

![](Asset/Ruby/2025-11-24-13-34-21.png)

把预制体的缩放设为1，设置DrawMode为Tiled

![](Asset/Ruby/2025-11-24-13-35-46.png)

通过举行工具或者缩放工具来调整范围和尖刺大小，最后再调整碰撞体范围

![](Asset/Ruby/2025-11-24-13-36-47.png)

# 敌人
## Spirit
- 下载资源，拖拽到Asset/Spirit目录下，重命名，**png拖拽到Unity中会自动转换成Spirit，有的不会自动变就右边的那个TextureType那里换就可以了，然后要先保存设置才可以使用**
  
![](Asset/Ruby/2025-11-25-23-17-09.png)
  
![](Asset/Ruby/2025-11-25-23-18-44.png)

- 设置Robot在每个单元的像素点
  
![](Asset/Ruby/2025-11-25-23-22-15.png)

- 设置轴心点位置
  
![](Asset/Ruby/2025-11-25-23-24-43.png)

- 拖拽到hierarchy窗口，并设置排序方式
  
![](Asset/Ruby/2025-11-25-23-28-33.png)

- 因为机器人也需要移动和碰撞，所以要添加刚体，为防止碰撞时旋转，锁定Z轴
  
![](Asset/Ruby/2025-11-25-23-32-41.png)  

- 添加2D碰撞体组件,碰撞体范围应该设置在脚的位置
  
![](Asset/Ruby/2025-11-25-23-34-18.png)

- 拖拽到Prefabs中

## 脚本
- 创建EnemyController脚本，这个脚本可以试着自己写，不必和教程一样
  
![](Asset/Ruby/2025-11-25-23-38-36.png)
  
![](Asset/Ruby/2025-11-25-23-43-41.png)
  
![](Asset/Ruby/2025-11-25-23-45-16.png)

- **也可以用Rigidbody2D.MovePosition(targetPosition) 来实现移动**
  
![](Asset/Ruby/2025-11-25-23-51-34.png)

- 伤害

  **为了保持玩家和敌人接触时依然有互相推挤的效果，不能设置敌人的碰撞体的IsTrigger属性为true，否则玩家会穿过敌人，OnTriggerEnter2D是碰撞体IsTrigger为True时触发的，此时不能使用OnTriggerEnter2D来处理玩家和敌人的碰撞，可以使用刚体碰撞的事件OnCollisionEnter2D来处理，要注意的是，与OnTriggerEnter2D不同不能直接从传入的Collision中GetComponent，因为它不是一个GameObject，要先获取其GameObject，再获取Component**
  
![](Asset/Ruby/2025-11-26-00-00-37.png)

# 动画

## 敌人

- 进入Robot的预制体，添加Animator组件，在Assets/Animations中创建Animation Controller，命名为Robot，给Animator添加这个Animation Controller

![](Asset/Ruby/2025-11-26-12-42-25.png)

![](Asset/Ruby/2025-11-26-12-43-18.png)

- 创建Animation

![](Asset/Ruby/2025-11-26-12-46-27.png)

- 点击Robot预制体，创建向左走的动画，保存为RobotLeft
  
![](Asset/Ruby/2025-11-26-12-49-00.png)

- 在资源中找到Robot的动画素材，拖拽到动画中，如果无法拖拽，先选中Robot的预制件

![](Asset/Ruby/2025-11-26-12-51-59.png)

- 设置动画帧率，Samples表示1秒钟刷60帧，把它改成4，否则会很鬼畜，可以点击播放预览
  
![](Asset/Ruby/2025-11-26-12-53-38.png)

- 创建向右走的动画RobotRight

![](Asset/Ruby/2025-11-26-12-55-47.png)

- 同样拖拽向左走的素材过来，设置X轴的翻转就能得到向右走的动画

![](Asset/Ruby/2025-11-26-12-57-40.png)

![](Asset/Ruby/2025-11-26-12-58-04.png)

- 添加向上走和向下走的动画，选择对应的素材

- 打开动画师，可以直接打开Assets/Animations中的Robot，也可以选中Robot预制体，再在window中打开
  
![](Asset/Ruby/2025-11-26-13-02-07.png)

- 删除掉所有动画，右键空白处创建动画混合树，双击进入BlendTree
  
![](Asset/Ruby/2025-11-26-13-06-40.png)

- 创建两个float参数，并在检查器中调用这两个参数，添加动画序列
  
![](Asset/Ruby/![](Asset/Ruby/2025-11-26-13-12-11.png).png)

- 在EnemyController中获取Animator组件

![](Asset/Ruby/2025-11-26-13-15-10.png)

- 在FixedUpdate()中设置animator的参数

![](Asset/Ruby/2025-11-26-13-17-33.png)

- 修改BlendTree的名字为Move

- 添加修复的动画

![](Asset/Ruby/2025-12-09-13-07-11.png)

- 触发进入修复

![](Asset/Ruby/2025-12-09-13-09-12.png)

## 玩家
- 动画资源

![](Asset/Ruby/2025-11-26-13-19-45.png)

- Ruby的Animator
  <!-- todo: -->
**  使用Trigger而不是Bool，目的是当设置Trigger为true时切换动画，在BlenderTree中设置HasExitTime，在动画播放结束后自动切换回Idle状态，无需等待Trigger变成false？这里要实验确认**

![](Asset/Ruby/2025-11-27-13-39-07.png)

- 声明一个Animator对象，并在Start()中获取

![](Asset/Ruby/2025-11-27-12-51-30.png)

- 受伤的动画

![](Asset/Ruby/2025-11-27-12-53-24.png)

- 在Updata()中切换移动和静止的动画
 **Mathf.Approximately() 表示比较两个浮点值是否近似相等，浮点值不能通过 == 比较**

![](Asset/Ruby/2025-11-27-12-57-35.png)

## 创建玩家动画
- Ctrl+6 或者在Window/Animation创建动画剪辑的时候并保存的时候，由于选中的是Ruby，如果在保存路径下没有Ruby的AnimatorController，则会自动创建一个，并将它挂到Ruby的Animator中

![](Asset/Ruby/2025-11-27-13-06-27.png)

- 要创建Ruby的这些动画

![](Asset/Ruby/2025-11-27-13-07-07.png)

- 为了使动画更加流畅，选择60的帧率，但要将每一个动画剪辑的拖拽到指定的帧上，不然一下子就播放完了

![](Asset/Ruby/2025-11-27-13-11-22.png)

- 做hit动画的时候，其实就是让Sprite在显示与不显示之间来回切换，勾选Enable为显示，第5帧的时候取消Enable，第10帧的时候再勾选Enable，实际调整看看效果

![](Asset/Ruby/2025-11-27-13-19-07.png)

- launch动画其实就是在第0和第30帧放同样的动作

![](Asset/Ruby/2025-11-27-13-23-11.png)

- Animator 中 Idle 

![](Asset/Ruby/2025-11-27-13-26-02.png)

- Animator 中 Moving 

![](Asset/Ruby/2025-11-27-13-27-24.png)

- Hit和Launch参考以上

- Hit 可以选择HasExitTime，当播放结束时自动退出回到Idle，就不需要设置回到Idle的条件了
![](Asset/Ruby/2025-11-27-13-36-57.png)

- 其他BlendTree的切换参数参考

![](Asset/Ruby/2025-11-27-13-33-45.png)

# 飞弹
## 编辑预制体
- 先拖拽一个齿轮到Hierarchy中，设置它的大小
  
![](Asset/Ruby/2025-12-05-22-50-09.png)

- 将设置好的齿轮拖到预制体文件夹中，并改名，删除Hierarchy中的飞弹
  
![](Asset/Ruby/2025-12-05-22-54-43.png)

- 进入预制体编辑界面，添加刚体和碰撞体，设置重力为0
  
![](Asset/Ruby/2025-12-05-22-56-21.png)

## 脚本
- 创建脚本，并添加给飞弹的预制体
  
![](Asset/Ruby/2025-12-05-22-57-23.png)

- 编辑子弹脚本
  
![](Asset/Ruby/2025-12-05-23-12-42.png)

- 在RubyController中增加一个public的子弹预制体

![](Asset/Ruby/2025-12-05-23-17-22.png)

- 给RubyController设置子弹的预制体

![](Asset/Ruby/2025-12-05-23-16-17.png)

- 玩家发射子弹函数

![](Asset/Ruby/2025-12-05-23-34-54.png)
![](Asset/Ruby/2025-12-05-23-21-46.png)

- 在update()函数中，C键被按下时发射子弹

![](Asset/Ruby/2025-12-05-23-23-37.png)

## 处理异常
- NULL 引用异常

![](Asset/Ruby/2025-12-05-23-27-46.png)

- 防止子弹与玩家碰撞
  创建两个layer，将Ruby的图层设置为Character，飞弹预制体的图层设置为Projectile，取消这两个图层的碰撞
  ![](Asset/Ruby/2025-12-05-23-30-30.png)
  ![](Asset/Ruby/2025-12-05-23-32-46.png)

## 修改射击按键获取方式
- 设置fire1为左键
  
![](Asset/Ruby/2025-12-05-23-37-53.png)

- 修改脚本

![](Asset/Ruby/2025-12-05-23-39-23.png)

## 修复机器人
- 给EnemyController添加一个变量(改为broken)，标记它是坏的

![](Asset/Ruby/2025-12-05-23-45-08.png)

- 机器人被修复之后不再移动

![](Asset/Ruby/2025-12-05-23-51-01.png)
![](Asset/Ruby/2025-12-05-23-51-31.png)

- EnemyController中创建修复机器人的方法

![](Asset/Ruby/2025-12-09-13-11-03.png)

- 在飞弹的脚本中，碰撞触发时获取机器人并修复

![](Asset/Ruby/2025-12-05-23-56-22.png)

## 子弹自动销毁
- 在子弹的脚本中

![](Asset/Ruby/2025-12-05-23-59-21.png)

# 镜头控制
## Cinemachine
- 安装插件
  
![](Asset/Ruby/2025-12-09-12-39-58.png)

 - 添加Cinemachine，摄像机支持以下两种模式
  透视模式 perspective：有近大远小的3D效果
  正交模式 orthographic：远近都一样大的2D效果
  
![](Asset/Ruby/2025-12-09-12-42-02.png)

- 将Rybu拖拽到Cinemachine的跟随目标

![](Asset/Ruby/2025-12-09-12-50-16.png)

- 设置摄像机的边界，给Cinemachine添加扩展组件，2D碰撞体

![](Asset/Ruby/2025-12-09-12-53-39.png)

- 添加一个EmptyObject，给它添加多边形碰撞体，并将碰撞体的边界设定在地图边界
  注意：必须是多变体碰撞体，否则对Cinemachine无效

  ![](Asset/Ruby/2025-12-09-12-59-02.png)

  ![](Asset/Ruby/2025-12-09-12-57-55.png)

  - 将空气墙拖到Cinemachine的边界属性上
  
  ![](Asset/Ruby/2025-12-09-13-00-45.png)

  - 修改空气墙碰撞体的layer，并设置它和任何层都不碰撞防止把玩家挤出去

![](Asset/Ruby/2025-12-09-13-02-34.png)

![](Asset/Ruby/2025-12-09-13-04-32.png)

# 粒子系统
## 准备素材
- 分割素材

![](Asset/Ruby/2025-12-09-13-18-51.png)

![](Asset/Ruby/2025-12-09-13-19-39.png)

## 添加粒子
- 新建粒子

![](Asset/Ruby/2025-12-09-13-21-19.png)

![](Asset/Ruby/2025-12-09-13-20-53.png)

- 替换素材

![](Asset/Ruby/2025-12-09-13-23-26.png)

- 随机生成两种烟雾，并在飘的过程中随机转换
设置StartFrame为0-2的随机数，表示从第0和1个烟雾中（不包含2）随机生成
FrameOverTime 设置为0时，飘的过程中将不会转换
![](Asset/Ruby/2025-12-10-23-24-09.png)

- 设置生成半径为0，使烟雾更集中

![](Asset/Ruby/2025-12-10-23-28-26.png)

- 设置速度、生命周期、尺寸为随机数

![](Asset/Ruby/2025-12-10-23-31-16.png)

- 设置淡出效果，Alpha起始设为0（不透明），结束设为255（全透明）

![](Asset/Ruby/2025-12-10-23-33-26.png)

- 设置size over lifttime，使其出现的时候最大，然后慢慢变小

![](Asset/Ruby/2025-12-10-23-36-25.png)

- 设置烟雾到机器人头上，将烟雾粒子拖拽到prefabs文件夹中，然后打开机器人的预制体设置，将烟雾粒子预制体拖拽到机器人的预制体中，并拖动烟雾修改其位置，使烟雾在机器人头顶生成

![](Asset/Ruby/2025-12-10-23-39-06.png)

- 烟雾实现拖尾效果，设置烟雾预制体

![](Asset/Ruby/2025-12-10-23-40-35.png)

## 脚本控制烟雾
- 在机器人的控制脚本中创建一个烟雾粒子特效对象

![](Asset/Ruby/2025-12-10-23-44-18.png)

- 将Hierarchy中的机器人底下的烟雾拖拽到该对象中

![](Asset/Ruby/2025-12-10-23-45-48.png)

- 销毁对象

![](Asset/Ruby/2025-12-10-23-46-41.png)

- 直接销毁对象，会导致烟雾直接消失，包括已生成正在飘的，会有点突然，使用stop()可以让特效停止生成，已生成的会走完生命周期再消失

![](Asset/Ruby/2025-12-10-23-49-34.png)

## 子弹击中特效
- 在子弹的脚本中创建一个碰撞特效的对象

![](Asset/Ruby/2025-12-11-13-01-36.png)

- 在碰撞事件中实例化特效
 **注意，在特效的Duration中，设置特效结束时Destroy**

![](Asset/Ruby/2025-12-11-13-03-22.png)

- 吃血包的特效同样挂在血包中

# UI
## UI系统
![](Asset/Ruby/2025-12-11-13-08-32.png)

## 头像血条
- 添加Canvas(画布)

![](Asset/Ruby/2025-12-11-13-15-20.png)

- 修改名字，然后右键Canvas->UI->Image，在画布中添加图像，修改Image名为HealthFrame
  
- 从资源中将血条拖到Image中，点击SetNativeSize，还原其尺寸，然后按住Shift，调节血条大小，并将其拖到UI框（即屏幕范围）左上角

![](Asset/Ruby/2025-12-11-13-20-56.png)

- 为防止屏幕分辨率变化导致血条位置变化，要设置血条的锚点在左上角，点击HealthFrame进行设置

![](Asset/Ruby/2025-12-11-13-25-59.png)

![](Asset/Ruby/2025-12-11-13-26-22.png)

- 在HealthFrame下新建一个Image，用来放置头像，然后将资源中的头像添加到Image中，调整大小和位置

![](Asset/Ruby/2025-12-11-13-27-24.png)

- 为防止血条形变时，头像的位置发生改变，修改头像的锚点位置

![](Asset/Ruby/2025-12-11-13-30-45.png)

- 再添加一个Image，命名为Mask，作为血条的遮罩

![](Asset/Ruby/2025-12-11-22-55-32.png)

- 调整Mask的大小，使其刚好遮住血槽，并将其轴心移动到最左侧，这样当减少20%的血量时，将会从最右边减少，否则会从左右各减少10%

![](Asset/Ruby/2025-12-11-22-58-47.png)

- 在Mask下新建一个Image，用来放置血量，按住Alt键，点击锚点设置框中右下角的选项，使其填充到Mask中，然后不按住Alt键的情况下，点击锚点设置框中左上角的选项，使血量的锚点定位到Mask的左上角，当Mask的尺寸变化时，血量的尺寸将不受影响（可以测试一下）

![](Asset/Ruby/2025-12-11-23-01-13.png)

![](Asset/Ruby/2025-12-11-23-03-35.png)

- 点击Mask，为其添加Mask组件，取消Show mask graphic，这样Mask的白色背景就消失了

![](Asset/Ruby/2025-12-11-23-06-24.png)

## 血条脚本
- 创建血条脚本，并使其返回一个静态对象，这样能保证当游戏中有多个场景时，只存在一个血条，将脚本挂在HealthFrame下，并将HealthFrame中的Mask赋给脚本中的mask

![](Asset/Ruby/2025-12-11-23-20-27.png)
![](Asset/Ruby/2025-12-11-23-18-15.png)

- 设置血量方法

![](Asset/Ruby/2025-12-11-23-17-06.png)
![](Asset/Ruby/2025-12-11-23-17-36.png)

- 在Ruby的脚本中，设置血量的位置调用血条的SetValue

![](Asset/Ruby/2025-12-11-23-23-34.png)

# NPC

## 创建NPC
- 将资源中的NPC下的三个切割后的图片拖拽到Hierarchy中，会将这三个图像打包成一个动画，保存下来

![](Asset/Ruby/2025-12-11-23-29-37.png)
![](Asset/Ruby/2025-12-11-23-29-05.png)

- 默认的动画会很快，点击NPC，按下Ctrl+6打开动画编辑界面，将帧数改为3或者6

![](Asset/Ruby/2025-12-11-23-31-26.png)

- 给NPC添加碰撞体组件，设置碰撞体大小，然后将其拖拽到预制体文件夹中

![](Asset/Ruby/2025-12-11-23-33-21.png)

- 设置NPC的图层为NPC

![](Asset/Ruby/2025-12-11-23-34-06.png)

## 交互
- 在Ruby的脚本Update方法中，使用物理射线投射，获取NPC

![](Asset/Ruby/2025-12-11-23-40-58.png)

- 在NPC下新建一个Canvas,并设置其参数，将Canvas移动到NPC头顶，这里测试一下看RenderMode的差异，设置OrderInLayer，防止其被其他的东西遮住

![](Asset/Ruby/2025-12-11-23-45-06.png)

- 在Canvas中添加一个Image作为对话框的背景，然后在锚点设置框中，按住Alt键，点击右下角的选项，使其填充画布

![](Asset/Ruby/2025-12-11-23-48-03.png)

- 添加文本，同样设置锚点使其填充整个Image，然后再调整一下，使文本区域在背景中的蓝色区域

![](Asset/Ruby/2025-12-11-23-50-16.png)
![](Asset/Ruby/2025-12-11-23-52-20.png)

- 输入文字，选择字体

![](Asset/Ruby/2025-12-11-23-54-46.png)

## 显示或隐藏对话框脚本
- 创建NPC脚本，挂给NPC，给Dialog赋值

![](Asset/Ruby/2025-12-12-12-35-03.png)
![](Asset/Ruby/2025-12-12-12-36-59.png)
![](Asset/Ruby/2025-12-12-12-38-30.png)

- 在Ruby的脚本中，在NPC旁边面对NPC按下X键是，显示对话框

![](Asset/Ruby/2025-12-12-12-40-36.png)

## 添加动态字体
- 在网下载好字体后，在unity中创建Fonts文件夹，将 .ttf 文件拖到这个文件夹中，右键新建TextMeshPro的字体资源

![](Asset/Ruby/2025-12-12-12-47-18.png)

- 将字体拖拽到TextMeshPro中

![](Asset/Ruby/2025-12-12-12-48-24.png)

## 添加静态字体
- 打开字体资源创建窗口

![](Asset/Ruby/2025-12-12-12-49-17.png)

- 选择字符源

![](Asset/Ruby/2025-12-12-12-50-20.png)

- 常用字资源
  https://pan.xunlei.com/s/VMovwhBhUQOnMxDnufbrVqRA1 提取码:3986

![](Asset/Ruby/2025-12-12-12-52-41.png)

![](Asset/Ruby/2025-12-12-12-50-59.png)

- 常用字资源太多的话，读条会很慢，可以使用较少的资源，也可以自己写，加载完成后点保存即可

![](Asset/Ruby/2025-12-12-12-54-02.png)

## 分页显示
- 如果要显示的文本太多，会溢出文本框，设置分页显示，默认显示第一页

![](Asset/Ruby/2025-12-12-12-57-02.png)

- 在NPC的脚本中

![](Asset/Ruby/2025-12-12-13-00-22.png)
![](Asset/Ruby/2025-12-12-13-01-29.png)
![](Asset/Ruby/2025-12-12-13-02-14.png)

- 在update中，其实可以不用每次update都获取_totalPages，可以试试放在其他地方

![](Asset/Ruby/2025-12-12-13-05-10.png)

- 赋值TMP对象

![](Asset/Ruby/2025-12-12-13-06-49.png)


- 设置音频为3D时，可设置音频范围以及衰减效果

![](Asset/Ruby/2025-12-16-12-46-46.png)

- 因为MainCamera的z轴默认是-10，吧AudioListener挂在MainCamera上，可能达不到音频的播放范围，所以在摄像机上添加一个空对象，将AudioListener挂在空对象上，并将空对象的z轴设为10，来抵消MainCamera的-10

![](Asset/Ruby/2025-12-16-12-52-44.png)

# 发布
- 设置游戏信息

![](Asset/Ruby/2025-12-16-12-57-27.png)

- 打包
![](Asset/Ruby/2025-12-16-12-58-23.png)

- 添加关卡

![](Asset/Ruby/2025-12-16-12-59-15.png)

- 操作系统支持
  在UnityHub中添加相关操作系统的支持，BuildAndRun才能点
![](Asset/Ruby/2025-12-16-12-59-53.png)