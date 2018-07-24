# 综合实验文档

## 小程序体验版二维码

<img src="https://github.com/Imalne/RollingBall/blob/master/README/img/qrcode.jpg" width="200px"/> 

## 游戏策划与功能

本次微信小游戏综合实验中本组完成的游戏名称为Rolling Ball，是一款使用Laya 3D引擎制作的3D小游戏。游戏主要元素包括玩家——一个在场景中穿梭的小球，以及漂浮在空中并四处移动的或者固定的跳板，玩家需要通过触摸屏幕左右移动小球使其能够落在跳板上不掉落，并避开跳板上的障碍物，障碍物的高度会随机地增长或下降。跳板上除了障碍物，还有两种道具，一种能够加快前进速度，一种能够增加跳跃高度，玩家能够利用这些道具大幅增加分数，但同时加速过快或者跳跃不合适也会导致玩家的死亡，需谨慎选择。当分数增加后，游戏难度也会提升，体现为球的速度随之增加。玩家在游戏中还能听到有趣的音效，包括背景乐、结束乐、球的落地、加速音效等。

## 界面布局与设计

### 启动界面

包括一个在左右移动的标题和一个开始按钮

<img src="https://github.com/Imalne/RollingBall/blob/master/README/img/start.jpg" width="250px"/>

### 游戏界面

主要元素包括玩家操控的小球、跳板，跳板中长方体障碍物、跳板上黄色箭头状的加速道具、跳板末端为会增加跳跃高度的道具。游戏左上角显示当前分数，右上角有一个暂停按钮，点击即暂停游戏并进入暂停界面。

<img src="https://github.com/Imalne/RollingBall/blob/master/README/img/game.jpg" width=250/>

### 暂停界面

暂停界面的主体为三个按钮，从上至下分别为继续游戏、重新开始和结束游戏。

<img src="https://github.com/Imalne/RollingBall/blob/master/README/img/pause.jpg" width="250px"/>

### 结束界面

结束界面会显示本局得分、历史最高分、用户及好友的排行榜，每页显示三名用户，包括用户的昵称、头像、历史最高分，排行榜默认显示页为用户所在页，通过下方的左右按钮可显示前一页和后一页。红色的按钮点击后重新开始游戏。

<img src="https://github.com/Imalne/RollingBall/blob/master/README/img/end.jpg" width="250px"/>



## 小组成员

* 秦睿 2016013241 软件61
* 林灏 2016013240 软件61

## 开发环境

### IDE：

* LayaAir 1.7.19.1 beta 
* 微信web开发者工具 v1.02.1807120

### 语言：

TypeScript

### 游戏引擎：

LayaAir 1.7.19.1 beta

### 测试机型：

* iPhone 8 Plus 分辨率：1920×1080
* Samsung Galaxy S8+ 分辨率：2220×1080
* HUAWEI Mate 9 分辨率：1920×1080

### 微信版本：

* Android：6.6.7
* IOS: 6.7.1

### 服务器工具：

Node.js Express

## 技术实现方案与重难点

### 主要技术方案：

本次实验选择的是使用Laya引擎构建3D小游戏，通过Laya官方的相关API以及小游戏转换，使用TypeScript语言完成游戏的制作。服务器方面则采用Express框架，用以响应获取用户微信`openID`的请求

### 程序结构：

#### 游戏主域（RollingBall文件夹）：

- `LayaAir3D.ts`：游戏逻辑的主体部分，包括场景的构建、游戏主逻辑的控制、游戏的结束暂停等。
- `Block.ts / BlockScript.ts`：跳板的构建、碰撞事件的处理
- `Obstacle.ts / ObstacleScript.ts`：跳板上障碍物的构建、障碍物碰撞处理
- `PlayerBall.ts / PlayerBallScript.ts`：玩家小球的构建、小球碰撞事件处理
- `SprBoard.ts / SprBoardScript.ts`：增加高度道具的构建、碰撞事件处理
- `RankList.ts`：主子域共享画布添加，绘制排行榜
- `StartMenu.ts / PausePage.ts / PauseUI.ts / EndPage.ts`：启动、暂停、结束UI界面扩展类，处理点击事件
- `CameraScript.ts`：

#### 游戏子域（RollingBallRank文件夹）：

- `RankListUI.ts`：获取用户的好友游戏数据、绘制排行榜

#### 服务器（RollingBallServer文件夹）：

- `users.js`：处理用户获取微信`openID`的请求。接收用户上传的`code`，并提交微信服务器获取`openID`并返回。
- `app.js / www`：Express主体部分

### 重点与难点：

* Laya相关API的使用，由于官方文档的不完整和相关教程有限，部分API的使用需要自行探索
* Laya的屏幕缩放API无法很好适配分辨率大于16:9的手机屏幕，解决方案为在代码中进行判断，并对屏幕布局进行调整，对背景等图案进行缩放
* 不同手机的贡献画布的分辨率有差异，在使用HUAWEI Mate 9等16:9的1080p手机进行测试时发现共享画布的面积小于实际子域画布面积，而Galaxy S8+等分辨率为18:9的手机共享画布比例也与子域画布比例不同，在代码中都需要进行判断和调整
* 用户`openID`的获取由于微信的安全政策限制，无法通过`wx.request`API完成，需要自行构建服务器并通过用户上传的相关`code`来获取并返回

## 游戏测试

游戏通过在iPhone 8 Plus、Galaxy S8+、HUAWEI Mate 9等机型上测试，未出现明显的bug。游戏过程顺畅，帧率维持在60FPS左右，界面无黑边、拉伸等情况。排行榜也能很好地显示用户自身及好友排名。

## 游戏亮点

* 跳板的四处移动以及障碍物的随机增长增加了游戏难度、游戏的等级提升增加了游戏的趣味性，3D场景的设置使得玩家更有真实体验，仿佛在云端穿梭。
* 排行榜的实现，使得用户能够看到好友的分数及排名。
* 暂停界面按钮弹出实现了一个渐次淡入的动画。
* 游戏素材为手工绘制结合部分网络素材的自主设计。
