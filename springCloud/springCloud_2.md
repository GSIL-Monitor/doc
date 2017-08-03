
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Spring Cloud Eureka实现微服务治理</title>
  <meta name="description" content="">
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
  <meta name="viewport" content="width=1100, maximum-scale=1.0, user-scalable=yes"/>
  <link rel="stylesheet" href="/assets/app-2525f05fd8e8ba68539fd96309305dd73e5549a8.css">
  <script src="/assets/app-97ad868fdc01a84efd6290132048872eb24c252c.js" data-main="article/show"></script>
  <script>require('main')</script>
  <!--[if lt IE 9]>
  <script src="/js/html5shiv-3.7.0.min.js"></script>
  <script src="/js/respond-1.4.2.min.js"></script>
  <![endif]-->
        <base target="_blank"/>
  </head>
<body class="" cnzz_category="文章详情">

      <header>
  <style>
    .navbar-nav>li {margin-right: 5px;}
  </style>
  <div class="navbar navbar-default">
    <div class="container">
      <ul class="nav navbar-nav">
        <li><a href="/square/">首页</a></li>
        <li><a href="/articles/?sort=yuan">文章</a></li>
        <li><a href="/ask">问答</a></li>
                <li><a href="/tool/">百宝箱</a></li>
        <li><a href="http://wiki.atatech.org">Wiki</a></li>
        <li class="divider"></li>
        <li><a href="/groups/">圈子</a></li>
        <li><a href="/teams/">团队博客</a></li>
        <li><a href="/activity/">活动</a></li>
        <li><a href="/conference/">会议</a></li>
        <li><a href="/techtalk/">对话科技</a></li>
        <li><a href="/news/">新人专区</a></li>
        <li class="divider"></li>
                <li><a href="/databoard/" target="_blank">数据看板</a></li>
                        <li class="li-margin"><a class="red" href="/publication" target="_blank">技术月刊&精华沉淀</a></li>
              </ul>
      <ul class="nav navbar-nav pull-right">

                                <li><a target="_self" href="/notifications/">消息            <em class="label label-danger">29</em></a></li>
        <li><a target="_self" href="/feed/">动态</a></li>
        <li class="divider"></li>
        <li class="dropdown">
          <a target="_self" href="/users/131096#information" class="navbar-avatar" data-toggle="dropdown">
            <img           src="http://img4.tbcdn.cn/L1/461/1/u_identicon_21e3421b828551f99142364273136912.png_40x40" width="20" height="20"
   alt="蒙可" class="img-circle">
            <span>&nbsp;蒙可</span>
            <span class="caret"></span>
          </a>
          <ul class="dropdown-menu" role="menu">
            <li>
              <a target="_self" href="/users/131096/own#information">
                <span class="glyphicon glyphicon-edit"></span>
                <span>我的文章</span>
              </a>
            </li>
            <li>
              <a target="_self" href="/activity/new">
                <span class="glyphicon glyphicon-fire"></span>
                <span>发起活动</span>
              </a>
            </li>
            <li>
              <a target="_self" href="/users/131096/ask#information">
                <span class="glyphicon glyphicon-question-sign"></span>
                <span>我的问题</span>
              </a>
            </li>
            <li>
              <a target="_self" href="/users/131096/mark#information">
                <span class="glyphicon glyphicon-star-empty"></span>
                <span>我的收藏</span>
              </a>
            </li>
            <li>
              <a target="_self" href="/users/131096/groups">
                <span class="glyphicon glyphicon-record"></span>
                <span>我的圈子</span>
              </a>
            </li>
            <li>
              <a target="_self" href="/teams">
                <span class="glyphicon glyphicon-list-alt"></span>
                <span>我的博客</span>
              </a>
            </li>

                                                                                                                                    <li>
              <a target="_self" href="/settings/">
                <span class="glyphicon glyphicon-cog"></span>
                <span>个人设置</span>
              </a>
            </li>
                        <li class="divider"></li>
            <li>
              <a target="_self" href="/logout" data-method="post">
                <span class="glyphicon glyphicon-log-out"></span>
                <span>退出</span>
              </a>
            </li>
          </ul><!-- /.dropdown -->
        </li>
        <li class="dropdown quick-search">
          <button class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
            <span class="glyphicon glyphicon-search"></span></button>
          <div class="dropdown-menu form-group" role="menu">
            <form target="_self" action="/search" method="get" class="quick-form js-active-on-valid">
              <input type="hidden" name="type" value="ARTICLE"/>
              <input type="text" name="q" class="form-control" placeholder="全站搜索" required>
              <button type="submit" class="quick_btn"></button>
            </form>
          </div>
        </li>
      </ul>
    </div>
  </div>

  </header>    <div class="container header-notice-container">
      </div>
  <style>
    .content table td, .content table th{
      word-break: break-all;
      word-wrap:break-word;
    }
    .content table tr:nth-child(2n) {
      background-color: #f6f8fa;
    }
    .influential-article{
      background: url(http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/5536328776615091af700516adbe8319.png);
      background-size: 200px 200px;
      width: 200px;
      height: 200px;
      position: absolute;
      right: 174px;
      top: 127px;
      z-index: 5000;
    }

    .article-anchor {float: left;line-height:1.5;margin-left: -20px;padding-right: 4px;position:absolute;}
    .article-anchor:hover{cursor: pointer;}
    .article-anchor .ancher-link {visibility:hidden;}
    .medal {margin-left: -12px; margin-right: 5px;}
    .medal-show {display: inline-block;position: absolute; background-color: #fe4625;border-radius: 0.25em; line-height: 25px;}
    .medal-show span {color: #fff;padding: 10px;}
    .medal-hide {display: none;position: absolute}
    .medal-show:after {
      border-color: #f0f0f0 transparent transparent #f0f0f0;
      border-style: solid solid solid solid;
      border-width: 6px 3px;

      /* 必須指定，才能顯示內容 */
      content: '';

      height: 0px;
      left: 0px;

      /* 必須指定，否則會變梯形 */
      position: absolute;
      top: 25px;
      width: 0px;
    }
    .medal-show:before {
      border-color: #fe4625 transparent transparent #fe4625;
      border-style: solid solid solid solid;
      border-width: 6px 10px;

      /* 必須指定，才能顯示內容 */
      content: '';

      height: 0px;
      left: 0px;

      /* 必須指定，否則會變梯形 */
      position: absolute;

      top: 25px;
      width: 0px;
    }
    .pull-right .article-favor, .article-favor, .article-delete {
      margin-left: 10px;
    }

    .article-tag {
      margin-right: 5px;
      color: #999999;
    }

    .knowledge-tags a {
      margin-right: 8px;
      color: #999999;
    }

    .knowledge-tags a:hover {
      color: #428bca;
      text-decoration: underline;
    }

    .labels span, .labels a {
      font-size: 13px;
    }
    .labels div span, .labels .knowledge-tags span, .span-weight {
      font-weight: 700;
      color: #666;
    }

    .article-title {
      text-align: center;
      border-bottom: #dfdfdf 1px solid;
      margin-bottom: 10px;
      padding-bottom: 15px;
    }

    .special-attr {
      color: #FF3300;
      margin-right: 10px;
      font-weight: 700;
    }

    .special-yuan {
      color: #009933 !important;
    }

  </style>
  
    <div class="container">
    <div class="_article-container">
      <div class="_article-box" role="main">
        <article class="_article">
          <header class="heading">
            <div class="infos clearfix">
              <a href="/users/239455" class="info" title="谦雅"><img           src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/fbbe40535f2dc90048600509df956f03.jpg?x-oss-process=image/resize,w_80,h_80" width="32" height="32"
   alt="谦雅" class="img-user"><span>&nbsp;谦雅</span></a>                            <span class="info">
                              <time class="" datetime="2017-08-02T13:42:21+08:00">2017-08-02 13:42:21</time>
                              </span>
                              <span class="info">发表于：                  <a href="/groups/1983">翻译平台</a></span>
                            
              <div class="pull-right">
                <span class="info">4  阅读</span>
                <span id="tools_favor_top" class="article-favor">
                  <a data-toggle="modal" href="/articles/87342/mark/" data-target="#my-mark-modal" title="收藏" class="btn btn-default item-tomark" role="button" rel="nofollow" >
                    0&nbsp;<span>收藏</span>
                  </a>
                  <a href="/articles/87342/unmark" title="取消收藏" class="btn btn-primary item-tounmark" role="button" rel="nofollow" data-remote="true" data-done="$(this).prev().addBack().toggle()" data-method="post" style="display:none">
                    1&nbsp;<span>取消收藏</span>
                  </a>
                </span>
                
                              </div>
            </div>

            
            <div class="labels">
  <div class="knowledge-tags">
    <span class="k-span">知识体系：</span>
          <a class="ktag" href="/articles/?kid=22" data-kid="22">高可用架构</a>
        <button href="/knowledge/knowledgeTags?type=show&aid=87342" class="btn btn-default btn-xs kbutton" data-toggle="modal" data-target="#editKnowledgeTagModal">
      <span class="glyphicon glyphicon-pencil"></span>
      修改知识体系
    </button>
  </div>
</div>

  <div class="modal fade" id="editKnowledgeTagModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true"></div>

            <div class="labels">
    <form target="_self" accept-charset="UTF-8" action="/articles/87342/tags" autocomplete="off" method="post"
    data-remote="true"
    data-done="$(this).closest('.labels').replaceWith(r);$.alert('修改成功', 'success')"
    style="display:none">
    <input type="hidden" name="_method" value="put">
    <div class="form-group">
      <input type="text" name="tags" class="form-control js-tag-complete" value="Java核心技术 spring cloud 服务治理 微服务 Eureka ">
    </div>
    <div class="form-group text-right">
      <button type="button" class="btn btn-default btn-sm" onclick="$(this).closest('.labels').children().toggle()">取消</button>
      <button type="submit" class="btn btn-primary btn-sm">更新</button>
    </div>
  </form>
  
  <div>
    <span>文章标签：</span>
          <a class="article-tag" href="/search?q=Java核心技术&type=INSIDE_ARTICLE_TAG">Java核心技术</a>
          <a class="article-tag" href="/search?q=spring&type=INSIDE_ARTICLE_TAG">spring</a>
          <a class="article-tag" href="/search?q=cloud&type=INSIDE_ARTICLE_TAG">cloud</a>
          <a class="article-tag" href="/search?q=服务治理&type=INSIDE_ARTICLE_TAG">服务治理</a>
          <a class="article-tag" href="/search?q=微服务&type=INSIDE_ARTICLE_TAG">微服务</a>
          <a class="article-tag" href="/search?q=Eureka&type=INSIDE_ARTICLE_TAG">Eureka</a>
              <button type="button" id="edit-tags" class="btn btn-default btn-xs" onclick="$(this).closest('.labels').children().toggle()">
        <span class="glyphicon glyphicon-pencil"></span>
        修改标签
      </button>
      <a href="/articles/87342/tags/history" class="btn btn-default btn-xs" data-toggle="modal" data-target="#modal-tag-history">
        <span class="glyphicon glyphicon-time"></span>
        标签历史
      </a>
      </div>
</div>

                          <div class="labels">
                <span class="span-weight">附加属性：</span>
                                  <span class="special-attr">内部资料请勿外传</span>
                                                  <span class="special-attr special-yuan">作者原创</span>
                                                                                                              </div>
            
            
                      </header>

            <div class="article-title">
              <h1 class="title clearfix unsafe">
                Spring Cloud Eureka实现微服务治理
                              </h1>
            </div>

          <div class="content unsafe" style="background: rgba(0, 0, 0, 0) url(/public/watermark.png) repeat scroll 0 0">
                          <h2>背景</h2>
<p>Spring Cloud Eureka 是 Spring Cloud Netflix 微服务套件中的一部分，它基于 Netflix Eureka做了二次封装，主要负责完成微服务架构中的服务治理功能。Spring Cloud通过为 Eureka增加了 Spring Boot风格的自动化配置，我们只需通过简单引入依赖和注解配置就能 让Spring Boot构建的微服务应用轻松地与Eureka服务治理体系进行整合。</p>
<h2>服务治理</h2>
<p>服务治理可以说是微服务架构中最为核心和基础的模块，它主要用来实现各个微服务实例的自动化注册与发现。<br>
在最初开始构建微服务系统的时候可能服务并不多，我们可以通过做一些静态配置来完成服务的调用。比如，有两个服务A和B,其中服务A需要调用服务B来完成一个业务操作时，为了实现服务B的高可用，不论采用服务端负载均衡还是客户端负载均衡，都需要手工维护服务B的具体实例清单。但是随着业务的发展，系统功能越来越复杂，相应的微服务应用也不断增加，我们的静态配置就会变得越来越难以维护。并且面对不断发展的业务，我们的集群规模、服务的位置、服务的命名等都有可能发生变化，如果还是通过手工维护的方式，那么极易发生错误或是命名冲突等问题。同时，对于这类静态内容的维护 也必将消耗大量的人力。<br>
为了解决微服务架构中的服务实例维护问题，产生了大量的服务治理框架和产品。这些框架和产品的实现都围绕着服务注册与服务发现机制来完成对微服务应用实例的自动化管理。</p>
<h3>服务注册</h3>
<p>在服务治理框架中，通常都会构建一个注册中心，每个服务单元向注册<br>
中心登记自己提供的服务，将主机与端口号、版本号、通信协议等一些附加信息告 知注册中心，注册中心按服务名分类组织服务清单。比如，我们有两个提供服务A 的进程分别运行于 192.168.0.100:8000 和 192.168.0.101:8000 位置上， 另外还有三个提供服务B的进程分别运行于192.168.0.100:9000、<br>
192.168.0.  101:9000、192.168.0.102:9000 位置上。当这些进程均启动, 并向注册中心注册自己的服务之后，注册中心就会维护类似下面的一个服务清单。 另外，服务注册中心还需要以心跳的方式去监测清单中的服务是否可用，若不可用 需要从服务清单中剔除，达到排除故障服务的效果。</p>
<div class="table-contianer"><table><thead>
<tr>
<th>服务名</th><th>位詈</th></tr>
</thead><tbody>
<tr>
<td>服务A</td><td>192.168.0.100:8000. 192.168.0.101:8000</td></tr>
<tr>
<td>服务B</td><td>192.168.0.100:9000、192.168.0.101:9000、192.168.0.102:9000</td></tr>
</tbody></table></div>
<h3>服务发现</h3>
<p>由于在服务治理框架下运作，服务间的调用不再通过指定具体的实例地址来实现，而是通过向服务名发起请求调用实现。所以，服务调用方在调用服务提供方接口的时候，并不知道具体的服务实例位置。因此，调用方需要向服务注册中心咨询服务，并获取所有服务的实例清单，以实现对具体服务实例的访问。比如，现有服务C希望调用服务A，服务C就需要向注册中心发起咨询服务请求，服务注册中心就会将服务A的位置清单返回给服务C，如按上例服务A的情况，C便获得 了服务 A 的两个可用位置 192.168.0.100:8000 和 192.168.0.101:8000。<br>
当服务C要发起调用的时候，便从该清单中以某种轮询策略取出一个位置来进行服务调用，这就是后续我们将会介绍的客户端负载均衡。这里我们只是列举了一种简单的服务治理逻辑，以方便理解服务治理框架的基本运行思路。实际的框架为了性能等因素，不会釆用每次都向服务注册中心获取服务的方式，并且不同的应用场景在缓存和服务剔除等机制上也会有一些不同的实现策略。</p>
<h2>Netflix Eureka</h2>
<h3>简介</h3>
<p>Spring Cloud Eureka,使用Netflix Eureka来实现服务注册与发现，它既包含了服务端组件，也包含了客户端组件，并且服务端与客户端均采用Java编写，所以Eureka主要适用于通过Java实现的分布式系统，或是与JVM兼容语言构建的系统。但是，由于Eureka服务端的服务治理机制提供了完备的RESTftil API,所以它也支持将非Java语言构建的微服务应用纳入Eureka的服务治理体系中来。只是在使用其他语言平台的时候，需要自己来实现Eureka的客户端程序。不过庆幸的是，在目前几个较为流行的开发平台上，都己经有了 一些针对Eureka注册中心的客户端实现框架，比如.NET平台的Steeltoe、Node.js的 eureka-js-client 等。<br>
Eureka服务端，我们也称为服务注册中心。它同其他服务注册中心一样，支持高可用配置。它依托于强一致性提供良好的服务实例可用性，可以应对多种不同的故障场景。如果Eureka以集群模式部署，当集群中有分片出现故障时，那么Eureka就转入自我保护模式。它允许在分片故障期间继续提供服务的发现和注册，当故障分片恢复运行时，集群中的其他分片会把它们的状态再次同步回来。以在AWS上的实践为例，Netflix推荐每个可用的区域运行一个Eureka服务端，通过它来形成集群。不同可用区域的服务注册中心通过 异步模式互相复制各自的状态，这意味着在任意给定的时间点每个实例关于所有服务的状 态是有细微差别的。<br>
Eureka客户端，主要处理服务的注册与发现。客户端服务通过注解和参数配置的方式， 嵌入在客户端应用程序的代码中，在应用程序运行时，Eureka客户端向注册中心注册自身 提供的服务并周期性地发送心跳来更新它的服务租约。同时，它也能从服务端查询当前注 册的服务信息并把它们缓存到本地并周期性地刷新服务状态。</p>
<h3>服务注册中心搭建</h3>
<h4>增加pom依赖</h4>
<pre><code data-language="xml">&lt;dependencyManagement&gt;
        &lt;dependencies&gt;
            &lt;dependency&gt;
                &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
                &lt;artifactId&gt;spring-cloud-dependencies&lt;/artifactId&gt;
                &lt;version&gt;Camden.SR2&lt;/version&gt;
                &lt;type&gt;pom&lt;/type&gt;
                &lt;scope&gt;import&lt;/scope&gt;
            &lt;/dependency&gt;
        &lt;/dependencies&gt;
    &lt;/dependencyManagement&gt;

    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
            &lt;artifactId&gt;spring-cloud-starter-eureka-server&lt;/artifactId&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;
</code></pre><h4>增加启动文件</h4>
<p>通过@EnableEurekaServer注解启动一个服务注册中心提供给其他应用进行对话。 这一步非常简单，只需在一个普通的SpringBoot应用中添加这个注解就能开启此功能</p>
<pre><code data-language="java">@EnableEurekaServer
@SpringBootApplication
public class RegistryApplication {

    public static void main(String[] args) {
        new SpringApplicationBuilder(ConfigApplication.class).web(true).run(args);
    }

}
</code></pre><p>通过@EnableEurekaServer注解启动一个服务注册中心提供给其他应用进行对话。 这一步非常简单，只需在一个普通的SpringBoot应用中添加这个注解就能开启此功能。</p>
<h4>增加application.properties</h4>
<pre><code data-language="xml">server.port=7002

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
eureka.client.serviceUrl.defaultZone=http://localhost:${server.port}/eureka/
</code></pre><p>server.port ： 设置注册中心启动端口<br>
eureka.client.register-with-eureka：由于该应用为注册中心，所1以设置 为false,代表不向注册中心注册自己。 <br>
eureka.client.fetch-registry ：由于注册中心的职责就是维护服务实例， 它并不需要去检索服务，所以也设置为false。</p>
<h4>访问注册中心</h4>
<p>启动成功以后，访问http://localhost:7002/， 即可以看见注册中心界面。<br>
可以看到如下图所示的Eureka信息面板，其中Instances currently registered with Eureka栏是空的，说明该注册中心还没有注册任何服务。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/b01d7b2cab0d519587ee4a924d6c5881.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/b01d7b2cab0d519587ee4a924d6c5881.png" alt="image.png" title="image.png"></a></p>
<h3>注册服务提供者</h3>
<h4>增加pom依赖</h4>
<pre><code data-language="xml">&lt;dependencyManagement&gt;
        &lt;dependencies&gt;
            &lt;dependency&gt;
                &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
                &lt;artifactId&gt;spring-cloud-dependencies&lt;/artifactId&gt;
                &lt;version&gt;Camden.SR2&lt;/version&gt;
                &lt;type&gt;pom&lt;/type&gt;
                &lt;scope&gt;import&lt;/scope&gt;
            &lt;/dependency&gt;
        &lt;/dependencies&gt;
    &lt;/dependencyManagement&gt;

    &lt;dependencies&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
            &lt;artifactId&gt;spring-boot-starter-web&lt;/artifactId&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
            &lt;artifactId&gt;spring-cloud-starter-eureka&lt;/artifactId&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
            &lt;artifactId&gt;spring-boot-starter-actuator&lt;/artifactId&gt;
        &lt;/dependency&gt;

    &lt;/dependencies&gt;
</code></pre><h4>增加测试http接口</h4>
<p>通过注入DiscoveryClient对象，可以再日志中打印出服务的相关内容。</p>
<pre><code data-language="java">@RestController
public class HelloController {

    @Autowired
    DiscoveryClient client;

    @RequestMapping(value = &quot;hello&quot;, method = RequestMethod.GET)
    public String index() {

        ServiceInstance serviceInstance = client.getLocalServiceInstance();

        System.out.println(&quot;path = index, &quot;
            + &quot;host = &quot; + serviceInstance.getHost() + &quot;, &quot;
            + &quot;port = &quot; + serviceInstance.getPort() + &quot;, &quot;
            + &quot;serviceId = &quot; + serviceInstance.getServiceId());

        return &quot;Hello World!&quot;;
    }

}
</code></pre><h4>增加启动类</h4>
<p>在启动类中通过加上@EnableDiscoveryClient注解，激活Eureka中的 DiscoveryClient实现（自动化配置，创建DiscoveryClient接口针对Eureka客户端的EurekaDiscoveryClient实例），才能实现上述Controller中对服务信息的输出。</p>
<pre><code data-language="java">@EnableEurekaClient
@SpringBootApplication
public class Provider1Application {

    public static void main(String[] args) {
        SpringApplication.run(Provider1Application.class, args);
    }

}
```在主类中通过加上@EnableDiscoveryClient注解，激活Eureka中的 DiscoveryClient实现（自动化配置，创建DiscoveryClient接口针对Eureka客户 端的EurekaDiscoveryClient实例），才能实现上述Controller中对服务信息的输出。

#### 增加配置文件
``` xml
server.port=7003
spring.application.name=hello-service
eureka.client.serviceUrl.defaultZone=http://localhost:7002/eureka/
</code></pre><h4>启动注册服务</h4>
<p>启动成功以后，即可以在注册中心看到注册的服务。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/ba32e48744431d4337916c34052e201c.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/ba32e48744431d4337916c34052e201c.png" alt="image.png" title="image.png"></a></p>
<h3>高可用注册中心</h3>
<p>在微服务架构这样的分布式环境中，我们需要充分考虑发生故障的情况，所以在生产环境中必须对各个组件进行高可用部署，对于微服务如此，对于服务注册中心也一样。但 是到本节为止，我们一直都在使用单节点的服务注册中心，这在生产环境中显然并不合适, 我们需要构建高可用的服务注册中心以增强系统的可用性。</p>
<p>Eureka Server的设计一幵始就考虑了高可用问题，在Eureka的服务治理设计中，所有节点即是服务提供方，也是服务消费方，服务注册中心也不例外。是否还记得在单节点的 配置中，我们设置过下面这两个参数，让服务注册中心不注册自己。</p>
<pre><code data-language="xml">eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
</code></pre><p>可以增加多个服务提供者，以保证服务的高可用。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/8a8cfba38aac86f9333b137181eb51f3.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/8a8cfba38aac86f9333b137181eb51f3.png" alt="image.png" title="image.png"></a></p>
<h3>服务发现与消费</h3>
<p>服务消费者主要完成两个目标，发现服务以及消费服务。其中, 服务发现的任务由Eureka的客户端完成，而服务消费的任务由Ribbon完成。Ribbon是一个基 于HTTP和TCP的客户端负载均衡器，它可以在通过客户端中配置的ribbonServerList服务端列表去轮询访问以达到均衡负载的作用。当Ribbon与Eureka联合使用时，Ribbon的服务实例清单RibbonServerList会被 DiscoveryEnabledNIWSServerList重写，扩展成从Eureka注册中心中获取服务端列表。同时它也会用NIWSDiscoveryPing来取代工Ping,它将职责委托给Eureka来确定服务端是否己经启动。</p>
<h4>增加pom依赖</h4>
<pre><code data-language="xml">&lt;dependencyManagement&gt;
        &lt;dependencies&gt;
            &lt;dependency&gt;
                &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
                &lt;artifactId&gt;spring-cloud-dependencies&lt;/artifactId&gt;
                &lt;version&gt;Camden.SR2&lt;/version&gt;
                &lt;type&gt;pom&lt;/type&gt;
                &lt;scope&gt;import&lt;/scope&gt;
            &lt;/dependency&gt;
        &lt;/dependencies&gt;
    &lt;/dependencyManagement&gt;

    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
            &lt;artifactId&gt;spring-boot-starter-web&lt;/artifactId&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
            &lt;artifactId&gt;spring-cloud-starter-eureka&lt;/artifactId&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
            &lt;artifactId&gt;spring-cloud-starter-ribbon&lt;/artifactId&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;
</code></pre><h4>增加启动类</h4>
<pre><code data-language="java">@EnableDiscoveryClient
@SpringBootApplication
public class ConsumerApplication {

    @Bean
    @LoadBalanced
    RestTemplate restTemplate() {
        return new RestTemplate();
    }

    public static void main(String[] args) {
        SpringApplication.run(ConsumerApplication.class, args);
    }
}
</code></pre><p>通过@EnableDisc〇veryClient注解让该应用注册为Eureka客户端应用，以获得服务发现的能力。同时，在该主类中创建RestTemplate的Spring Bean实例，并通过@RestTemplate注解开启客户端负载均衡。</p>
<h4>增加服务消费类</h4>
<pre><code data-language="java">@RestController
public class HelloController {

    @Autowired
    RestTemplate restTemplate;

    @RequestMapping(value = &quot;/hello&quot;, method = RequestMethod.GET)
    public String hello() {
        return restTemplate.getForEntity(&quot;http://HELLO-SERVICE/hello&quot;, String.class).getBody();
    }
}
</code></pre><p>RestTemplate来实现对HELLO-SERVICE服务提供的/hello接口进行调用。可以看到这里访问的地址是服务名HELLO-SERVICE,而不是一个具体的地址，在服务治理框架中，这是一个非常重要的特性，也符合对服务治理的解释。</p>
<h4>增加配置文件</h4>
<pre><code data-language="xml">server.port=7005
spring.application.name=ribbon-consumer
eureka.client.serviceUrl.defaultZone=http://localhost:7002/eureka/
</code></pre><h4>启动消费端</h4>
<p>启动成功后便可以在注册中心看到消费端。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7e276dc9e26f5e4588fa1c8555d74161.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7e276dc9e26f5e4588fa1c8555d74161.png" alt="image.png" title="image.png"></a>
多次请求消费端，会发现请求最终会随机调用提供者接口，实现负载均衡。</p>
<h2>Eureka详解</h2>
<p>Eureka服务治理体系中的三个核心角色：服务注册中心、服务提供者以及服务消费者。但是在实践中，我们的系统结构往往都要比上述示例复杂得多，如果仅仅依靠之前构建的服务治理内容，大多数情况是无 法完全直接满足业务系统需求的，我们还需要根据实际情况来做一些配置、调整和扩展。 </p>
<h2>基础架构</h2>
<h3>服务注册中心</h3>
<p>Eureka提供的服务端，提供服务注册与发现的功能</p>
<h3>服务提供者</h3>
<p>提供服务的应用，可以是SpringBoot应用，也可以是其他技术平台且遵循Eureka通信机制的应用。它将自己提供的服务注册到Eureka,以供其他应用发现。</p>
<h3>服务消费者</h3>
<p>消费者应用从服务注册中心获取服务列表，从而使消费者可以知道去何处调用其所需要的服务。使用Ribbon和Feign都可以实现服务消费。<br>
很多时候，客户端既是服务提供者也是服务消费者。</p>
<h2>服务治理机制</h2>
<p>Eureka实现的服务治理体系运作方式<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1ed87113917ff8cc548015c1a096748a.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1ed87113917ff8cc548015c1a096748a.png" alt="image.png" title="image.png"></a></p>
<ul>
<li> &quot;服务注册中心-1&quot; 和 &quot;服务注册中心-2&quot;，它们互相注册组成了高可用集群。 </li>
<li>&quot;服务提供者”启动了两个实例，一个注册到“服务注册中心-1&quot;上，另外一个注 册到“服务注册中心-2”上。</li>
<li>还有两个&quot;服务消费者&quot;，它们也都分别只指向了一个注册中心。</li>
</ul>
<h3>服务提供者</h3>
<h4>服务注册</h4>
<p>&quot;服务提供者&quot;在启动的时候会通过发送REST请求的方式将自己注册到Eureka Server 上，同时带上了自身服务的一些元数据信息。Eureka Server接收到这个REST请求之后， 将元数据信息存储在一个双层结构Map中，其中第一层的key是服务名，第二层的key是具体服务的实例名。（Eureka信息面板中一个服务有多个实例的情况，这些内容就是以这样的双层Map形式存储的。）<br>
在服务注册时，需要确认一下eureka.client.register-with-eureka=true 参数是否正确，该值默认为true。若设置为false将不会启动注册操作。</p>
<h4>服务同步</h4>
<p>两个服务提供者分别注册到了两个不同的服务注册中心上，也就是说，它们的信息分别被两个服务注册中心所维护。此时，由于服务注册中心之间因互相注册为服务，当服务提供者发送注册请求到一个服务注册中心时，它会将该请求转发 给集群中相连的其他注册中心，从而实现注册中心之间的服务同步。通过服务同步，两个服务提供者的服务信息就可以通过这两台服务注册中心中的任意一台获取到。</p>
<h4>服务续约</h4>
<p>在注册完服务之后，服务提供者会维护一个心跳用来持续告诉Eureka Server: “我还活着”，以防止Eureka Server的“剔除任务”将该服务实例从服务列表中排除出去，我们称该操作为服务续约（Renew)。<br>
关于服务续约有两个重要属性，我们可以关注并根据需要来进行调整：<br>
eureka.instance.lease-renewal-interval-in一seconds=30，参数用于定义月艮 务续约任务的调用间隔时间，默认为30秒。<br>
eureka.instance.lease-expiration-duration-in-seconds=90，参数用于定义服务失效的时间，默认为90秒。</p>
<h3>服务消费者</h3>
<h4>获取服务</h4>
<p>到这里，在服务注册中心己经注册了一个服务，并且该服务有两个实例。当我们启动服务消费者的时候，它会发送一个REST请求给服务注册中心，来获取上面注册的服务清单。为了性能考虑，Eureka Server会维护一份只读的服务清单来返回给客户端，同时该缓存清单会每隔30秒更新一次。<br>
获取服务是服务消费者的基础，所以必须确保eureka.client.fetch-registry= true参数没有被修改成false,该值默认为true。若希望修改缓存清单的更新时间，可 以通过 eureka.client.registry-fetch-interval-seconds=30 参数进行修改，该参数默认值为30,单位为秒。  .</p>
<h4>服务调用</h4>
<p>服务消费者在获取服务清单后，通过服务名可以获得具体提供服务的实例名和该实例 的元数据信息。因为有这些服务实例的详细信息，所以客户端可以根据自己的需要决定具体调用哪个实例，在Ribbon中会默认采用轮询的方式进行调用，从而实现客户端的负载均<br>
衡。<br>
对于访问实例的选择，Eureka中有Region和Zone的概念，一个Region中可以包含多个 Zone，每个服务客户端需要被注册到一个Zone中，所以每个客户端对应一个Region和一个 Zone。在进行服务调用的时候，优先访问同处一个Zone中的服务提供方，若访问不到，就访问其他的Zone。</p>
<h4>服务下线</h4>
<p>在系统运行过程中必然会面临关闭或重启服务的某个实例的情况，在服务关闭期间， 我们自然不希望客户端会继续调用关闭了的实例。所以在客户端程序中，当服务实例进行正常的关闭操作时，它会触发一个服务下线的REST请求给Eureka Server,告诉服务注册 中心：“我要下线了”。服务端在接收到请求之后，将该服务状态置为下线（DOWN)，并把该下线事件传播出去。</p>
<h3>服务注册中心</h3>
<h4>失效剔除</h4>
<p>有些时候，我们的服务实例并不一定会正常下线，可能由于内存溢出、网络故障等原因使得服务不能正常工作，而服务注册中心并未收到“服务下线”的请求。为了从服务列表中将这些无法提供服务的实例剔除，Eureka Server在启动的时候会创建一个定时任务， 默认每隔一段时间（默认为60秒）将当前清单中超时（默认为90秒）没有续约的服务剔除出去。</p>
<h4>自我保护</h4>
<p>当我们在本地调试基于Eureka的程序时，基本上都会碰到这样一个问题，在服务注册 中心的信息面板中出现类似下面的红色警告信息：</p>
<pre><code data-language="xml">EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.
</code></pre><p>实际上，该警告就是触发了 Eureka Server的自我保护机制。服务注册到Eureka Server之后，会维护一个心跳连接，告诉Eureka Server自己还活着。Eureka Server在运行期间，会统计心跳失败的比例在15分钟之内是否低于85%,如果出现低于的情况(在单机调试的时候很容易满足，实际在生产环境上通常是由于网络不稳定导致），Eureka Server会将当前的实例注册信息保护起来，让这些实例不会过期，尽可能保护这些注册信息。但是，在这段保护期间内实例若出现问题，那么客户端很容易拿到实际己经不存在的服务实例，会出现调用失败的情况，所以客户端必须要有容错机制，比如可以使用请求重试、断路器等机制。<br>
由于本地调试很容易触发注册中心的保护机制，这会使得注册中心维护的服务实例不那么准确。所以，我们在本地进行开发的时候，可以使用eureka.server.enable- self-preservation=false参数来关闭保护机制，以确保注册中心可以将不可用的实 例正确剔除。 </p>

                      </div>

          
          <div class="_article-float hidden-xs" data-fixed="false">
            <div class="container">
              <div class="_article-float-inner">
                <aside class="_article-side">
    <a href="/articles/87342/follow" class="btn btn-primary btn-block" role="button" rel="nofollow" data-remote="true" data-method="post" data-done="$(this).parent().replaceWith(r)">关注</a>
    <section class="_article-followers">
    <h6 class="heading text-center">0人关注该文章</h6>
    <ul class="list-inline">
          </ul>
  </section>
</aside>
              </div>
            </div>
          </div>

          <footer class="actions">
            <a href="#comment" class="btn btn-primary js-scroll-to">评论文章
                              (0)
                          </a>
            <a href="/articles/87342/voteup" title="赞" class="btn btn-default" role="button" rel="nofollow" data-remote="true" data-method="post" data-done="$(this).next().addBack().toggle();$('#voters').html(r)" >
              <span class="glyphicon glyphicon-thumbs-up"></span>
              0
            </a>
            <a href="/articles/87342/unvoteup" title="取消赞" class="btn btn-primary" role="button" rel="nofollow" data-remote="true" data-method="post" data-done="$(this).prev().addBack().toggle();$('#voters').html(r)" style="display:none">
              <span class="glyphicon glyphicon-thumbs-up"></span>
              1
            </a>
                          <button class="btn btn-default" data-toggle="modal" data-target="#modal-share" data-article-id="87342" data-share-count-target="#share-count" title="分享">
                <span class="glyphicon glyphicon-share"></span>
                <span id="share-count">0</span>
              </button>
                          
            <!-- 分享到内外工作圈 -->
            <button class="btn btn-default" data-toggle="modal" data-target="#modal-aliwork-share" data-article-id="87342" title="分享到内外工作圈">
              <span class="glyphicon glyphicon-share-alt"></span>
            </button>

            <!-- 收藏 -->
            <span id="tools_favor_footer"></span>
          </footer>
        </article>

        <hr>


        <div id="voters" js-load-url="/articles/87342/voters"></div>

                <div js-load-url="/articles/87342/similar"></div>

                  <ul class="pager _article-pager">
                          <li class="previous cnzz_block" cnzz_event="上一篇"><a href="/articles/86382">上一篇：中间件学习笔记之分布式事务TXC</a></li>
                                  </ul>
          <hr>
        
        <section class="comments-box clearfix">
          <div class="media-list comments" id="comments" js-load-url="/articles/87342/comments">评论正在加载...</div>

          <form target="_self" accept-charset="UTF-8" action="/comments" method="POST" data-remote="true" data-target="#comments" class="js-comment-create js-active-on-valid">
            <input type="hidden" name="type" value="article">
            <input type="hidden" name="isCheck" value="1">
            <input type="hidden" name="pid" value="87342">
            <div class="form-group">
              <div class="editor js-editor">
                <div class="editor-container">
                  <div class="editor-actions">
                    <button type="button" class="btn btn-xs editor-action js-editor-fullscreen" data-original-title="全屏">
                      <span class="glyphicon glyphicon-fullscreen"></span>
                    </button>
                    <button type="button" class="btn btn-xs editor-action js-editor-preview" data-original-title="预览">
                      <span class="glyphicon glyphicon-eye-open"></span>
                    </button>
                    <button type="button" class="btn btn-xs editor-action js-editor-upload" data-original-title="插入图片">
                      <span class="glyphicon glyphicon-picture"></span>
                    </button>
                  </div><!-- /.editor-actions -->
                  <textarea name="content" rows="4" class="form-control js-mention js-auto-expand editor-content" id="comment" placeholder="写下你的评论…" required></textarea>
                </div><!-- /.editor-container -->

                <div class="editor-preview-container">
                  <div class="editor-actions">
                    <button type="button" class="btn btn-xs editor-action js-editor-preview-off" data-original-title="取消预览">
                      <span class="glyphicon glyphicon-eye-close"></span>
                    </button>
                  </div><!-- /.editor-actions -->
                  <div class="editor-preview content unsafe" data-disable-with="<span class=&quot;text-muted&quot;>loading...</span>"></div>
                </div><!-- /.editor-preview-container -->
              </div><!-- /.editor  -->
            </div>
            <div class="form-group text-right">
              <button type="submit" class="btn btn-primary" disabled>评论</button>
            </div>
          </form>
        </section><!-- /.comments -->

      </div><!-- /._article-box -->
    </div>
  </div><!-- /.container -->
        <footer class="footer" role="contentinfo">
  <div class="container">
	  <p class="copyright pull-left">
	  	© 2017 阿里巴巴集团 版权所有 Copyright© 17 ATA. All rights reserved.
      <script type="text/javascript">var cnzz_protocol = (("https:" == document.location.protocol) ? " https://" : " http://");document.write(unescape("%3Cspan id='cnzz_stat_icon_1254194462'%3E%3C/span%3E%3Cscript src='" + cnzz_protocol + "s11.cnzz.com/stat.php%3Fid%3D1254194462%26show%3Dpic' type='text/javascript'%3E%3C/script%3E"));</script>	  </p>
    <ul class="list-inline pull-right">
      <li><a href="/articles/67826" target="_blank">ATA社区常见问题汇总</a></li>
    </ul>
  </div>
</footer>
<script>require('module/global')</script>
<script>
  require('module/jquery.lazyload');
  $('img[data-original]').lazyload({effect: 'fadeIn'});
</script>
    <div id="modal-followers" class="modal fade" role="dialog" aria-labelledby="followers" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
        <h4 class="modal-title">关注者</h4>
      </div>
      <div class="modal-body"></div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">取消</button>
        <button type="submit" class="btn btn-primary">分享</button>
      </div>
    </div>
  </div>
</div><!-- /#modal-share -->  <div id="modal-share" class="modal fade" role="dialog" aria-labelledby="share" aria-hidden="true">
  <div class="modal-dialog">
    <form target="_self" action="/articles/#{id}/share" method="POST" data-remote="true" data-done="$(this).closest('.modal').modal('hide');$('#share-group-or-team').selectize({plugins: ['remove_button']})[0].selectize.clear();$($(this).data('shareCountTarget')).inc();$.alert('分享成功', 'success')" class="js-active-on-valid" novalidate>
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
        <h4 class="modal-title">分享</h4>
      </div>
      <div class="modal-body">
                                    <div class="form-group">
            <label for="share-group">分享到我的团队/圈子</label>
            a
            <select name="share-group-or-team[]" id="share-group-or-team" class="form-control" autocomplete="off" placeholder="Groups or Teams" required="1" multiple>
                                    <option value="">请选择</option>
                                                                                                        <optgroup label="&nbsp;集团客户体验事业群-产品研发团队">
                                                                    <option value="tid_115_cid_531">&nbsp;&nbsp;&nbsp;&nbsp;集团客户体验事业群-产品研发团队 - 线上问题排查</option>
                                                                    <option value="tid_115_cid_532">&nbsp;&nbsp;&nbsp;&nbsp;集团客户体验事业群-产品研发团队 - 新人总结</option>
                                                                    <option value="tid_115_cid_546">&nbsp;&nbsp;&nbsp;&nbsp;集团客户体验事业群-产品研发团队 - 网络通讯</option>
                                                                    <option value="tid_115_cid_547">&nbsp;&nbsp;&nbsp;&nbsp;集团客户体验事业群-产品研发团队 - 经验贴</option>
                                                                    <option value="tid_115_cid_1171">&nbsp;&nbsp;&nbsp;&nbsp;集团客户体验事业群-产品研发团队 - 体验改进</option>
                                                                </optgroup>
                                                                                            
                                            <optgroup label="&nbsp;圈子">
                                                    <option value="gid_1376">猿来如此</option>
                                                </optgroup>
                                                </select>
        </div>
        <div class="form-group">
          <label for="share-users">分享给同事</label>
          <input type="text" name="users" class="form-control js-user-complete" id="share-users" placeholder="Users" required="1">
        </div>
        <div class="form-group">
          <textarea name="content" rows="3" class="form-control" placeholder="羞羞的内容"></textarea>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">取消</button>
        <button type="submit" class="btn btn-primary" disabled>分享</button>
      </div>
    </div>
    </form>
  </div>
</div><!-- /#modal-share -->
<script>require('modal-share')</script>
<script>
$('#share-group-or-team').selectize({
    plugins: ['remove_button']
})[0].selectize.clear();
</script>
  <div id="modal-aliwork-share" class="modal fade" role="dialog" aria-labelledby="share" aria-hidden="true">
  <div class="modal-dialog">
    <form target="_self" action="/articles/87342/sharetoaliwork" method="POST" data-remote="true" data-done="$(this).closest('.modal').modal('hide');$('#share-group-or-team').selectize({plugins: ['remove_button']})[0].selectize.clear();$($(this).data('shareCountTarget')).inc();$.alert('分享成功', 'success')" class="js-active-on-valid" novalidate>
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
          <h4 class="modal-title">分享到内外工作圈</h4>
        </div>
        <div class="modal-body">
          <div class="form-group">
            <label for="share-group">Spring Cloud Eureka实现微服务治理</label>
          </div>
          <div class="form-group share-detail">
            <div class="img-thumb"><img class="share-img" src="" width="60px" height="60px"></div>
            <div class="content-brief">背景
Spring Cloud Eureka 是 Spring Cloud Netflix 微服务套件中的一部分，它基于 Netflix Eureka做了二次封装，主要负责完成微服务架构中的服务治理功</div>
          </div>
          <div class="form-group">
            <textarea name="content" rows="3" class="form-control" placeholder="感谢你的分享！ 想对同学们说点什么？"></textarea>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-default" data-dismiss="modal">取消</button>
          <button type="submit" class="btn btn-primary" disabled>分享</button>
        </div>
      </div>
    </form>
  </div>
</div><!-- /#modal-share -->
<script>require('modal-share')</script>
<script>
  $(document).ready(function(){
     var imgSrc = $("._article img").attr("src");
     if (imgSrc != undefined) {
       $("#modal-aliwork-share .share-img").prop("src", imgSrc);
     }
  });
</script>
  <div id="modal-tag-history" class="modal fade" role="dialog" aria-labelledby="tag history" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
        <h4 class="modal-title">标签历史</h4>
      </div>
      <div class="modal-body">      
        <p class="text-muted">Loading...</p>
      </div>
    </div>
  </div>
</div><!-- /#modal-share -->
  <div id="modal-article-modify-history" class="modal fade" role="dialog" aria-labelledby="article modify history" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
        <h4 class="modal-title">编辑历史</h4>
      </div>
      <div class="modal-body">      
        <p class="text-muted">Loading...</p>
      </div>
    </div>
  </div>
</div><!-- /#modal-share -->
  
  <!-- markModal -->
  <div class="modal fade" id="my-mark-modal" tabindex="-1" role="dialog" aria-labelledby="myMarkModal" aria-hidden="true"></div>

      <link rel="stylesheet" href="/vender/lib-video-player/video.css">
    <script src="/vender/lib-video-player/bundle.js"></script>
  
  <script data-group-id="1983" data-user-id="239455">require('module/mention')</script>  <script data-group-id="1983" data-user-id="239455">require('module/user-complete')</script>  <script>require('module/tools-favor')</script>
  <script>require('module/tag-complete')</script>
  <script>require('module/editor')</script>
  <script>require('module/mark');</script>
  <script>hljs.initHighlightingOnLoad();</script>
  <script src='/assets/swfobject_modified.js'></script>
  <script>
    // 替换文章详情页图片地址避免出现下载
    $(document).on('click', '._article .content.unsafe a, comments-box.clearfix .content.unsafe a', function(e){
      var img = $(this).children('img');
      if(img.attr('src') != $(this).attr('href') || !img.attr('src').match(/[0-9a-z]{32}$/)) {
        return true;
      }
      $(this).attr('href', '/public/imageView?src=' + img.attr('src'));
      return true;
    });

    $('.js-video').on('click', function () {
      if ($('#taobao-online-video').length > 0) {
        swfobject.registerObject("taobao-online-video");
      }
    })

    $(function () {
      $('body').append('<div id="article-outline"><div style="overflow: hidden;"><button type="button" class="close" data-dismiss="modal" aria-hidden="true">×</button></div></div><style>#article-outline{display:none;min-width:140p;max-width: 200px; max-height: 450px;position:absolute;top:150px;left:-999px;border:1px solid #ccc;box-shadow:5px 5px 2px #ccc;padding: 5px 10px;background-color: #fff;overflow-y: auto;scroll-x: auto;}#article-outline ul{margin:5px 0 5px 0;padding-left:30px;font-size:12px;border-left:1px dotted #ccc;}#article-outline ul:first-child{padding-left:15px;border:none;}#article-outline li{list-style-type:decimal;margin:3px 0;}</style>')
      var jOutline = $('#article-outline');

      var jContent = $('article .content');
      var jContentHeight = jContent.height();
      jOutline.find('.close').on('click', function () {
        jOutline.hide();
      })

      updateOutline();

      var top = jOutline.offset().top;
      scrollOutline();
      $(window).on('scroll', scrollOutline);
      function scrollOutline() {
        var scrollTop = $(document).scrollTop();
        var maxTop = jContent.offset().top + jContent.height() - jOutline.height();
        if (scrollTop >= top && scrollTop <= maxTop) {
          jOutline.css({'position': 'fixed', 'top': 0});
        }
        else if (scrollTop < top) {
          jOutline.css({'position': 'absolute', 'top': '150px'});
        }
        else {
          jOutline.css({'position': 'absolute', 'top': maxTop + 'px'});
        }

      }

      function updateOutline() {
        var arrAllHeader = jContent.find("h1,h2,h3,h4,h5,h6");
        var arrOutline = ['<ul>'];
        var header, headerText;
        var id = 0;
        var level = 0,
            lastLevel = 1;
        var levelCount = 0;
        for (var i = 0, c = arrAllHeader.length; i < c; i++) {
          header = arrAllHeader[i];
          headerText = $(header).text();

          header.setAttribute('id', id);

          level = header.tagName.match(/^h(\d)$/i)[1];
          levelCount = level - lastLevel;

          if (levelCount > 0) {
            for (var j = 0; j < levelCount; j++) {
              arrOutline.push('<ul>');
            }
          } else if (levelCount < 0) {
            levelCount *= -1;
            for (var j = 0; j < levelCount; j++) {
              arrOutline.push('</ul>');
            }
          }
          ;
          arrOutline.push('<li>');
          arrOutline.push('<a target="_self" href="#' + id + '">' + headerText + '</a>');
          arrOutline.push('</li>');
          lastLevel = level;

          var html = '<a target="_self" class="article-anchor" href="#'+$(header).prop("id")+'" ><svg class="octicon octicon-link ancher-link" height="16" version="1.1" viewBox="0 0 16 16" width="16">' +
                     '<path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"/></svg>'+
                     '</a>' +
                     $(header).html();
          $(header).html(html);
          id++;
        }
        arrOutline.push('</ul>')
        if (arrOutline.length > 2) {
          jOutline.append(arrOutline.join(''));
          jOutline.find('ul').each(function (i, n) {
            var jThis = $(this);
            if (jThis.children('li').length === 0) {
              jThis.replaceWith(jThis.children());
            }
          });
          showOutline();
        }
        else {
          hideOutline();
        }
      }

      // 设置锚点
      $("h1,h2,h3,h4,h5").hover(function(){
          $(this).find(".ancher-link").css({"visibility": "visible"});
        }, function(){
          $(this).find(".ancher-link").css({"visibility": "hidden"});
      });
      $("h1 .article-anchor,h2 .article-anchor,h3 .article-anchor,h4 .article-anchor,h5 .article-anchor").hover(function(){
        $(this).find(".ancher-link").css({"visibility": "visible"});
      }, function(){
        $(this).find(".ancher-link").css({"visibility": "hidden"});
      });
      function showOutline() {
        var offset = jContent.offset();

        jOutline.css({
          left: offset.left + jContent.width() + 10 + 'px',
        }).show();
      }

      function hideOutline() {
        jOutline.hide();
      }
    });
  </script>
  <script src="//cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_CHTML"></script>
  <script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    jax: ["input/TeX","output/HTML-CSS"],
    tex2jax: {
      inlineMath: [ ['\\$','\\$'], ["\\(","\\)"], ['$', '$'] ],
      displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
      processEscapes: true,
      ignoreClass:"title"
    },
    TeX: {
      equationNumbers: { autoNumber: "AMS" }
    },
    displayAlign: "center"
  });
  </script>
