# 监控系统

## 1.常见目标



## 2.常见的监控维度

## 3.监控常见的流程和方法

## 4.常用监控指标以及告警设计时的诉求

## 为什么要引入监控系统

随着互联网的发展，系统越来越复杂，需求变更越来越快，应用压力越来越大，服务压力越来越大，不得不引入监控系统

1.系统可视化
利用监控系统可以东西当前系统运行情况，如内存，cpu，磁盘占用率，接口成功率

2.科学管理

3.复杂问题定位
微服务系统中包含多个系统中的系统调用，如A->B->C,如果没有监控系统不得不去3个系统所在服务器，分析日志，是业务异常还是网络问题，而借助监控系统，如日志监控工具，可在统一的地方分析所有微服务日志，可以快速找到出问题日志，利用调用链监控工具可以快速分析出哪个调用位置出现问题调用，从而让问题排查有据可依
4.可以提前预防问题

监控系统展示io，硬盘cpu，占用情况，如果监控内存持续满负荷可以提前扩容，防止性能瓶颈的产生

5.快速介入问题
可以快速配置告警规则，如配置接口错误率达到多少，推送告警给对应人员

6.节省成本
没有监控系统时代，需要值班轮班，总得需要人员看有没有问题，而有了监控系统后可以实现24小时无人值守

7.形象提升
提升企业可以科技感


简单说，利用监控系统有助于了解系统情况，从而提前预测，快速发现问题，快速定位有据可依



## 监控系统所具备能力或者说是监控系统目标

1.丰富监控指标

2.灵活数据聚合与分析
需要对监控系统进行收集到的原始数据，分析计算汇总，得到想要的数据，比如cpu,qps

3.具备功能强劲，人性化的dashboard

4.灵活异常检测能力
能够通过配置参数，如cpu达到多少检查存在问题，

5.丰富的告警模板

检测到异常后，推送告警，可以设置灵活的模板，针对不同人发送不同模板
对于某个错误，不同的人可以有不同的模板

6.丰富告警渠道
飞书，短信

7.监控系统的可扩展，高可用

8.故障复盘

没必要专门一个模板来做，但方式比较灵活

## 监控维度

1.微服务实例维度
占用cpu,内存，磁盘，QPS，WEB容器，线程池，数据库连接池，线程状态与占用CPU时间

2.容器实例维度
内存，cpu，磁盘，网络，端口占用

3.JVM维度
cpu内存，GC,吞吐量，堆栈，线程栈

4.操作系统维度
CPU，内存，网络，磁盘，工作负载，端口占用

5.中间件维度
监控中间件使用情况（不同中间件不同）

6.系统整体监控

日志，调用链路。。。

不同维度监控工具不一样

总之让监控粒度丰富细致，监控一切

## 监控流程和方法

监控系统流程

jiankongliucheng.jpg

监控系统针对需要监控的指标数据收集，之后数据分析，预测可能存在的系统风险，分析内存占用，预测可能存在的瓶颈，让项目提前扩容，经过数据分析后，发现系统指标超出正常阈值，推送故障告警，相关人员问题解决，故障解决完后，监控系统还可以发挥预热进行系统复盘，为复盘提供数据支撑，如在某某时刻发现流量不正常，平时多少，现在多少，什么时候解决问题了

监控系统常用方法

1、嵌入式监控

qianrushijiankong.png

监控功能和应用集成在一起，比如javaMeloy,Alibaba Druid连接池

好处：架构简单，不需要考虑监控系统与应用之间的通信，同时也不需要引入其他组件
缺点：只能监控单个进程自身，集群维度的监控无能为力

因此比较适合单个微服务实例监控，不太适合集群维度的监控

2.分离式监控

fenlishijiankong.png
监控功能和应用是分离的，这种情况下监控系统会采用客户端，服务端架构，客户端与服务器通信共同构建监控系统，客户端负责采集数据，服务端负责，分析，计算，汇总数据

