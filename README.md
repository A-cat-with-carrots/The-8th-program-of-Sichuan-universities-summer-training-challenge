# AI-Studio-英雄联盟大师

## 项目描述
> 本赛题属于典型的分类问题，要求选手根据英雄联盟玩家的实时游戏数据，预测玩家在本局游戏中的输赢情况，Baseline分数可达77分。在经过对于数据、参数以及模型的改进之后，分数可达84.1分

## 赛事介绍
> 实时对战游戏是人工智能研究领域的一个热点。由于游戏复杂性、部分可观察和动态实时变化战局等游戏特点使得研究变得比较困难。我们可以在选择英雄阶段预测胜负概率，也可以在比赛期间根据比赛实时数据进行建模。那么我们英雄联盟对局进行期间，能知道自己的胜率吗？

## 赛事任务
> 比赛数据使用了英雄联盟玩家的实时游戏数据，记录下用户在游戏中对局数据（如击杀数、住物理伤害）。希望参赛选手能从数据集中挖掘出数据的规律，并预测玩家在本局游戏中的输赢情况。

赛题训练集案例如下：

训练集18万数据；
测试集2万条数据；
import pandas as pd
import numpy as np

train = pd.read_csv('train.csv.zip')
对于数据集中每一行为一个玩家的游戏数据，数据字段如下所示：

id：玩家记录id
win：是否胜利，标签变量
kills：击杀次数
deaths：死亡次数
assists：助攻次数
largestkillingspree：最大 killing spree（游戏术语，意味大杀特杀。当你连续杀死三个对方英雄而中途没有死亡时）
largestmultikill：最大mult ikill（游戏术语，短时间内多重击杀）
longesttimespentliving：最长存活时间
doublekills：doublekills次数
triplekills：doublekills次数
quadrakills：quadrakills次数
pentakills：pentakills次数
totdmgdealt：总伤害
magicdmgdealt：魔法伤害
physicaldmgdealt：物理伤害
truedmgdealt：真实伤害
largestcrit：最大暴击伤害
totdmgtochamp：对对方玩家的伤害
magicdmgtochamp：对对方玩家的魔法伤害
physdmgtochamp：对对方玩家的物理伤害
truedmgtochamp：对对方玩家的真实伤害
totheal：治疗量
totunitshealed：痊愈的总单位
dmgtoturrets：对炮塔的伤害
timecc：法控时间
totdmgtaken：承受的伤害
magicdmgtaken：承受的魔法伤害
physdmgtaken：承受的物理伤害
truedmgtaken：承受的真实伤害
wardsplaced：侦查守卫放置次数
wardskilled：侦查守卫摧毁次数
firstblood：是否为firstblood
测试集中label字段win为空，需要选手预测。

## 使用方式
> 在AI Studio上[运行本项目](https://aistudio.baidu.com/aistudio/usercenter)  