<a target="_self" href="#" class="go-top" style="display:none"><span class="glyphicon glyphicon-arrow-up"></span></a>
<script>require('module/go-top')</script>  <a href="https://work.alibaba-inc.com/work/group/8401" target="_blank" title="为ATA2.0提供反馈" style="position:fixed;right:0;bottom:0px;" class="hidden-xs">
  <img src="http://gtms04.alicdn.com/tps/i4/T17D1tFrVeXXcUfo_w-30-100.png" alt="">
</a>


<script type="text/javascript">
  // CNZZ事件统计
    var _czc = _czc || [];
  $(document).on('click', '.cnzz_block a', function (e) {
      var me = $(this),
          parent = me.parents('.cnzz_block');
      var cnzz_category = $('body').attr('cnzz_category'),
          cnzz_event = parent.attr('cnzz_event'),
          cnzz_label = '',
          cnzz_value = '', // me.attr('href').replace(/^.*\/(\d+?)[^\d]*$/, '$1'),
          cnzz_nodeid = parent.attr('id') || '';

      _czc.push(["_trackEvent", cnzz_category, cnzz_event, cnzz_label, cnzz_value, cnzz_nodeid]);

      // 给cnzz留点时间, 防止数据丢失
      var t, target = me.attr('target'), h = me.attr('href');
      if (!target && $('head base[target]').length == 0 || target == '_self') {
          if (h) {
              t = setTimeout(function () {
                  clearTimeout(t);
                  return true;
              }, 300);
              return true;
          }
      }

      return true;
  });
  
  $('.form-submit-btn').click(function () {
      var form = $(this).parents('form');
      form.find('input').focus();
      form.submit();
  });
  //实习生提醒
  $('#intern-sure').click(function () {
    // var cookie_key = 'sfb';
    // $('.safepop_mask,.safepop_wrap').remove();
    // setcookie(cookie_key, 1, getTodaySeconsLeft());
    window.location.href="/news/";
  });
  // 导航 dropdown
  (function () {
      var t;
      $('.dropdown').hover(function () {
          var that = $(this);
          that.siblings().find('.dropdown-menu').hide();
          that.children('.dropdown-menu').show();
          clearTimeout(t);
      }, function () {
          var that = $(this);
          t = setTimeout(function () {
              that.children('.dropdown-menu').hide();
          }, 500)
      })
  }());
</script>
</body>
</html>