分离式监控有分为2种模式

第一种模式，主动上报（push模式）

pushmoshi.png

在被监控的应用中集成客户端，客户端产生业务数据，上报给服务端

典型实现：skywalking(通过java agent插件产生监控数据，自动上报到service端，分析展示数据)

好处：服务端压力小一些，因为客户端可以聚合后推送给服务器
服务器端无需知道客户端具体地址
缺点：客户端侧定期上报，上报频率一般由客户端决定，难以在服务端统一控制
客户端必须知道服务端地址，一旦服务端地址变化，每个应用要修改，可以通过域名解决


第二种被动采集（pull模式）

一般是客户端提供api，服务器端拉取数据，汇总展示

典型实现：Prometheus，ali的sentinel+dashboard

好处：服务端可以方便 控制采集频率，客户端无需感知服务端地址变化

缺点：服务端压力大一些

客户端侧设计1（客户端嵌入应用中）

监控客户端嵌入应用中，比如sleuth +zipkin,sleuth集成在应用中负责产生监控数据，上报到
zipkin server

客户端侧设计2（客户端为java agent）

典型实现skywalking，和客户端侧设计1没什么本质区别都会部署在一个进程中，但javaagent会降低代码侵入性，比如skywalking不需要修改代码，加一个配置就行

客户端侧设计3（客户端与应用进程分离）

客户端一个进程，应用另一个进程，
典型的：各种serviceMesh，比如istio，linerd等，监控系统客户端以sidecard容器形式，和应用部署在同一个pod中，但是属于不同容器，不同进程

优点：应用进程只需要关注自身业务，和监控完全解耦，职责单一，是可插拔的，不想使用的话，把进程干掉就行
缺点：性能损耗


## 监控指标和告警策略

四类告警指标
	google的四个黄金指标，百分比指标，系统指标，业务指标

告警五大诉求

### google的四个黄金指标

google的四个黄金指标是谷歌针对分布式系统的经验总结，通过4个指标在服务级别，帮助衡量终端用户的使用体验，业务影响，服务质量问题，

这四个指标是延迟，通讯量，错误，饱和度

延迟：服务请求所需要时间，通过延迟到，可以分析应用性能以及用户使用体验

通讯量：系统当前的流量，通过流量层面监控可以衡量服务的容量需求，流量是广义概念对于微服务来说，流量指的是每秒http请求数，对于redis来说是指每秒请求redis次数，对于操作系统来说，是每秒带宽数据

错误：系统发送错误请求的指标，通过错误可以帮助我们排查异常情况

饱和度：检测最能影响服务状态的，受限制资源，比如一个io密集应用性能，如果受内存影响最大，应该关注内存占用情况，受磁盘影响最大，更加关注磁盘io，当资源占用达到饱和后服务性能明显下降，还可以通过饱和度对系统进行预测，比方预测磁盘3天后就要满了，可以提前扩容

### 百分比指标
百分比指标：
在数据统计时，往往引入百分比指标，百分位是统计学概念，比如skywalking会将数据从小到达排序，比如90百分位是1.2秒，标识90%的响应时间都是低于1.2s的，除此外，百分位还有一些通用表示

median表示中值，也叫中分位，用来表示一组数值中居于中间的数值，相当于p50，简写成me

Mean:算法平均值，把一组序列所有值加起来，除以个数

四分位数：

p25表示第一个四分位数，简称Q1
p50表示第二个四分位数，简称Q2
p75表示第三个四分位数，简称Q3

### 系统指标

响应时间（RT）,响应时间指系统对请求作出的响应时间

RPS(request per sercond),指系统在单位时间内处理请求的数量，一般用该指标作为系统的吞吐量指标

TPS(transaction per second):每秒处理事务的数量，有一些监控系统把TPS作为吞吐量的指标
，但更多系统用RPS衡量吞吐量

QPS（query per second）:每秒查询速率

并发用户数：系统同时承载使用系统功能的用户数量

