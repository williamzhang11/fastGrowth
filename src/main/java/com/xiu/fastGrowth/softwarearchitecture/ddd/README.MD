# 领域驱动设计（DDD）

了解领域驱动设计的应用场景

掌握通用语言与界限上下文

掌握领域建模常用设计模式和方法论

熟悉领域建模核心概念

了解从需求分析到微服务拆分落地全过程

掌握微服务拆分工具--事件风暴

熟悉蛛网模型

从实战理解微服务交互接口，代码结构和分层模型


what:DDD概念，模型要素，分层结构，方法论

why:技术高度分水岭（面试资深架构师，了解知识面，管控过多么复杂的系统设计，当系统越复杂时，理论越有效，），产品为导向的架构发展趋势（互联网时代，大量的产品经理，领域专家，和it系统架构紧密配合）

how:理论类比实战，案例解析（项目管理，电商）

## 领域驱动基本概念

为什么DDD很火?因为它解决了架构师的通用问题，Do the right thing right!也就是完成业务和客户的需求，要把事情做漂亮，分而治之，把每一小块实现了，拼接起来就是一个大的系统，总之两点：业务需求和分而治之，领域驱动设计就是以客户和产品为导向，进行业务拆分的架构设计思路。


有时候和产品经理沟通时，总感觉乖乖的，
用同一种语言语言说业务需求，通用语言（ubiquitous language）,因为技术人员沟通时，是从技术角度，有一些专用术语，如电商平台的用户，不同系统概念是不一样的。怎么界定通用语言呢，把大的领域进行切分，切到合适大小，每个大小都有独有的含义，含义呢，不管是it人员还是架构师还是客户经理都能认可，这个时候的语言记录下来就是唯一的通用语言

领域设计4层模型

展现和接口层（api或图形化界面），应用层（命令，编排），领域层，基础架构层（cache,persistence,message,queue,mail,log）基础架构层也可以通过控制反转往上放，领域这层是核心，而应用层调用领域层，有应用服务也可能有领域服务。没法抽象比较容易理解的领域模型，可以放在应用层处理，最外面是controller层


DDD架构亮点，在什么场景下用的最多，下面是鲁棒图的3个小图标，左边和用户相关，中间表示和逻辑相关，右边表示和数据相关，和用户相关设计方法叫smart ui非常聪明方法，比较适用移动端app，很多移动端app厂商，以移动端用户体验，一个个控件主导开发的，控制后台服务器，看如何支撑空间和小模块的，这种设计思路叫以前端为主导的设计思路。中间的以逻辑为主导的设计思路也就是ddd,以逻辑抽象为主题，不是很关心数据存储，用户界面展现，把逻辑分成一个个小单元，通过逻辑驱动页面展现。右边以后台数据为主导的思路，也就是uml，都在考虑处理什么样的数据，数据在传输过程种如何处理，而领域数据抛弃数据，抛弃界面，因此领域驱动设计适合1.产品导向（整个产品的业务逻辑，如设计宠物管理系统，洗澡，吃饭，监控宠物）2.适合中大型项目（项目很大，大到2-3个人没法在2-3个月内完成，需要拆分，不能随便让一个架构师从无到有拆分，因为太复杂，需要借鉴已有的思路，找一些相关领域的思路，这些领域默认拆分成一些微服务或子域，参考这些服务，进行核心域的开发，同时很多支撑域，通用域来借鉴）3.api经济，中台化，很多内容通过阿里云平台，都已经包装好了，如商品中心，重点关注核心子域，通过这种方式简化工作量，减少研发周期。

DDD什么时候用的最多？

1.升级汇报时，把原来的微服务拆分用领域驱动设计简单包装下，采用了领域驱动设计的XXX概念，怎么样进行事件拆分，怎么样进行架构分层
2.高光时刻，技术大赛，pk下谁的技术更高，5-10分钟聊一下架构设计一个新的功能，这个功能如何改变企业的平台，什么样的效果，什么样的方法，如ddd开发，怎么样拆分的，另外还有大型社区演讲机会，把自己做的项目用ddd包装一下，显得高大上
3.面试后2轮的讲故事环境，前几轮一般pk中间件，java基本功能，并发功能后两轮碰到研发总监，首席架构师，这个时候他们都会问ddd理论，项目经验，这个时候需要把故事准备好，包装好，对ddd了解足够深刻
4.日常厚积薄发，DDD不需要完整用光，如果是小型项目可以剪裁一些DDD理念，如事务处理理念，消息机制的理念，读写分离的理念，以产品为导向的理念，事件风暴进行微服务拆分理念，只有厚积才能薄发 

## 核心概念：领域与子域和界限上下文

领域：赚钱的机会，二手车商品交易平台，宠物托管平台

子域：支付子域，产品子域，用户认证子域
由领域拆分出子域

界限上下文：支付上下文，认证上下文

子域提出一个问题，如支付能力，需要搭建一个支付平台，支付会与产品和用户认证打交道，这样与其他打交道的功能合在一起就是支付上下文，界限上下文解决问题

