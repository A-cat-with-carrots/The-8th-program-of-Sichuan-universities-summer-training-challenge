# AI-Studio-英雄联盟大师

## 项目描述
本赛题属于典型的分类问题，要求选手根据英雄联盟玩家的实时游戏数据，预测玩家在本局游戏中的输赢情况，Baseline分数可达77分。在经过对于数据、参数以及模型的改进之后，分数可达84.1分

## 赛事介绍
实时对战游戏是人工智能研究领域的一个热点。由于游戏复杂性、部分可观察和动态实时变化战局等游戏特点使得研究变得比较困难。我们可以在选择英雄阶段预测胜负概率，也可以在比赛期间根据比赛实时数据进行建模。那么我们英雄联盟对局进行期间，能知道自己的胜率吗？

## 赛事任务
比赛数据使用了英雄联盟玩家的实时游戏数据，记录下用户在游戏中对局数据（如击杀数、住物理伤害）。希望参赛选手能从数据集中挖掘出数据的规律，并预测玩家在本局游戏中的输赢情况。

赛题训练集案例如下：
- 训练集18万数据；
- 测试集2万条数据；

```plain
import pandas as pd
import numpy as np

train = pd.read_csv('train.csv.zip')
```

对于数据集中每一行为一个玩家的游戏数据，数据字段如下所示：

* id：玩家记录id
* win：是否胜利，标签变量
* kills：击杀次数
* deaths：死亡次数
* assists：助攻次数
* largestkillingspree：最大 killing spree（游戏术语，意味大杀特杀。当你连续杀死三个对方英雄而中途没有死亡时）
* largestmultikill：最大mult ikill（游戏术语，短时间内多重击杀）
* longesttimespentliving：最长存活时间
* doublekills：doublekills次数
* triplekills：doublekills次数
* quadrakills：quadrakills次数
* pentakills：pentakills次数
* totdmgdealt：总伤害
* magicdmgdealt：魔法伤害
* physicaldmgdealt：物理伤害
* truedmgdealt：真实伤害
* largestcrit：最大暴击伤害
* totdmgtochamp：对对方玩家的伤害
* magicdmgtochamp：对对方玩家的魔法伤害
* physdmgtochamp：对对方玩家的物理伤害
* truedmgtochamp：对对方玩家的真实伤害
* totheal：治疗量
* totunitshealed：痊愈的总单位
* dmgtoturrets：对炮塔的伤害
* timecc：法控时间
* totdmgtaken：承受的伤害
* magicdmgtaken：承受的魔法伤害
* physdmgtaken：承受的物理伤害
* truedmgtaken：承受的真实伤害
* wardsplaced：侦查守卫放置次数
* wardskilled：侦查守卫摧毁次数
* firstblood：是否为firstblood

测试集中label字段win为空，需要选手预测。

## 评审规则
1. 数据说明

选手需要提交测试集队伍排名预测，具体的提交格式如下：

```plain
win
0
1
1
0
```

 2. 评估指标

本次竞赛的使用准确率进行评分，数值越高精度越高，评估代码参考：

```
from sklearn.metrics import accuracy_score
y_pred = [0, 2, 1, 3]
y_true = [0, 1, 2, 3]
accuracy_score(y_true, y_pred)
```

## 模型构建
1. 模型整体建立在 baseline 的基础之上，基本流程大致相同，主要区别体现在对于原有的数据处理、参数设置以及模型搭建等部分的调整上
2. 模型处理步骤：
  * 引入相关库，包括 pandas、paddle、numpy 等
  * 读取训练集、测试集
  * 绘制数据集中各项数据之间的图像，例如 wardsplaced & win 箱形图、wardskills & win 箱形图、kills/deaths & deaths 散点图等等，作为后续处理的依据
  * 删除包括 magicdmgtochamp、physdmgtochamp 在内的对于 win 标签影响较小的数据
  * 搭建模型，在原有的基础上增设线性变换层
  * 训练过程，在原有的基础上，减少学习率，增加训练轮数，减少批处理
  * 预测过程，将测试集作为输入调用训练完毕的模型
  * 将预测结果输出至 submission.csv 中，至此运行完毕

## 调优思路
1. 模型整体建立在 baseline 的基础之上，通过对于原有的数据处理、参数设置以及模型搭建等部分的调整，使得测试集得分由基础的77分上升至84.1分
2. 数据处理调优：
* 通过绘制 wardsplaced & win 箱形图、wardskills & win 箱形图、kills/deaths & deaths 散点图等图像，分析数据的相关特征，在原有的基础上，将 magicdmgtochamp、physdmgtochamp 等七项对于 win 标签影响较小的数据删除，以扩大其他数据的权重
3. 模型搭建调优：
* 在原有的基础上，于模型搭建部分增设线性变换层，以使模型对于数据的处理更加充分
4. 参数设置调优：
* 在原有的基础上，设置 learning_rate = 0.001、EPOCH_NUM = 100、BATCH_SIZE = 20
* 降低学习率以使训练过程更加易于控制，便于初学者观察过程、调整参数，同时，使训练过程也更加充分，对于数据的处理更为细致
* 增加训练循环次数的原理与学习率类似，通过增加循环次数以使数据利用得更加充分，并且每次循环都输出相应的 loss、acc，便于后续调参
* 减少批处理的数量亦是为了配合增加的训练循环次数，使数据利用得更加充分，对于测试集的预测更加精准

## 使用方式
> 在AI Studio上[运行本项目](https://aistudio.baidu.com/aistudio/usercenter)  