这些指标之间存在数学关系
可以参考：https://help.aliyun.com/document_detail/29343.html


## 业务指标

不同的业务指标是不一样的，比如crm系统
新增用户，活跃用户，留存用户，留存率，pv，uv


订单中心：成绩总额，成交数量，客单价，付费率，复购率，GMV

需要产品协助指定指标

## 告警5种诉求

丰富告警渠道

丰富告警模板


高度可配置性(推送不同的人)

告警分级（可以按照问题严重程度，紧急程度）
比如不严重可以邮件，严重告警打电话，发短信

易用性 

告警规则简单，内容易懂，重复内容自动精简


## 一个完整的监控系统

一个完整的监控系统应该有6个维度的监控：微服务实例，容器实例，jvm
操作系统，中间件，系统整体


如果每个维度各自为政，会导致一些问题  

1.体验差
2.监控数据整合，分析，聚合很不方便
3.学习成本高，需要掌握各种工具，因此一般企业会使用一种监控工具，把各个维度监控串起来，这里采用Prometheus，所有数据会汇聚到这一个监控系统上就不会存在上面问题



Prometheus

官方地址：prometheus.io
官方文档：prometheus.io/docs/introduction/overview
架构图

这个架构图看起来可能比较多，那我们分块看一下
中间一块是Prometheus server端是Prometheus，负责数据采集存储，以及提供api查询数据，
retrieval 表示检索，tsdb表示时序数据库，http service表示提供http接口，方便其他组件的交互，左下角prometheus用来采集监控指标，
并把监控指标暴露出来， server端会从各种exporter里面获取监控数据，这种获取是pull的方式，左边push gateway有2个作用，1是有些应用不是长期运行的而是运行完就停止了，如果也相对这种应用进行监控就可以对这种应用进行pushgaway，让短期运行的应用推送到pushgateway上，然后service端拉取，pushgateway上的监控数据，第2个作用，如果exporter与server端不在一个局域网，或者防火墙原因没有办法直接拉取exporter上的监控数据，这个时候可以让各个exporter把数据汇总到pushgateway上去

只要pushgeteway和service通信就行了

上面的service discovery用于服务发现，来帮助service端找到其他组件，比如找到pushgateway，找到exporter.
Prometheus支持多种服务发现方式，比如k8s的服务发现机制，基于文件的服务发现方式

右上角alertmanager用来实现告警通知，alertmanager支持多种告警渠道

右下角是Prometheus可视化，可以使用webUI，grafana来实现数据可视化

从上面可以发现Prometheus组件虽然比较多，但是职责还是比较清晰的，而且协作起来不复杂。


### 微服务实例维度核心指标，监控工具对比与选择

核心指标：

资源占用相关：内存,cpu

健康度监控：微服务实例依赖资源是否健康（数据库缓存，磁盘）

web容器层面的监控信息：tomcat运行状态，请求响应情况

各种性能指标：线程池，连接池，qps

### 监控工具对比

weifuwushilijiankongduibi.png
weifuwushilijiankongduibi.png

优先spring boot actuator ,次选javamelody

实现微服务实例监控，常用的端点有

springbootactaor.png

spring boot actactor内置了很多的监控检查，比如磁盘，等，如果项目种使用了某些资源，比如zookeeper，但是spring boot actactor 自身没有实现对zookeeper健康检查，这个时候可以自定义

prometheus端点会把metrics端点监控信息，以Prometheus格式进行展示

# HELP表示这个指标的含义是什么， #TYPE表示指标类型是什么，一整行表示一个指标
大括号前面表示指标名称，大括号里面是指标的lable也就是指标标签，如果为空表示没有标签，最后数字是指标的指

现在微服务已经可以产生监控数据，并且监控数据暴露成了Prometheus需要的格式，service端如何获取监控数据呢，需要修改Prometheus.yml配置文件，然后找到scrape_configs:
添加一个job，名称随便写， scrape_interval：配置拉取时间 scrape_timeOut拉取数据超时时间
metrics_path：拉取地址

