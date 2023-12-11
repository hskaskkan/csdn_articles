> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/JamSlade/article/details/127361567)

设计过程
----

1.  数据库模式
2.  访问更新数据库的**程序**
3.  控制数据访问的**安全模式**

阶段：

1.  概念设计阶段：实体联系图
2.  逻辑设计阶段：E-R 图映射到关系模式
3.  物理设计阶段： 指明数据库文件组织格式、索引结构

可能的问题：

1.  冗余
2.  不完整

实体 - 联系模型
---------

E-R 模型是一种语义模型，将现实世界**含义**，相互**关联**映射到概念模型方面

有三个基本概念

1.  实体集
2.  联系集
3.  属性

数据库可建模为

1.  实体的集合
2.  实体间联系

### 实体集

实体解释：现实世界可区别于其他所有对象的 “对象”  
实体集解释：相同类型的实体集合

实体集**外延**指属于实体机的实体的实际集合  
（类比对象的实际取值集合）

实体有属性：他是实体集中每个成员拥有的描述性性质，实体属性都对应一个值

### 联系集

联系：多个实体之间的相互联系  
联系集是同类型联系的集合

联系集也可以有**属性**

如 instructor 和 student 之间的联系集 advisor 可以有属性 date 表示教师称为学生导师的日期

**联系集中实体的角色**

实体在联系中扮演的功能称为**实体的角色**

角色是隐含的，通常不指定

同样的实体集参与一个联系集多于一次，这类联系集称为**递归**联系集

```
如：课程先修关系prereq
需要用角色指明实体如何参与联系实例
```

**联系集的度**  
参与联系集的实体集数目为联系集的度

**二元联系集**

1.  设计两个实体集
2.  数据库系统中大多数联系集都是二元的  
    两个实体集以上的联系较少

### 属性

实体通过一组属性表示，属性是实体集中每个成员拥有的描述性性质

```
instructor = (ID, name, street, city, salary)
course = (course_id, title, credits)
```

域 – 每个属性可取值的集合，也叫做**值域**

属性类型：

1.  简单 or 复合属性
2.  单值 or 多值属性  
    比如多值属性 phone_numbers
3.  派生属性 or 基属性  
    派生属性可从别的属性派生  
    派生属性值不储存，需要的时候计算出来（如 ave