核心域（产品上下文，订单上下文，物流上下文） 

支撑域（用户推荐，用户违规分析（提供给核心域信息，禁止买东西），）

通用子域（用户认证（可以对接，淘宝登录，微信登录）,一是不为我们赚钱，二是支撑粒度没有像其他用户行为分析，对业务影响那么大，可以用简单方法实现）

有子域，可以用界限上下文落地 

hexinyuziyutongyongyu.JPG

## 界限上下文

界限上下文有沟通的，边界之间有关系的，如屈从，合作伙伴，共享内核

界限是围着的东西，

上游提供服务（upstream），下游接受服务（downstream）
jiexianshangxiawen.png

边界之间有关系的：

1.分离模式：互不干扰，各走各道，互相竞争，没有太多关系

2.反腐层模式：你的服务过来，我不太新人要处理下，适合老旧系统沟通，把老旧系统处理下，（采用open hostservice/published language,如rpc,消息队列）集成遗留系统，又不强制老系统更新，新系统要做个适配器层来转换模型

3.跟随者模式：跟某个上游系统绑定，但是其他系统是主，本系统是从

4.客户供应者模式： 客户供应商关系，强依赖，上下游系统开发合作顺利，对模型逻辑修改反应迅速。如消息队列2端

5.共享内核模式：两团队之间紧密合作，代码模型可提取通用组件共享

6.合并上下文模式：两个界限上下文，两个团队如一家人，紧密到无法分开，一切模型要素都是共享的

## 案例分析--敏捷项目管理系统

子域划分：

身份与访问上下文（）

敏捷项目管理上下文（ACL）--协作上下文（ACL）

xiangmuminjiedemo.png

## 领域模型的要素-实体，值对象，聚合

实体（Entity）:

唯一标识（用户提供，应用生成uuid/guid,持久化生成）,学生的学号

数据可变：

生命周期管理：订单状态管理（待支付，已支付）

唯一标识是实体与实体沟通的，聚合与聚合之间的纽带

值对象：

无标识

数据不可变，可复制，可替代

整体性（100元，浙江杭州西湖区）

通常用来做度量和描述

聚合aggregate--班长:

事务一致性边界

聚合之间通过聚合根（通常为聚合同名实体）进行沟通

尽量缩小聚合（最终一致性和领域消息）

聚合是创建，发布领域事件和操作资源库的操作主体

shitizhiduixjuhe.png


编码：

任务项是实体，有不同的值对象

//聚合根
public class BacklogItem extend Entity{

	priavate BacklogItemId backlogItemId;//唯一标识，可用uuid生成

	private BusinessPriority businessPriority;//值对象

	private Set<Task> tasks;//普通实体
	

	第一种创建
    public void defineTask(...){
    	Task task = new Task(...);

    	this.tasks.add(task);

    	//发布领域事件
    	DomainEventPublisher.instance().publish();
    }

}

第二种创建
public class Product{
	
	BackLogItem planBackLogItem(...){
	BackLogItem backlogItem = new BackLogItem();

	DomainEventPublisher.instance().publish();
	}

}

public class BacklogItemId extend ValueObject{
	
	private String id;

	public BacklogItemId(String anId){
	this();
	this.setId(anId);
}
}

public class BusinessPriority extend ValueObject{
	
	private BusinessPriorityRatings ratings;

	public BusinessPriority(String anId){
	this();
	this.setId(anId);
}
}




领域驱动中对外提供的永远是聚合根的功能，聚合根里面的实体对聚合根提供服务，其他的普通实体访问set用protected

或者直接是一个普通类，不用封装Entity,类里面有唯一标识id,内部成员变量private,set getprivate,鲜明的名词public,描述性的内容用值对象封装，不用让聚合根有生命周期管理，全部用快速创建，快速复制，永不修改实现值对象描述信息，一个对象下有多少子对象呢，子对象用实体封装好（成员变量private,set、getprivate 操作protected,）把聚合跟打包在一起是聚合

## 如何构建领域模型的工厂，库，领域服务


工厂factory--班主任：

创建聚合和实体

工厂类，工厂方法，创建者模式

对象解耦

封装复杂操作过程

资源库repository--辅导员:

聚合的增删改查

对接持久化机制（hibernate）

和dao,mapper类似，但更侧重集合的处理

封装复杂操作过程


领域服务domainservice---教官

无状态

无法归为实体，值对象

业务操作过程，对领域对象进行转换

如：密码加密服务，文字转换服务，应收账款，和应付账款对账

## 领域服务与防腐层

fangfuceng.png

public class DiscussionApplicationService{
	
	pirvate CollaboratorService collaboratorService;

	public void postToDiscussion(,,,){

		collaboratorService.authorFrom();

	}
}

port.adapter包下（领域服务是抽象接口，真正的实现基础架构层）
public class TranslatingCollaboratorService implements CollaboratorService{
	