targets：设置微服务地址

fuwufaxianprometheus.png

组件中记录微服务地址

怎么理解呢，就是这个指标被打了2个标签，一个instance，一个job，筛选时，大括号加上值，表示的意思就是这个指标，带有某个标签的指标

这种语法叫promql

可以查询所有实例instance指标的值，可以额外打标签



## JVM维度核心指标和监控工具对比

资源占用相关指标：

内存相关：年轻代，来年代，元空间，代码缓存区

线程相关：线程数据，线程占用cpu，线程状态

网络占用


垃圾收集相关指标：

垃圾收集频率，垃圾收集次数，收集前后的内存变化

性能相关指标：

类加载，卸载，动态编译

对比选择：jconsole,jvisualvm,jmc,jmap,jcmd,jstat

git.imooc.com/class-109/resource

虽然这些工具从监控能力上看比较强大，而且排查问题时，这些工具也是我们的好帮手，但是从使用体验上说，并不是很好，我们更加希望所有的监控数据汇总到同一个监控系统上去，降低学习成本和维护成本，而监控系统选择Prometheus，那么如何选择与Prometheus整合呢

可以选择 Prometheus jmx_exporter&& spring boot actuator

jmx_exporter 是exporter之一用来产生监控信息的

jmx exporter有2种方式 javaagent，另外一种http server ,但是http server方式有一些缺点，例如配置，公开指标困难，所以官方建议javaagent方式运行


在jvm应用上集成jmx exporter 爆露出/metrics, exporter以javaagent方式运行


## 操作系统核心维度指标，监控工具对比与选择

操作系统是一切的基石，操作系统的监控是非常重要的

CPU相关指标：cpu使用率，cpu队列长度（当cpu忙不过来时 会产生任务队列，当队列越长时，越忙）

内存相关指标：内存总大小，使用的大小剩余大小，虚拟内存大小，页交换频率（内存与虚拟内存数据交换频率，频率越低越好）

磁盘相关指标：磁盘总大小，使用的磁盘大小，剩余磁盘大小，磁盘使用率（磁盘处于读写状态占总时间的比例，越大则越忙），磁盘队列长度（要保持在个位数）

网络相关：网络带宽消耗（网卡每秒接受数据量，每秒传出数据量，如果其中一个和网络带宽相当说明达到性能瓶颈，这个时候要么进行扩容，要么将流量降下来），丢包率，错误率

总的说分为2大类一种计算资源，cpu,另一种io资源,内存，磁盘，网络

比对与选择

caozuoxitong.png

不论使用那种工具都能达到监控模板，另外node_exporter只支持linux，unix系统


caozuoxitongjiankong2.png


## 中间件监控

消息中间件

存储：mysql redis

分库分表：


严格说这些是基础设施

这些中间件也可能出现问题，需要提前预测问题，发现问题，解决问题，

因为中间件太多了，每个中间件都有自己的监控工具，甚至不止一款，有关中间件维度怎么监控
zhongjianjianjiankong.png

因为prometheus对容器进行监控时，需要涉及到完整监控体系，目前来说探讨了exporter与service,以及基于eurake的服务发现，像pushgateway和alertmanager，grafana没有探讨，
这里过一下

## PromQL

promQL是Prometheus内置的查询语音，可针对prometheus数据做查询计算，汇总等

是一种专门查询Prometheus语言，像alertmanager种的告警规则，以及可视化，都需要用到promQL

几个重要概念

瞬时向量（Instant vector）：表示某个瞬间数据，比如cpu

区间向量（Range vector）：表示一段时间数据，一般是多个瞬时向量的组合，比如5分钟以内的cpu占用率,是多个瞬时向量组成的集合

纯量（Scalar）:纯量只有一个数字，一般是分析汇总后的结果，比如count做计数，统计几条，只是一个数字

语法1：

