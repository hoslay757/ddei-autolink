# ddei-autolink

#### 介绍
该包用于在两个图形之间生成链接路径，实现自动连线功能、并返回中间节点。<br>
特性：<br>
   1.自动躲避障碍<br>
   2.寻找看起来尽可能美观的路径功能<br>
   3.够通过优先级影响寻路结果<br>

感兴趣的朋友可以登陆ddei在线设计器体验效果<br>
步骤：<br>
   注册账号 >>>> 创建设计图 >>>> 创建图形和连线。<br>

https://www.ddei.top <br>

#### 安装教程

1.  `npm i ddei-autolink`

#### 使用说明

1.  引入包
    `import { calAutoLinePath, getRecommendPath } from 'ddei-autolink'`

2.  构建开始点和结束点信息
    参考以下例子构建json对象，包含：点坐标、开始图形的外接矩形、开始点相对于所在外接矩形中心点的角度/方向 三个属性
    
```
let startPoint = {
        point: {   //点坐标
          x: x,
          y: y
        },
        rect: {x:startX,y:startY,x1:endX,y1:endY,width:width,height:height}, //外接矩形
        angle: -90,  //角度-90上、0右、90下、180左，传入角度可以不传方向
        direct:1   //方向1上、2右、3下、4左，传入方向可以不传入角度
      },
```


3.  构建障碍物（该步骤可以省略）
    参考以下例子构建json数组，每个子元素代表一个障碍物，包含：外接点集合，开始结束标识。开始和结束点所在的外接矩形也需要加入障碍物中。
    
```
let obis = []
    //创建一个障碍物并加入障碍物数组中
    let obj = { 
          points: [ //当前障碍物的多个外接点,按顺时针顺序传入，点的数量>2
              {x:0,y:0},
              {x:100,y:0},
              {x:100,y:100},
              {x:0,y:100}
          ],
          isStartOrEnd:true/false //开始结束标识,如果当前障碍物是开始结束节点所在的图形，则传入true，否则false
     }
    obis.push(obj)
```


4.  获取推荐路径(该步骤可以省略）
    通过调用getRecommendPath方法获取推荐路径，推荐路径是预置路径，可以在寻路时影响寻路结果，使寻路结果更倾向于美观
    其中sAngle、eAngle 为开始点和结束点的角度，取值为-90上、0右、90下、180左
    startPoint, endPoint 为开始点和结束点
    startRect, endRect 为开始和结束点所在的外接矩形
    这六个参数都可以在步骤1获取。

    
```
let recommendPaths = getRecommendPath(sAngle, eAngle, startPoint, endPoint, startRect, endRect)

```


5.  调用calAutoLinePath方法生成路径
   
    
```
let linePathData = calAutoLinePath(
      startPoint, //第二步构建的开始点
      endPoint,   //第二步构建的结束点
      obis,       //第三步构建的障碍物
      {           //如果需要使用推荐路径或者强制路径功能，则需要传入此参数，其余情况可以不传
        recommendPaths: recommendPaths,   //第四步的返回值
        forcePaths: forcePaths,       //强制路径，如果没有可以不传入
        recomWeight: 100,             //推荐路径权重，默认100
        rectMidWeight: 50             //矩形中线权重，默认50
      })
```



6.   读取pathPoints，获取连线路径，第一个元素为开始点，最后一个元素为结束点，其余为中间经过的点
     
```
let pathPoints = linePathData.pathPoints
     let startPathPoint = pathPoints[0] //开始点
     let endPathPoint = pathPoints[pathPoints.length-1] //结束点
```


#### 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码
4.  新建 Pull Request
