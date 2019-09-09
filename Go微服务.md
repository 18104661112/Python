1.什么是微服务
1. 什么是微服务

      ```
      		在介绍微服务时，首先得先理解什么是微服务，顾名思义，微服务得从两个方面去理解，什么是"微"、什么是"服务"， 微 狭义来讲就是体积小、著名的"2 pizza 团队"很好的诠释了这一解释（2 pizza 团队最早是亚马逊 CEO Bezos提出来的，意思是说单个服务的设计，所有参与人从设计、开发、测试、运维所有人加起来 只需要2个披萨就够了 ）。 而所谓服务，一定要区别于系统，服务一个或者一组相对较小且独立的功能单元，是用户可以感知最小功能集。
      ```

      

2. 微服务由来

    		微服务最早由Martin Fowler与James Lewis于2014年共同提出，微服务架构风格是一种使用一套小服务来开发单个应用的方式途径，每个服务运行在自己的进程中，并使用轻量级机制通信，通常是HTTP API，这些服务基于业务能力构建，并能够通过自动化部署机制来独立部署，这些服务使用不同的编程语言实现，以及不同数据存储技术，并保持最低限度的集中式管理。

3. 为什么需要微服务？

        	在传统的IT行业软件大多都是各种独立系统的堆砌，这些系统的问题总结来说就是扩展性差，可靠性不高，维护成本高。到后面引入了SOA服务化，但是，由于 SOA 早期均使用了总线模式，这种总线模式是与某种技术栈强绑定的，比如：J2EE。这导致很多企业的遗留系统很难对接，切换时间太长，成本太高，新系统稳定性的收敛也需要一些时间。最终 SOA 看起来很美，但却成为了企业级奢侈品，中小公司都望而生畏。
        
    3.1 最期的单体架构带来的问题
    
    		单体架构在规模比较小的情况下工作情况良好，但是随着系统规模的扩大，它暴露出来的问题也越来越多，主要有以下几点：
    
    1.复杂性逐渐变高
    
    		比如有的项目有几十万行代码，各个模块之间区别比较模糊，逻辑比较混乱，代码越多复杂性越高，越难解决遇到的问题。
    
    2.技术债务逐渐上升
    
    		公司的人员流动是再正常不过的事情，有的员工在离职之前，疏于代码质量的自我管束，导致留下来很多坑，由于单体项目代码量庞大的惊人，留下的坑很难被发觉，这就给新来的员工带来很大的烦恼，人员流动越大所留下的坑越多，也就是所谓的技术债务越来越多。
    
    3.部署速度逐渐变慢
    
    		这个就很好理解了，单体架构模块非常多，代码量非常庞大，导致部署项目所花费的时间越来越多，曾经有的项目启动就要一二十分钟，这是多么恐怖的事情啊，启动几次项目一天的时间就过去了，留给开发者开发的时间就非常少了。
    
    4.阻碍技术创新
    
    		比如以前的某个项目使用struts2写的，由于各个模块之间有着千丝万缕的联系，代码量大，逻辑不够清楚，如果现在想用spring mvc来重构这个项目将是非常困难的，付出的成本将非常大，所以更多的时候公司不得不硬着头皮继续使用老的struts架构，这就阻碍了技术的创新。
    
    5.无法按需伸缩
    
    		比如说电影模块是CPU密集型的模块，而订单模块是IO密集型的模块，假如我们要提升订单模块的性能，比如加大内存、增加硬盘，但是由于所有的模块都在一个架构下，因此我们在扩展订单模块的性能时不得不考虑其它模块的因素，因为我们不能因为扩展某个模块的性能而损害其它模块的性能，从而无法按需进行伸缩。
    
    3.2 微服务与单体架构区别
    
    		单体架构所有的模块全都耦合在一块，代码量大，维护困难，微服务每个模块就相当于一个单独的项目，代码量明显减少，遇到问题也相对来说比较好解决。
    
    单体架构所有的模块都共用一个数据库，存储方式比较单一，微服务每个模块都可以使用不同的存储方式（比如有的用redis，有的用mysql等），数据库也是单个模块对应自己的数据库。
    
    单体架构所有的模块开发所使用的技术一样，微服务每个模块都可以使用不同的开发技术，开发模式更灵活。

2.微服务用什么优势，go或者py去做，怎么实现

	python微服务
	
		python的一个比较轻量级的web服务框架tornado，可以快速搭建起基础的简易的http服务。
github.com/xuanbo/eureka-client 这里也有其他开发人员实现了eureka的python客户端，同时还支持跨服务的访问，基于这两个框架基础就很容实现python的微服务并注册到eureka注册中心，详细代码如下

```
import string

import tornado.web
import py_eureka_client.eureka_client as eureka_client
from tornado.options import define, options

#当前服务监听的端口
define("port", default=8505, help="run on the given port", type=int)
#当前在eureka中注册的名称
define("appName", default='python', help="app name in eureka", type=string)

class IndexHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('[GET] python server...')

def eurekaclient():

```
tornado.options.parse_command_line()
#注册eureka服务
eureka_client.init(eureka_server="http://127.0.0.1:8502/eureka/",
                                   app_name=options.appName,
                                   instance_port=options.port,
                                   # 调用其他服务时的高可用策略，可选，默认为随机
                                   ha_strategy=eureka_client.HA_STRATEGY_RANDOM,
                   data_center_name='MyOwn'
                   )