符合属性可以有层次![](https://img-blog.csdnimg.cn/733bb9d2c32b4c9fac6079903778cd0f.png)

约束
--

映射基数约束

**基数映射（基数比率）**：一个实体通过一个联系集能关联的实体的个数

对二元联系集来说，映射基数为以下情况：

1.  1 to 1
2.  1 to multi
3.  multi to 1
4.  multi to multi  
    两个集合（如 A）中的某些元素可能并不映射到另一集合（B）中的任一元素

### 参与约束

若实体集 E **每个实体**都参与到联系集 R 的至少一个练习中，实体集 E 在联系集 R 的参与称为**全部的 (total)**

若实体集 E 只有**部分实体**参与到 R 的联系中，上面称为**部分的 (partial)**

### 实体集的码

一个实体集的**超码**是一个或多个可以用来唯一地标识实体的属性（集）

一个实体集的**候选码**是最小的超码

1.  ID 是 instructor 的候选码
2.  course_id 是 course 的候选码

主码：被选择区分实体的候选码  
主码只有一个，超码可以有多个

### 联系集的码

如果联系集 R 没有属性，那么属性集合  
p r i m a r y − k e y ( E 1 ) ∪ p r i m a r y − k e y ( E 2 ) ∪ … ∪ p r i m a r y − k e y ( E n ) primary-key(E1) ∪ primary-key(E2) ∪…∪ primary-key(En) primary−key(E1)∪primary−key(E2)∪…∪primary−key(En)  
描述 R 的联系

如果联系集 R 有属性 a 1 ， a 2 , … , a m a_1，a_2, …, a_m a1​，a2​,…,am​，那么属性集合  
p r i m a r y − k e y ( E 1 ) ∪ p r i m a r y − k e y ( E 2 ) ∪ … ∪ p r i m a r y − k e y ( E n ) ∪ a 1 ， a 2 , … , a m primary-key(E1) ∪ primary-key(E2) ∪…∪ primary-key(En)∪ {a_1，a_2, …, a_m} primary−key(E1)∪primary−key(E2)∪…∪primary−key(En)∪a1​，a2​,…,am​  
描述了集合 R 中的一个联系

**相关实体集的主码行成联系集的超码**  
以上两种情况下  
p r i m a r y − k e y ( E 1 ) ∪ p r i m a r y − k e y ( E 2 ) ∪ … ∪ p r i m a r y − k e y ( E n ) primary-key(E1) ∪ primary-key(E2) ∪…∪ primary-key(En) primary−key(E1)∪primary−key(E2)∪…∪primary−key(En)  
总是构成**联系集**的一个**超码**

比如  
`(s_id,i_id)`是 advisor 的超码  
✓表示一对实体集在一个特定的联系集中只能有一个  
联系

决定候选码时必须考虑联系集的映射基数  
✓一对一、一对多、多对一、多对多

### 冗余属性

假设有实体集

1.  instructor 实体集有 dept_name 属性
2.  department 实体集和联系集
3.  inst_dept，联系 instructor 和 departmen

instructor 中的 **dept_name 属性是冗余的**，因为有一个明确的关系 inst_dept 来联系 instructor 和 department  
✓dept_name 属性重复了 inst_dept 联系中的信息，应该从 instructor 中除去

实体－联系图
------

![](https://img-blog.csdnimg.cn/7b68ee2ae0df438095e097fae65e399c.png)

1.  矩形代表实体集
2.  属性在实体矩阵中列出
3.  构成主码的属性以下划线标明
4.  菱形代表联系集

全部参与（两条线表示）：实体集每个实体都参与到联系集至少一个联系中

如：`section`全部参与`sec_course` 每个`section`都有一个`course`  
![](https://img-blog.csdnimg.cn/9833d764f3f74d1f86fd7c0d4fce492f.png)  
部分参与：某些实体不参与到联系集中的任何一个联系  
例：部分 instructor 参与到 advisor 中  
![](https://img-blog.csdnimg.cn/0af9f6189f204ab2882c919d28570a2d.png)

### 基数约束

联系集和实体集之间用箭头 → \rightarrow →表示 **“一”**  
或一条线段表示 **“多” **表示技术约束  
![](https://img-blog.csdnimg.cn/e626de1db4e74b55b109f368361b3312.png)  
一对一联系  
instructor 和 student 之间的**一对一 ** 联系

1.  一个 insturctor 通过 advisor 至多与一个 student 相联系
2.  一个 student 通过 advisor 至多与一个 insturctor 联系  
    ![](https://img-blog.csdnimg.cn/a3951cd973ce4b7ba03079a2448d54a6.png)  
    instructor 与 student 之间的**一对多**联系  
    一个 instructor 通过 advisor 与多个 student（包括 0 个）相联系  
    一个 student 通过 advisor 至多与一个 instructor 相联系  
    ![](https://img-blog.csdnimg.cn/41179c7e31924120bf156ecb156b3ca0.png)

instructor 与 student 之间的**多对一**联系  
一个 instructor 通过 advisor 至多与一个 student 相联系  
一个 student 通过 advisor 与多个 instructor（包括 0 个）相联系  
![](https://img-blog.csdnimg.cn/f3090e0c186740dfa0dd82980ef84d2f.png)  
多对多  
一个 instructor 通过 advisor 与多个 student（包括 0 个）相联系  
一个 student 通过 advisor 与多个 instructor（包括 0 个）相联系  
![](https://img-blog.csdnimg.cn/60b6d2cfe247405daaad821737efb4a0.png)  
基数限制也可以用参与约束来表示  
![](https://img-blog.csdnimg.cn/2731ca08f4a4438585be8017b800d86d.png)  
具有复合、多值、派生属性的实体  
![](https://img-blog.csdnimg.cn/ce5acdcd3cf1496ba9ed8890492be186.png)

### 角色

一个联系的实体集不需要唯一  
一个实体集中的元素每次出现都在关系中代表一个 “角色”

`course_id`和`prereq_id`被称为角色  
![](https://img-blog.csdnimg.cn/9014fe66c2ea4741bb9e504aa05a8f15.png)

### 三元联系 E-R 图

![](https://img-blog.csdnimg.cn/a70f0088c9ba4812bbeac477c2edfdf4.png)  
**基数约束**：  
我们只允许在一个三元（或三元以上）联系集外有**一个箭头**来表示基数约束

```
从proj_guide到instructor的箭头表示每个学生在每个项目上至多有一个导师来指导
```

有多于一个的箭头，那就会有两种解释

```
R 是 A, B 和C 之间的三元联系集，并且有两个箭头指向B和C，这可以表示为
```

1.  每个在 A 中的实体都与来自 B 和 C 的至多一个实体相关联
2.  每个来自 (A, B) 的实体对都与来自 C 的至多一个实体相关联，而每个来自 (A, C) 的实体对都与来自 B 的至多一个实体相关联

**弱实体集**

1.  一个_没有足够的属性_形成_主码_的实体集叫做弱实体集
2.  弱实体集必须与_标识_或_属主实体集_（owner entity set）关联才有意义  
    弱实体集存在依赖于标识实体集
3.  将弱实体集与其标识实体集相连的联系称为_标识性联系_  
    标识联系集以双边框菱形表示
4.  弱实体集通过一个全部参与的、（多对一或一对一）的联系集与标识实体集联系
5.  弱实体集的_分辨符_是使得我们区分弱实体集中实体的属性集合，也称为该弱实体集的_部分码_
6.  _弱实体集的主码_由_标识实体集的主码_和该弱实体集的_分辨符_共同组成
7.  弱实体集的分辨符用虚下划线表示
8.  标识联系集以双边框的菱形表示
9.  section 的主码 – (course_id, sec_id, semester, year)  
    ![](https://img-blog.csdnimg.cn/8d4a49d59dde460a99a48cd1010a8c59.png)
10.  强实体集的主码并不存储于弱实体集  
    如果`course_id`被隐含的存储，`section`会是一个强实体集，这样`section`和`course` 之间的联系集将会重复存储`course_id`属性
11.  弱实体集可以_参与标识性联系以外的其他联系_，也可能与不_止一个_标识实体集关联
12.  在某些情况下，数据库设计者会选择将一个弱实体集表示为属主实体集的一个（多值）复合属性

```
如果弱实体集只参与标识性联系，而且其属性不多，建模时更适合将其作为属性
	反之，更适合将其表示为弱实体集
```

转换为关系模式
-------

一个符合 E-R 模型的数据库可以表示为一些关系模式的集合

1.  **实体集**和**联系集**都可以用表示数据库内容统一的关系模式表示
2.  每个实体集和联系集都有一个特有的关系模式对应，且有对应的名字
3.  每个关系模式都有一些列，每个列有唯一名字

### 简单属性实体集表示

从强实体集转换模式与强实体集有相同属性、主码  
`student(ID, name, tot_cred`

从弱实体集转换的模式包含**弱实体集的属性**和**标识强实体集的主码**

该模式主码有**强实体集**主码和弱实体集分辨符组合  
`section( course_id, sec_id, sem, year)`

需要建立相应的**外码约束**和**完整性约束**（如联机删除  
![](https://img-blog.csdnimg.cn/6161b6d98a3f4ea8ac974c33332dccde.png)  
复合属性通过为每个子属性创建单独的属性

```
实体集instructor有复合属性name：由first_name、
middle_initial和last_name构成
```

```
转化关系模式后，该实体集具有name_first_name、
name_middle_name和name_last_name
```

不考虑多值属性的话，扩展的 instructor 模式是

```
instructor(ID, first_name, middle_initial, last_name, 
street_number,street_name,apt_number, 
city, 
state, 
zip_code, 
date_of_birth)
```

### 多值属性

实体集 E 的多值属性 M 用一个独立的模式 EM 表示

模式 EM 包含对应于的 E 主码及多值属性 M 的属性  
例子：instructor 的**多值属性** phone_number 在模式中表示为  
i n s t p h o n e = ( I D ‾ , p h o n e _ n u m b e r ‾ ) inst_phone= (\underline{ID}, \underline{phone\_number}) instp​hone=(ID​,phone_number​)

多值属性的每个值映射到关系模式 EM 的每一个独立元组

1.  一个实体 instructor 的主码 22222 及电话号码 456-7890 和 123-4567 映射到两个元组: (22222, 456-7890) 和 (22222, 123-4567)
2.  在关系模式 EM 上建立参照实体集 E 关系模式主码的**外码约束**

`time_slot`除了主码外只有一个多值属性  
不需要建立一个对应于实体集的模式，只需要简历一个对应于多值属性的关系即可  
t i m e s l o t ( t i m e _ s l o t _ i d , d a y , s t a r t _ t i m e ‾ , e n d t i m e ) time_slot(\underline{time\_slot\_id, day, start\_time}, end_time) times​lot(time_slot_id,day,start_time​,endt​ime)  
sec_time_slot 中的 time_slot_id 属性在这样处理后不能作为被参照的外码  
![](https://img-blog.csdnimg.cn/339a3cf2f89b476586e264f1fe37178c.png)

### 表示

联系集关系模式属性的选择

需要为联系集属性建立**外码约束**  
例子：advisor 模式  
a d v i s o r = ( s _ i d ‾ , i _ i d ) advisor = (\underline{s\_id}, i\_id) advisor=(s_id​,i_id)

### 冗余 - 合并

多对一、一对多联系集的模式  
若 “多” 方参与的是**全部的 total** 那么可以将**多方实体集**和联系集模式**合并成**单个包含两个模式所有属性并集的关系模式

合并后模式中加入**原联系集的外码约束**

```
将属性dept_name加入到实体集instructor的模式中，而不是新建一个联系集inst_dept的模式
```

![](https://img-blog.csdnimg.cn/352c3e9f14c941e18b731139fd5b53eb.png)  
如果若 “多” 方参与的是**部分的 partial**，可以通过使用空值进行模式合并。  
转换为关系模式的时候。联系集中的 “一” 方的相关属性不能设为 not null

一般而言：  
连接弱实体集与强实体集的**标识性联系集**转换出的模式是_冗余的_

```
例子：section（弱实体）的关系模式已经包含了出现在
sec_course模式中的属性
```

实体－联系设计问题
---------

使用实体集 vs. 属性

![](https://img-blog.csdnimg.cn/42bf8ac9fbb94b80aaaa24fa279fee52.png)  
phone 作为实体集有额外信息

注意避免以下错误：

1.  实体集主码作为另一个实体集的属性
2.  相关实体集的主码属性作为联系集的属性

若信息与其他实体集相联系，可以继续设置更多联系集  
**（当描述实体之间可能发生的行为时采用联系集）**

创建一个实体集可以将**非二元联系集** — 转换 —> 为 **二元联系集**  
![](https://img-blog.csdnimg.cn/2b15f2c65349426db40c72b93c9ef9d7.png)  
所有非二元的联系集可以用多个二元联系集来表示，但有时 n 元的联系集能够更清楚的表示几个参与的实体集之间的联系

### 联系属性的布局

联系集的描述性属性 – 是 联系集的？还是实体集的？  
由**映射基数约束**决定

1.  一对多，放到参与联系的 “多” 方实体集
2.  一对一，可放任意一个
3.  属性—由**参与的实体集**联合决定而非单独实体集决定。必须放到**多对多**联系集中

扩展的 E-R 特性
----------

**自顶向下**：从初始实体集到一系列不同层次的实体子集细化

**特化 specialization**：实体集内部分组过程

子集的实体再某些方面区别于实体集中的其他实体

1.  自己成为了较低层的实体集
2.  可能有高层实体集没有的属性
3.  或参与高层实体集不参与的联系集中

特化从特化实体指向另一个实体的空箭头表示  
![](https://img-blog.csdnimg.cn/80b0b826a9e348248d07f2aa6a4a426b.png)

**自底向上**：  
多个实体集根据共同优点特征综合为一个叫高层实体集

**概化 generalization**

1.  这些子集成为较低层实体集
2.  可能有高层实体集没有的属性
3.  或参与到高层实体集不参与的联系集中

概化和特化互逆，可以互换使用  
他们在 E-R 图表示享同，区别在于不同的出发点和目标

高层、底层实体集可以形成**超类、- 类**

UML
---

UML（Unified Modeling Language）: 统一建模语言  
➢ UML 包含了将整个软件系统图形化的很多组件  
➢ UML 类图与 E-R 图类似，仅有一些不同  
![](https://img-blog.csdnimg.cn/277d2f2909c349a08b512490b5010633.png)  
![](https://img-blog.csdnimg.cn/06ceaddfc3874799a5498b1bb06bb720.png)

总结
--

➢ 什么是实体、联系、实体－联系数据模型？  
➢ 什么是弱实体集、强实体集、属性？  
➢ 什么是映射基数、参与度约束？  
➢ 如何从 E－R 图转换为关系模式？  
➢ E-R 图设计和转换关系模式中有哪些常见问题？  
➢ 什么是特化、概化（泛化）？  
➢ 什么是 UML？