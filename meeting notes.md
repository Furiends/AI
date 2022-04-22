## Furiends AI Team Weekly Meeting Minutes

### Date: 2022.4.16

#### NPO - 图片美化的建议（小韩）

- 图片的常见问题：
(1)照片清晰，背景清晰
(2)光线
(3)视角奇怪
(4)照片不够吸引人，不能体现动物的亲人属性
- 理想的照片：
(1)全身，平视，背景不杂乱
(2)希望一张图可以反映尽可能多的信息
- 解决方案：希望可以有用户拍摄指引（在框中拍摄）+cv工作（前后背景分离，光线调整)

#### CV

(papa)
- 收集到实拍的几十张图片
  见github
- 背景分离示例：removal.ai
- 如果照片是动物关在笼子中的，可能无法处理，需要首先拍出合格照片，减少编辑成本
- 加背景，增加生活信息

(乱乱)
- 通过爬虫收集到一些毛色分辨数据集（问题：是不是需要精度那么高？很难达成）
- 明确精度：识别大致颜色，大类品种

(橘子)
- 显著性检测 -> image matting 增加羽化效果
- 压缩图片示例：scale/resize 和 超分的区别（可以后续和前端确认）

#### Updates from other teams：
- （需要确认）目前第一步to b和to c端都从做小程序入手
- （需要确认）数据库框架

#### DA

（Caicai）

- Recommendation:
c.f. https://towardsdatascience.com/recommendation-systems-explained-a42fc60591ed 
NOT a binary classification → output how likely the pet and the user is a good match

- Method 1: Model-based Collaborative Filtering Recommendation
  - Con: not friendly for recommending new items – cold-start problem
  - Input：c.f. Siyi Notes
  - 动物：长相（玳瑁猫 – 性格普遍很好、三花猫很难被领养），年龄（超过2岁就比较难送出去），伤病史（猫瘟等），猫 / 狗，具体品种，性别，是否绝育（大部分不在乎是否绝育），是否打疫苗（大部分不在乎是否打疫苗）、颜色
  - 用户：
（1）职业：什么样的职业 - 什么种类high ranking
（2）是否wfh：Yes / No - rankin
（3）家住人口
（4）是否有自己的庭院
（5）是否已经有宠物
（6）房子大小：大房子-大型犬，小房子-小型犬
  - Output：probability / score / ranking
  - Models：gradient boosting / random forest + cross validation
  - Training data: 根据有倾向性的特征赋值，其他特征随机，训练模型
  - Pet features - rankings: in general these combinations would have a higher ranking to be liked ←→ 钉子户问题
  - User features - pet rankings: build connections btw user profile - ranking
  - Constantly changing: Clicked pet photo + stay time - more training data rsp rankings
Clicked pet photo + stay time ~ more training data rsp rankings

- Method 2: Content-based Recommendation
  - Generates recommendations based on users preferences and profile
  - Model: cosine similarity between two vectors

- Combined Recommendations
  - Random photos + 钉子户photos
  - Once a user has clicked a photo
  - Return most similar 50 pets using Method 2
  - Compute the predicted ratings of these 50 pets using Method 1 with more training data
  - Return 10 pets with most highest rankings

- Memory-based Collaborative Filtering Recommendation: Singular value decomposition (SVD)
  - User-based — give recommendations based on similar users
  - item-based — give recommendations based on similar items

- 非结构化数据NoSQL + MongoDB

- 本周任务：
  - DA：重点放在疏通“推荐方法”，同时尝试网络收集数据集作为training data
  - CV：imaging matching，resize/upsample/downsample等方法实现
  - 确认数据库架构（potentially跨组会议）（Natalia）
  - potentially和其他组织的技术人员经验交流（小韩）

______________________________________________________________________________________________________________________________________________________

### Date: 2022.04.09