app = tornado.web.Application(handlers=[(r"/", IndexHandler)])
http_server = tornado.httpserver.HTTPServer(app)
http_server.listen(options.port)
tornado.ioloop.IOLoop.instance().start()
print("eureka exec")
```
if __name__ == "__main__":
    eurekaclient()
```

## Go微服务

和python的流程类似，只是换了一种语言实现而已。具体流程和python的完全一致就不做特别的介绍。主要还是实现eureka客户端的这款是我们需要关注的核心，也是有待改造和完善的这里我们只做基础研究使用

```
package main

import (
	"fmt"
	"net/http"
	 eureka "github.com/xuanbo/eureka-client"
)

func main() {
	// create eureka client
	client := eureka.NewClient(&eureka.Config{
		DefaultZone:           "http://127.0.0.1:8502/eureka/",
		App:                   "gomoudle",
		Port:                  8506,
		RenewalIntervalInSecs: 10,
		DurationInSecs:        30,
		Metadata: map[string]interface{}{
			"VERSION":              "0.1.0",
			"NODE_GROUP_ID":        0,
			"PRODUCT_CODE":         "DEFAULT",
			"PRODUCT_VERSION_CODE": "DEFAULT",
			"PRODUCT_ENV_CODE":     "DEFAULT",
			"SERVICE_VERSION_CODE": "DEFAULT",
		},
	})
	// start client, register、heartbeat、refresh
	client.Start()

	http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
		// full applications from eureka server
		//apps := client.Applications
		//b, _ := json.Marshal(apps)
		name := "hello welcome go moudle"
		_, _ = writer.Write([]byte(name))
	})

	// start http server
	if err := http.ListenAndServe(":8506", nil); err != nil {
		fmt.Println(err)
	}
}
```





3.分布式CAP理论是什么

```
CAP理论概念:
	一个分布式系统最多只能同时满足一致性（Consistency）、可用性（Availability）和分区容错性（Partition tolerance）这三项中的两项。

简单介绍：
	一致性（Consistence） :所有节点访问同一份最新的数据副本
	可用性（Availability）:每次请求都能获取到非错的响应——但是不保证获取的数据为最新数据
	分区容错性（Partition tolerance） : 分布式系统在遇到某节点或网络分区故障的时候，仍然能够对外提供满足一致性和可用性的服务。
	
CAP无法同时满足，但是并不是三选二！
	首先，P分区容错性是必须要满足的，如果舍弃分区容错，也就失去分布式系统的意义了。当网络分区之后P是前提，决定了P之后才有C和A的选择。也就是说分区容错性（Partition tolerance）我们是必须要实现的。
	
总结和梳理CAP理论:
	一般我们说的分布式系统，P：分区容错性(partition-tolerance)这个是必需的，这是客观存在的。
CAP是无法完全兼顾的，从上面的例子也可以看出，我们可以选AP，也可以选CP。但是，要注意的是：不是说选了AP，C就完全抛弃了。不是说选了CP，A就完全抛弃了！
在CAP理论中，C所表示的一致性是强一致性(每个节点的数据都是最新版本)，其实一致性还有其他级别的：

	弱一致性：弱一致性是相对于强一致性而言，它不保证总能得到最新的值；
	最终一致性(eventual consistency)：放宽对时间的要求，在被调完成操作响应后的某个时间点，被调多个节点的数据最终达成一致

可用性的值域可以定义成0到100%的连续区间。



```





4.分布式数据库BASE理论是什么

```
BASE理论

eBay的架构师Dan Pritchett源于对大规模分布式系统的实践总结，在ACM上发表文章提出BASE理论，BASE理论是对CAP理论的延伸，核心思想是即使无法做到强一致性（Strong Consistency，CAP的一致性就是强一致性），但应用可以采用适合的方式达到最终一致性（Eventual Consitency）。

BASE是指基本可用（Basically Available）、软状态（ Soft State）、最终一致性（ Eventual Consistency）。

基本可用（Basically Available）

基本可用是指分布式系统在出现故障的时候，允许损失部分可用性，即保证核心可用。

电商大促时，为了应对访问量激增，部分用户可能会被引导到降级页面，服务层也可能只提供降级服务。这就是损失部分可用性的体现。

软状态（ Soft State）

软状态是指允许系统存在中间状态，而该中间状态不会影响系统整体可用性。分布式存储中一般一份数据至少会有三个副本，允许不同节点间副本同步的延时就是软状态的体现。mysql replication的异步复制也是一种体现。

最终一致性（ Eventual Consistency）

最终一致性是指系统中的所有数据副本经过一定时间后，最终能够达到一致的状态。弱一致性和强一致性相反，最终一致性是弱一致性的一种特殊情况。

ACID和BASE的区别与联系

ACID是传统数据库常用的设计理念，追求强一致性模型。BASE支持的是大型分布式系统，提出通过牺牲强一致性获得高可用性。

ACID和BASE代表了两种截然相反的设计哲学

在分布式系统设计的场景中，系统组件对一致性要求是不同的，因此ACID和BASE又会结合使用。
```