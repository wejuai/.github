# wejuai
带有隐私性的贴吧或者论坛项目

[站酷上UI级项目介绍连接](https://www.zcool.com.cn/work/ZNTM2MDgwNDg=.html)

源网址为 [wejuai.com](https://www.wejuai.com/), 小程序为“为聚爱”，现在停止运营开源代码
### 主要功能介绍
1. 子站为不同的爱好分类，可以创建文章和悬赏
2. 所有的站内金额为付费购买的积分，可以充值提现，但是提现会扣除7%手续费
3. 文章可以添加标题，内容（内容内可以加图片），封面图。可以设置为付费阅读文章，额外可以设置付费前可阅读内容和付费后阅读内容
4. 悬赏是发布需求，设置给予多少金额（先付费），对于需求的描述，悬赏的功能细节较多：
   1. 悬赏发布者可以选定其他用户提交的内容
   2. 其他用户提交悬赏的内容，同样也有选定前可阅读和选定后可阅读
   3. 发布悬赏者可以选取其中一个提交作为最佳答案，积分给予提交者，发布者可以看到选定后内容
   4. 如果全是垃圾提交，发布者可以申请申诉全额退积分
   5. 如果有合适内容，发布者拖住不选，等到3个月的过期时间后会将他提交的积分平分给所有提交过的用户
   6. 发布者有一次延长一个月过期时间的机会
5. 一个可以滑动探索的“爱好宇宙”，每个注册的用户都有一个星球，爱好也有一个星球，这些星球可以根据积分变化大小

### 主要技术介绍
- 主要语言：java
- 框架：spring生态
- 构建工具：gradle(7.5)
- 数据：mysql，mongo，redis
- 部署：k8s，或者docker compose
- 内部通讯：spring restTemplate
- 文件存储：aliyun oss

### 项目组件和结构（更加详细信息查看对应项目的readme）
- wejuai-core：业务功能以及数据核心，包括子站功能以及订单的写操作
- wejuai-accounts: 账号系统和有状态数据服务
- wejuai-app：无状态公共数据服务
- wejuai-entity：通用数据实体，在不同项目间使用的数据实体，包括关系型数据以及非关系型数据，并且构建jar包存储在GitHub的库中
- wejuai-dto：通用数据传输对象，在不同项目间使用的数据传输对象，也同样构建为jar存储在GitHub库中
- wejuai-config-server：配置服务，spring cloud框架中的配置服务，为其他项目提供不同profile的配置参数
- wejuai-trade：内部包含gateway和sync两部分，分别负责组装参数发送对第三方支付系统的参数和同步支付状态的功能
- wejuai-instant-message：im服务，为用户提供聊天服务，以及系统消息总线
- wejuai-weixin-server：微信相关服务，包括收发两部分以及微信token 的定时更新，收可以接收公众号通知事件定制处理方式，发可以发送公众号消息以及小程序的模版消息
- wejuai-timed-task：定时任务服务
- wejuai-console：系统管理后台

![QQ截图20221009165354](https://user-images.githubusercontent.com/18435332/194747476-4a8731cb-709d-42ba-bd30-ced57adef4ca.png)

### 项目详细结构
- config: 配置参数和需要初始化数据库的内容以及基础框架配置
- repository: 连接数据库的jpa配置，包括mongo和mysql
- service：业务功能
  - dto：在service层面使用的数据传输对象
- support：第三方服务支持以及使用方式
- web|controller：对外开放的接口
- dto：接口层使用的数据传输对象

##### note
1. 原来是部署在aliyun的k8s托管集群，宕机检测、自动重启、可用性、扩展性都依赖于此，所以没有使用全套spring cloud
2. 最初的版本还有网页版本的，但是pc和手机网页便携性低和开发周期长所以放弃了
3. 本来仓库使用的gitlab，所以还有gitlab的runner配置文件，可以自动build docker镜像并且push到aliyun的仓库直接在k8s中使用
4. 目前在一步步补全开源代码以及文档