#### Regular Weekly Meeting Time
- default开会时间：Sat 10-11am (美东) / 10-11pm (国内）/ 3-4pm（东一区）
- 备注：如果有成员来不及，可以提前商量，偶尔后延一小时

#### Team Collaboration
- Meeting Minutes - Google Doc
- Resources/Work, Task Assignment - Github
- Technical Discussion - Slack
- Logistics Discussion - AI Team Wechat Group
- Github id认人：Huiyu Chen , Haojun Cai (caicai), siyi415 (siyi), vio372925, My Wang (papa), Natalia (natalia)
- 似乎还没有加入/无法从昵称判断：橘子，乱乱，jeni，落泽成海

#### Research Update: CV - 图片压缩、美化
- （papa）需求1：像素不够高、背景杂乱等 参考：[npo提供的待优化图片示例](https://drive.google.com/drive/folders/1mMT4lDLm_rWZb-0qNGRLhOtppJb3o9oA?usp=sharing) 👉 方法：替换背景，减少人工美化时间精力成本
- （papa）需求2：品种分类，可能需要比较细的[花色分类](https://github.com/Furiends/AI/blob/0fdbdc8b632455c4933153b956c30ae063987714/imges/npo%E8%8A%B1%E8%89%B2%E5%88%86%E7%B1%BB%E9%9C%80%E6%B1%82.JPG)👉实现层面：npo可以提供多少训练样本？每一种可能需要200张图片以上

- 需求3: 图片压缩，以节省存储、传输压力，到客户端之前都是压缩状态，之后解压 👉 方法：超分，对于视频可以进行抽帧

#### Research Update: DA 
##### 1. 黑名单数据收集
- (siyi) 微博话题 #领养黑名单# 主要是组织和个人发布，发布的个人信息不一，一般是微信号截图、打码后的姓名、身份证号、电话、户籍、事由
  - 信息发布格式不一，有文字有图片，可能需要人工整理、录入信息？
  - 个人信息比较零散，如何定位/match到人？
  - 信息真实性（黑名单的下一步：直接拉黑or更仔细的领养审核）？
  - 数据比较少（3页），范围全国，较分散，需要拓展数据收集渠道
  - 可以联系发布信息比较多的组织看能不能共享名单（e.g.波奇公益）
  - 信息发布格式不一，有文字有图片，可能需要人工整理、录入信息？个人信息比较零散，如何定位/match到人？信息真实性（黑名单的下一步：直接拉黑or更仔细的领养审核）？
- (siyi) 支付宝领养失信黑名单
  - 支付宝搜索领养->点击“我要领养”小程序->首页“领养失信名单
  - 文字列表形式，主要是微信号，也有其他信息，将名单人员分为”已被确认、已在救助圈出名的虐猫者和救助者拒绝的名单“ 
- （npo）目前大概可以拿到几百条数据
- 依托第三方平台信息背书，进行合作？：芝麻信用分、支付宝实名体系，形成约束

##### 2. 智能排序&推荐
- (siyi) [recommender system to find the ideal pet](https://e-82849.medium.com/animal-adoption-how-data-science-can-be-used-to-help-animals-in-shelter-30b980db7403)
  - Use classification model to understand adopter preference
  - Feature variables: animal type(dog/cat), intake condition, animal profile(breed, color, sex, neutered/sprayed.)
  - Target Variable: adoption or euthanasia.
  - Cross validated with logistic regression, KNN, decision trees, random forest and gradient boost (XGBoost, best performance)
  - Results: people tend to adopt younger, neutered/spayed, miniature pets
  - Recommender System: rerank the pets by age afterwards
![image2](https://github.com/Furiends/AI/blob/ee933a82d4fdc6a9a775d1b86c0f056565ca1513/imges/recommender%20system%20example%20(1).png)
- (siyi) Combine application + matching process
  - Combine steps and make the process more efficient overall by having users fill out the application form upfront so that they will only be paired with good matches, reducing the likelihood they will be rejected and have to go through the whole process again. This method might feel lengthy, but we will use all the collected data to only show users pets that match their lifestyle, making the overall experience much more efficient and much less frustrating. It will also remove the necessity of filling out the same form for each pet separately.
![image3](https://github.com/Furiends/AI/blob/246a10e7b218e205d38a12b81c18eaf66ce492a9/imges/recommendation%20matching%20(2).png)
  - Example: Used by petfinder "PetFinder Quiz - find your perfect match"
- (乱乱）双塔模型(dssm模型)：使用相对独立的两个复杂网络构建用户相关特征的user embedding和item相关特征（也就是宠物相关特征）的item embedding，计算score，从高到低进行排序
- (siyi) 可以帮助“识别钉子户”的一些方法: 参考方法，可以部分参考结论（注意地域差异的影响）
  - [Increasing adoption rates at animal shelters: a two-phase approach to predict length of stay and optimal shelter allocation](https://bmcvetres.biomedcentral.com/articles/10.1186/s12917-020-02728-2#Sec3)
  - [Characterizing unsuccessful animal adoptions: age and breed predict the likelihood of return, reasons for return and post-return outcomes](https://www.nature.com/articles/s41598-021-87649-2#Sec8)

##### 3. (Jeni) Database Research
Reference: [Animal Adoption System](https://github.com/Furiends/AI/blob/a03e130dfccf37c0a236962110a471f6396a089b/resource/Animal%20Adoption%20System.pdf)
Dataflow Diagram (on page 13/14)
![image4](https://github.com/Furiends/AI/blob/e002e8b2d90090e69e9a811cbc9242d08d680553/imges/animal%20adoption%20dataflow%20diagram.png)

#### Update from other teams/TPM
- 数据库框架
- 后端开发语言（python）
- 产品形式：目前在小程序or网页中选择

#### 本周任务
- CV：确定背景分离和压缩等使用的模型，有数据支持的表现，自己试一下效果（papa，乱乱，橘子）
- 向npo拿一些图片数据（papa）
- DA: 找数据，学习智能推荐&排序相关模型（试一下具体code执行，效果）（caicai，落泽，siyi，jeni）
- 和后端数据库确定variable制定和ai组可以做的部分（jeni）
- DA: 数据分析开发平台确定（caicai，落泽，siyi，jeni）

__________________________________________________________________________________________________________________________________________________________

### Date: 2022.04.02
#### Self-intro & Ice-breaking!  
#### Discussion: 数据分析组在Furiends的作用，以及现阶段可以做些什么  
根据目前需求，AI可提供的技术：  
##### 1.  上传视频/图片：压缩视频/图片；品种识别；视频美化（宠物照片对于能否成功吸引送养人至关重要）-- CV
- 机构上架待领养宠物，提供照片
- 图片美化：几何校正，补光，朦胧滤镜
- 品种识别（npo需求确认）：训练公开[数据集](http://vision.stanford.edu/aditya86/ImageNetDogs/)
- 优先进行：图片压缩和美化功能
##### 2.  智能排序：帮助送走钉子户
- 原因：年龄、疾病、非品种狗etc  
- 如何衡量？点击率和最终确认领养比率，在平台上停留时间长  
- 其他问题：钉子户之间如何决定排序？与用户偏好智能推荐结合  
##### 3.  数据分析，模式识别，建立领养人黑名单 – DA  
- 目的：减少动物虐待、二次抛弃  
- 途径：领养人注册流程（用户数据收集，提供身份证明？作为初创平台是否有公信力），跟进领养情况（是否有具有法律效应的合同或约束？)  
- 具体方法：通过手机号进行身份认证连结（privacy issue）；要求用户填写问卷  
- 其他问题：建立黑名单后，下一步是否由我们平台负责？或者如果发生领养后虐待行为，我们可以如何约束？（目前国内没有成形法律）  
- 需要了解黑名单领养人的领养偏好等pattern  
- 可以参考银行风控模式？建立普遍规则和特殊规则，把用户分类分级，追踪用户行为和特征（reach out to npo组），建立模型，模型调试。（银行数据更高频，领养更长期，one shot）
- 汇总其他平台/机构黑名单（通过npo联系共享黑名单（实名制？），社交平台曝光，爬虫？），整合数据  
- 拉高领养成本：提供工作证明，资产证明/银行流水等，领养前线下见面筛出red flag领养人，写申请文章等  
- 建立追踪/回访流程，建立违规行为标准，根据领养人其他行为来判断是否加入黑名单：如多次逾期没有提交视频/图片反馈  
- 设定时间段和领养数量，比如半年内只能领养一只  
- 事前预防？or事后发现，纠正？  
- 和在地救助机构联动，城市志愿者  
##### 4.  智能推荐（有些人偏好某些样子的毛孩子；有些毛孩子很抢手）
- 和智能排序结合一起做
#### 现阶段工作：分小组调研
-  cv组（papa，乱乱，橘子） - 调研图片压缩、美化  
-  da组（caicai，落泽，siyi，jeni）   （1）调研收集黑名单数据 + 爬虫；（2）智能排序&推荐
-  和后端组沟通数据库框架 - Natalia、papa
-  确定固定周会时间, setup weekly meeting link - siyi
#### 后续准备
-  开发框架（pytorch）开发语言（python）
-  AWS学习 ，docker学习

