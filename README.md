# 任务管理网站

## 简介
大三Vue作业
由Vue实现的一个简单的任务管理网站
具有添加任务，修改任务进度，删除任务等功能

## 子组件功能
| 名称	    | 功能                        |
| :----------  |:------------------------ |
| Navmen    | 目录显示                    |
| MyList	    | 列表显示                 |
| MyForm	| 添加或修改任务               
| MyCalenda  | 日历                       |
| AllList	    | 过滤条件，数据传给MyList|


# The format of the pickle files
Each pickle file corresponds to an action recognition dataset. The content of a pickle file is a dictionary with two fields: split and annotations

1.Split: The value of the split field is a dictionary: the keys are the split names, while the values are lists of video identifiers that belong to the specific clip.


2.Annotations: The value of the annotations field is a list of skeleton annotations, each skeleton annotation is a dictionary, containing the following fields:

# NTU-15
数据集下载：xxx.pkl
数据集介绍：从NTU-120中挑选出15种危险行为，15种行为
| 原ntu120的标签	    | 行为名称                        |
| :----------  |:------------------------ |
|42	| falling
|43	| touch head (headache)
|44	| touch chest (stomachache/heart pain)
|45	| touch back (backache)
|46	| touch neck (neckache)
|47	| nausea or vomiting condition
|49	| punching/slapping other person
|50	| kicking other person
|51	| pushing other person
|56	| touch other person's pocket
|105| hit other person with something
|106| wield knife towards other person
|107| knock over other person (hit with body)
|108| grab other person'stuff
|109| shoot at other person with a gun

危险行为定义：单人的危险行为，例如headache，backache，因疾病引发的行为。
             双人危险行为，例如pushing，kicking等回对他人或自身造成伤害的行为。
关于train和test划分方式，我们与NTU RGB+D 120中划分方式相同
Split:danger_xsub_train和danger_xsub_test分别是NTU-15的训练集和测试集划分方式

# Anomal action-18
数据集下载:xxx.pkl
数据集介绍：在NTU-15的基础上，我们从现有的常用的RGB行为识别数据集（）中挑选出符合我们对危险行为定义的行为，并将它们整合在一起。
以下是我们在hmdb51，ucf101，kinetics400上选取的行为


hmdb51			
label	name	train_num	val_num
12	fall floor	70	30
16	handstand	70	30
17	hit	70	30
27	punch	70	30
35	shoot bow	70	30
36	shoot gun	70	30
![image](https://github.com/1211186431/tastWeb/assets/56494120/87e297b6-325f-404a-8bfa-cd94760b9bb7)


ucf101			
label	name	train_num	val_num
37	handstandWalking	77	34
70	punch	121	39
73	RockClimbingIndoor	103	41
74	RopeClimbing	85	34
![image](https://github.com/1211186431/tastWeb/assets/56494120/eea704e5-0180-4bb8-852b-0351930a1ae6)


kinetics400			
label	name	train_num	val_num
105	drop kicking	551	47
278	rock climbing	967	50
314	slapping	290	49
396	wrestling	326	50
![image](https://github.com/1211186431/tastWeb/assets/56494120/714c038d-c8a6-47ae-bcb6-0bbe358a1372)


我们将行为种类相似的行为进行合并，以NTU-15为基础得到了Anomal action-18

label	name	train_num	test_num	Source dataset			
0	falling	741	305	NTU-15、HMDB51	dataset2	dataset3	dataset4
1	touch head	670	276	NTU-15	hmdb		
2	touch chest	671	276	NTU-15			
3	touch back	672	276	NTU-15			
4	touch neck	672	276	NTU-15			
5	vomiting	671	275	NTU-15			
6	punch/slap	1148	392	NTU-15、HMDB51、UCF101、Kinetics			
7	kicking	1220	323	NTU-15、Kinetics	HMDB51	ucf	HMDB51
8	push	670	276	NTU-15	Kinetics		
9	touch pocket	670	275	NTU-15			
10	handstand	147	64	HMDB51、UCF101			
11	climbing	1155	125	UCF101、Kinetics	ucf		
12	wrestling	326	50	Kinetics	UCF101	Kinetics	
13	shoot bow	70	30	HMDB51			
14	hit	838	1181	NTU-15、HMDB51			
15	knock over	383	576	NTU-15	NTU-15	HMDB51	
16	grab	384	575	NTU-15			
17	shoot gun	453	605	NTU-15、HMDB51			
![image](https://github.com/1211186431/tastWeb/assets/56494120/cc58d480-3e58-40d3-b7f4-7572caa2e398)

train和test的划分方式遵循原数据集中的划分方式
视频文件下载地址：video.zip
视频生成关键点：bash tool/......

# Open environment-12
数据集下载
数据集介绍：我们自己采集的开放场景下的危险行为数据集。共有xxx个视频数据，1920×1080的分辨率。我们从NTU-15中选取12种行为，3名演员进行录制，场景更为常见，行为更贴切正常生活中的危险行为。
行为种类如下
| 标签	    | 行为名称                        |
| :----------  |:------------------------ |
|1	| falling
|2	| touch head (headache)
|3	| touch chest (stomachache/heart pain)
|4	| touch back (backache)
|5	| touch neck (neckache)
|6	| nausea or vomiting condition
|7	| punching/slapping other person
|8	| kicking other person
|9	| pushing other person
|10	| touch other person's pocket
|11| hit other person with something
|12| knock over other person (hit with body)

我们选取1/3的视频进行在NTU-15的预训练模型上训练，使用2/3的视频进行测试
视频数据下载：video2.zip