	public Author authorFrom(){

		//适配器(既能获取信息又能转换)：

		this.userInRoleAdapter().toCollaborator(); 
	}
}

public HttpUserInRoleAdapter implent UserInRoleAdapter{
	
	publiv <T extends Collaborator> T toCollaborator(){
		//http请求


	}
}

防腐层推荐，领域服务进行适配管理，用restapi，进行沟通，另一种消息队列沟通

public class ProductApplicationService{
	
	public void initiateDiscussion(){

		//repository

		//工厂

		//repository
	}

}

## 隐形概念显性化

隐形的概念显性化（多种逻辑封装在一起）

隐形的上下文显性化（）

封装多对象行为

用值对象化---（属性和约束）

规则选择：策略模式
规则拼装：组合模式

如：

//值对象，属性和约束specification
public class BusinessPriorityRatings extends ValueObject{
	
	private int benefit;
	private int cost;
	private int penalty;
	private int risk;
}


## 建模方法论

五大方法论：

企业架构法（CBM组件化业务模型）

面向服务架构设计（面向服务的SOA架构模型）

用例分析法（uml,软件设计模型）

四色建模法：

谁，哪里：party、place，thing对象（红）

什么身份：角色对象

如何的：描述对象

做什么：时标型对象

红绿黄蓝，简单却不完善

事件风暴方法论：

真的要这样拆分吗，落地问题多多

演进式架构和迭代式开发

微服务拆分和向前兼容

如何和产品业务人员沟通

有没有一个可以在和和气气氛围中聊

也叫嘴炮贴纸法：因为不画一张图，没有visio，world和ppt，准备一打便利贴，全程嘴炮和贴纸游戏，完成微服务模块拆分。

微服务拆分：然后微服务间关联关系（调用还是什么）
shijianfengbaoweifuwchaifen.png

shijianfengbaoweifuguanlian.png


## 领域模型的选择--贫血模型，充血模型

血指的是业务逻辑

贫血：getter、setter方法，crud工程师，没有太多业务逻辑，众多业务逻辑在上层，getter，setter模型也是public的

充血模型：说人话，应用逻辑为主，数据管理为辅，更符合领域语言，setter,getter是私有的，用户调用动名词方法。

充血模型，贫血模型哪个好？

没有银弹，各有优劣，贫血模型：上手快，发展受限，适合应用逻辑简单，信息记录类型的记录，充血模型：上限高，容易三高，适合逻辑复杂，信息处理类型的业务

## DDD的整洁架构之道--洋葱圈模型

洋葱圈模型和四层模型一一对应的 

yangcongquanmox.png

## 领域的延展-领域事件

领域事件之间通过异步通信，或事情驱动风格，也可以使用消息队列削峰填谷。

lingyushijian.png



创建领域事情--发布--事件发布器--订阅--传给（另一个聚合根）


创建领域事情--发布--事件发布器--订阅--传给（另一个聚合根）


lingyushijianfabu.png


public BackLogItemCommitted implements DomainEvent{
	

}

## 领域事件与CQRS

Command query responsibility separation

command:执行行动，返回void

query:只查询，不修改对象

风格：事件驱动系统风格，管道过滤器风格适合cqrs

cqrs和es事件溯源：

cqrs和es事件溯源.png

## DDD模型下的代码分层模型

一个ddd项目分为3个子域，iddd_agilepm(敏捷),iddd_collaboration（合作）,iddd_identityaccess(认证)其中iddd_agilepm(敏捷)：

是有4层结构组成：application(应用层)，domain.model(领域层)，port.adapter(基础架构层)，少了控制器对外提供api的（controller）其中port.adapter有message(mq)与persistence（存储），其中domain.model(不同的模块/模型 sprint，)。一个企业中可能10个子系统，每个子系统下可能10个module，每个module下可能1个聚合，而是发布100个微服务，而不是1000个，微服务之间可以选择restapi或mq

### 电商场景领域模型设计


c2c电商平台

c2cdianshangpingtai.png

c2cshang.png

c2czhong.png

c2cxia.png






## 如何理解领域驱动设计中的域，子域，边界上下文，微服务，聚合，实体，值对象？

题眼：考察DDD关键概念定义

加分项：能通过类比或者实际案例，分析不同概念的差异和作用，如何实现各领域模型要素的管理


## 详细讲述一个曾经使用过的领域建模方法论

题眼：用例分析法，四色建模法，事件风暴法

加分项：能结合实际，讲述微服务拆分，服务关系定义和领域建模具体过程，以及采用这个建模方法的原因优势和难点

##　讲一下ＣＱＲＳ和事件溯源的架构特点和实现方式
题眼：领域事件，事件存储，消息队列，读写分离

加分项：能结合实力，讲述领域事件的管理，消息队列的使用，cqrs的价值，代价和使用场景

## 分享一个曾经参与过的领域设计，微服务拆分，服务依赖梳理，模块接口设计的例子，如果重用ddd改造，你会如何优化架构和代码结构