指标名称：system_cpu_usage,这样查询的是最新数据，返回的是瞬时向量，也叫瞬时向量表达式

语法2：

指标名称{标签名称 操作符 标签值}

语法1：相当于system_cpu_usage{}

在大括号里面还可以继续筛选

操作符有=(等于)，!=(不等于)，=~(正则匹配)，!~(不正则匹配)

http_requests_total{environment=~"staging|testing|development"}
筛选出staging或testing或development指标

大括号里写多个标签，是同时具备多个标签的指标

上面都是最新数据的瞬时向量，很多时候都是查询一段时间的数据，这个时候

语法3：
指标名称{标签名称 操作符 标签值}[区间]

比如system_cpu_usage{instance="localhost:8080"}[5m] ,查询出标签instance="localhost:8080"的名为system_cpu_usage指标的最近5分钟数

支持的单位s,m,h,d,w,y

语法4：
如果想要查询过去某个段时间的数据
指标名称{标签名称 操作符 标签值}offset 时长

system_cpu_usage{instance="localhost:8080"}offset 1m

表示查询1分钟之前这个时间瞬时值，这个是瞬时向量 

system_cpu_usage{instance="localhost:8080"}[区间]offset 1m

表示指标在1天之前5分钟区间数据


语法---算法运算符


支持：+,-,*,/,%,^


system_cpu_usage*2
表示乘以2


比较运算符：==，!=,>,<,>=,<=

实例：system_cpu_usage > 0.00001 才返回


逻辑运算符：and or unless 

例子：system_cpu_usage > 0.00001 or system_cpu_usage<1.0 才返回

聚合算法符：sum,min，max,avg,stddev,stdvar,count ,count_values,bottomk,topk，quantile

max(system_cpu_usage) 表示取得这个指标最大值

函数：支持abs(),absent(),ceil（）三十多个函数


rate(system_cpu_usage[5m]),最近5分钟数据计算平均每秒值是什么


Prometheus.io/docs/prometheus/latest/querying/functions/
函数特别多，用到时找到文档查一下

## 基于alertmanager Prometheus


prometheus本身提供告警能力，可以提供告警规则


多个告警规则可以形成一个告警组

groups:
   - name:example //告警组名称
     rules:  //告警规则
        - alert:SystemCpuTooHigh
          expr: system_cpu_usage > 0.0001//基于promql计算是否有时间序列满足告警条件
          for: 10s //评估等待时间（可选参数），表示当expr条件持续为true一段时间后，才触发告警
          albels://给告警打的标签
             imooc: good
          annotations: //用来指定一组附加信息，比如描述告警的详细文字，内容也会发给alertmanager,以key:value形式，支持模板
             summary:cpu占用过高：{{$labels.instances}}
             description:"实例{{$labels.instances}}的系统cpu占用过高，并持续10s, cpu占用是{{$value}}"


 告警状态：inactive,pending,firing

 一开始规则还没有加载，表达式计算出来是false,这个时候是inactive,经过计算，表达式true,状态切换pending,表达式持续true,持续10s ,触发告警变成firing

 prometheus告警能力太弱了，要想满足生产环境的诉求需要使用alertmanager


 prometheus端定义告警规则，server进行计算，如果满足告警规则触发条件，就向alertmanager发送告警信息，收到告警后进一步处理，比如推送告警

## grafana可视化
grafana是一款开源的可视化系统，可实现prometheus,elasticsearch,等数据源可视化

grafana架构：比较简单，从数据源读取数据产生图表


有一些dashboard市场可用选择

12856



grafana告警
体验更好，grafana告警并不依赖alertmanager

grafanagaojing.png


alert evaluate every :每隔多久评估告警规则 for是持续多久condition后推送告警


## 容器实例如何监控

其实也是k8s监控，手动搭建的化非常痛苦，这意味着编写n多k8s脚本，但是提供了prometheus operator工具，这个是对Prometheus及相关组件发布包装

prometheusoperatorjiagou.png










































