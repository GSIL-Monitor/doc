 
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Spring Cloud的微服务组件</title>
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
            <img           src="http://img3.tbcdn.cn/L1/461/1/u_identicon_21e3421b828551f99142364273136912.png_40x40" width="20" height="20"
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
              <a href="/users/267310" class="info" title="风则"><img           src="/identicon/user/267310_80x80" width="32" height="32"
   alt="风则" class="img-user"><span>&nbsp;风则</span></a>                            <span class="info">
                              <time class="" datetime="2017-02-13T23:14:03+08:00">2017-02-13 23:14:03</time>
                                  (最初创作于: 2017-02-12 19:44:49)
                              </span>
                            
              <div class="pull-right">
                <span class="info">212  阅读</span>
                <span id="tools_favor_top" class="article-favor">
                  <a data-toggle="modal" href="/articles/71754/mark/" data-target="#my-mark-modal" title="收藏" class="btn btn-default item-tomark" role="button" rel="nofollow" >
                    3&nbsp;<span>收藏</span>
                  </a>
                  <a href="/articles/71754/unmark" title="取消收藏" class="btn btn-primary item-tounmark" role="button" rel="nofollow" data-remote="true" data-done="$(this).prev().addBack().toggle()" data-method="post" style="display:none">
                    4&nbsp;<span>取消收藏</span>
                  </a>
                </span>
                
                              </div>
            </div>

            
            <div class="labels">
  <div class="knowledge-tags">
    <span class="k-span">知识体系：</span>
        <button href="/knowledge/knowledgeTags?type=show&aid=71754" class="btn btn-default btn-xs kbutton" data-toggle="modal" data-target="#editKnowledgeTagModal">
      <span class="glyphicon glyphicon-pencil"></span>
      修改知识体系
    </button>
  </div>
</div>

  <div class="modal fade" id="editKnowledgeTagModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true"></div>

            <div class="labels">
    <form target="_self" accept-charset="UTF-8" action="/articles/71754/tags" autocomplete="off" method="post"
    data-remote="true"
    data-done="$(this).closest('.labels').replaceWith(r);$.alert('修改成功', 'success')"
    style="display:none">
    <input type="hidden" name="_method" value="put">
    <div class="form-group">
      <input type="text" name="tags" class="form-control js-tag-complete" value="">
    </div>
    <div class="form-group text-right">
      <button type="button" class="btn btn-default btn-sm" onclick="$(this).closest('.labels').children().toggle()">取消</button>
      <button type="submit" class="btn btn-primary btn-sm">更新</button>
    </div>
  </form>
  
  <div>
    <span>文章标签：</span>
              <button type="button" id="edit-tags" class="btn btn-default btn-xs" onclick="$(this).closest('.labels').children().toggle()">
        <span class="glyphicon glyphicon-pencil"></span>
        修改标签
      </button>
      <a href="/articles/71754/tags/history" class="btn btn-default btn-xs" data-toggle="modal" data-target="#modal-tag-history">
        <span class="glyphicon glyphicon-time"></span>
        标签历史
      </a>
      </div>
</div>

                          <div class="labels">
                <span class="span-weight">附加属性：</span>
                                                  <span class="special-attr special-yuan">作者原创</span>
                                                                                                              </div>
            
            
                      </header>

            <div class="article-title">
              <h1 class="title clearfix unsafe">
                Spring Cloud的微服务组件
                              </h1>
            </div>

          <div class="content unsafe" style="background: rgba(0, 0, 0, 0) url(/public/watermark.png) repeat scroll 0 0">
                          <h4><strong>下面的内容还得再做些整理，暂时先发布上来了</strong>。</h4>
<p>&emsp;前些天在小组里面做了次分享，效果不是很好，想再用文字记录下。</p>
<h3>一、什么是Spring Cloud</h3>
<p>&emsp;官网的原话是：Spring Cloud provides tools for developers to quickly build some of the common patterns in distributed systems。<br>
&emsp;上面描述的工具集包含：分布式配置管理, 服务注册与发现, 断路器（熔断器）, 智能路由, 微代理， 控制总线, 全局锁, 决策竞选，集群状态管理。。。。。。 <br>
&emsp;利用上面的组件，已经可以构建一个简单的微服务系统了。</p>
<h3>二、Spring Cloud的微服务组件</h3>
<p>&emsp;&emsp;下面我简单的介绍部分组件。</p>
<h4>1. Spring Cloud的服务注册与发现</h4>
<p>&emsp;&emsp;Spring Cloud支持Zookeeper、Consul、Eureka作为服务注册中心，后面我以Eureka作为案例介绍。<br>
&emsp;&emsp;<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7cf8fe555c4fa292af5196c903ba3e31.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7cf8fe555c4fa292af5196c903ba3e31.png" alt="eureka.png" title="eureka.png"></a><br>
&emsp;&emsp;Eureka Server集群实现高可用注册中心，Eureka Client用于服务的自动发现和注册。图中的Application Service通过Eureka Client注册到us-east-1c(Eureka Server)，us-east-1c配置复制到us-east-1d，再复制到us-east-1e，如此三个区域内的客户端都能获取到Application Service。</p>
<h5>1.1 Spring Cloud服务注册中心</h5>
<p>&emsp;&emsp;&emsp;pom文件中引入依赖：<br>
&emsp;&emsp;&emsp;<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0b61ecbe7b4a6db10f8283311639e1e8.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0b61ecbe7b4a6db10f8283311639e1e8.png" alt="11.png" title="11.png"></a><br>
&emsp;&emsp;&emsp;设置Eureka Server发布端口：<br>
&emsp;&emsp;&emsp;<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/2e323bb348dafec93b971115c7d60297.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/2e323bb348dafec93b971115c7d60297.png" alt="113.png" title="113.png"></a><br>
&emsp;&emsp;&emsp;设置启动类：<br>
&emsp;&emsp;&emsp;<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/82de75ca9357447fafb48adc3e343b97.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/82de75ca9357447fafb48adc3e343b97.png" alt="112.png" title="112.png"></a></p>
<p>&emsp;&emsp;<strong>通过@EnableEurekaServer注解，标识需要开启Eureka Server，运行EurekaServerStarter后可以打开<a href="http://localhost:8000">http://localhost:8000</a>：</strong><br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/190601fc916de93c597d78e3630bbea2.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/190601fc916de93c597d78e3630bbea2.png" alt="118.png" title="118.png"></a></p>
<h5>1.2 Spring Cloud服务注册</h5>
<p>&emsp;&emsp;Spring Cloud的服务基于Rest API，将普通的基于Spring MVC构建的rest接口，能通过简单的配置部署为Spring Cloud的服务。<br>
&emsp;&emsp;比如下面为基于Spring boot的rest接口：<br>
&emsp;&emsp;ComputeController.java:<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/b01dba74b932ba0b11ffb4b3ae4c5af3.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/b01dba74b932ba0b11ffb4b3ae4c5af3.png" alt="114.png" title="114.png"></a><br>
&emsp;&emsp;ServiceStarter.java:<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1624d8a70a59e86582df7536916bca89.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1624d8a70a59e86582df7536916bca89.png" alt="115.png" title="115.png"></a><br>
&emsp;&emsp;application.properties:<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/4fbba7170b052fbc3e929c248e9df340.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/4fbba7170b052fbc3e929c248e9df340.png" alt="116.png" title="116.png"></a><br>
&emsp;&emsp;pom中新增依赖：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/718dbd49efcc67164dab450120c1e6f6.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/718dbd49efcc67164dab450120c1e6f6.png" alt="117.png" title="117.png"></a></p>
<p>&emsp;&emsp;<strong>@EnableDiscoveryClient注解，自动开启服务的自动发现和注册功能，扫描目录下所有的rest接口，自动被注册到注册中心，可以打开<a href="http://localhost:8000">http://localhost:8000</a>看到该服务已经被成功注册</strong>：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/78a4c2eb998ca3084fd94a172bf7955a.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/78a4c2eb998ca3084fd94a172bf7955a.png" alt="119.png" title="119.png"></a></p>
<h5>1.3 Spring Cloud服务发现</h5>
<p>使用Eureka Server注册的服务，可以通过客户端自动发现，此外还需要配合上Ribbon实现的负载均衡，而Feign是Ribbon的基础上，实现webservice的调用方式。</p>
<h6>1.3.1 Ribbon</h6>
<p>&emsp;&emsp;首先，需要在pom中引入依赖<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7531fa89130ce6f470026e3c671cdbe6.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7531fa89130ce6f470026e3c671cdbe6.png" alt="屏幕快照 2017-02-13 下午6.43.26.png" title="屏幕快照 2017-02-13 下午6.43.26.png"></a><br>
&emsp;&emsp;&emsp;&emsp;比服务端的增加一个spring-cloud-starter-ribbon依赖。<br>
&emsp;&emsp;接着，开启自动发现@EnableDiscoveryClient，实例化RestTemplate，并配置负载均衡<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/acd32d501fc1fc1b9e3fbec4bc9be5e6.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/acd32d501fc1fc1b9e3fbec4bc9be5e6.png" alt="屏幕快照 2017-02-13 下午6.45.55.png" title="屏幕快照 2017-02-13 下午6.45.55.png"></a><br>
&emsp;&emsp;&emsp;&emsp;RestTemplate是Spring Web项目里面HttpClient的封装，解决模板方法。另外上面增加了@LoadBalanced注解，并开启了客户端的负载均衡，当然均衡策略也可以配置。<br>
&emsp;&emsp;通过上面的配置，我们的客户端自动发现和负载均衡就实现了，最后就是调用：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7273575fb0691786a6583db47b5452e5.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7273575fb0691786a6583db47b5452e5.png" alt="屏幕快照 2017-02-13 下午6.50.56.png" title="屏幕快照 2017-02-13 下午6.50.56.png"></a><br>
&emsp;&emsp;&emsp;&emsp;此处我写了一个rest 接口（/add），restTemplate call的url被替换成了服务名，请求这个接口的时候，会调用服务compute-service，实现非常简单。<br>
&emsp;&emsp;再来看看Eureka Server，我们发现Ribbon的客户端也被注册到了Eureka Server，所有上面写的服务消费端，也是服务的提供方。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/039600e051dfd5553ee68f06160f4592.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/039600e051dfd5553ee68f06160f4592.png" alt="屏幕快照 2017-02-13 下午6.59.55.png" title="屏幕快照 2017-02-13 下午6.59.55.png"></a></p>
<h6>1.3.2 Feign</h6>
<p>&emsp;&emsp;Feign的开发思路和普通的webservice调用比较类似，需要引入一个二方库。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/209a090ce36ff0d46899234c86611213.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/209a090ce36ff0d46899234c86611213.png" alt="屏幕快照 2017-02-13 下午7.07.31.png" title="屏幕快照 2017-02-13 下午7.07.31.png"></a><br>
&emsp;&emsp;上面的@FeignClient(&quot;compute-service&quot;)，标识这是代理着Eureka Server上面注册的服务。<br>
&emsp;&emsp;pom文件中需要增加一个spring-cloud-starter-feign的依赖：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/e05fedb40b59a612723a192649063548.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/e05fedb40b59a612723a192649063548.png" alt="屏幕快照 2017-02-13 下午7.09.58.png" title="屏幕快照 2017-02-13 下午7.09.58.png"></a><br>
&emsp;&emsp;Starter中开启Feign客户端自动发现：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/ff74e020f13fee5738cd896ea692743f.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/ff74e020f13fee5738cd896ea692743f.png" alt="屏幕快照 2017-02-13 下午7.10.44.png" title="屏幕快照 2017-02-13 下午7.10.44.png"></a><br>
&emsp;&emsp;使用：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/43cbeb12ae96b5db9d007ac5946c59f2.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/43cbeb12ae96b5db9d007ac5946c59f2.png" alt="屏幕快照 2017-02-13 下午7.11.35.png" title="屏幕快照 2017-02-13 下午7.11.35.png"></a></p>
<h4>2. Spring Cloud的分布式配置管理</h4>
<p>&emsp;&emsp;Spring Cloud支持分布式配置方案，包括Spring Cloud Config、Spring Cloud Zookeeper等，我介绍下Spring Cloud Config。Spring Cloud Config和Diamond很类似，实现了对配置的持久化，持久层采用Git、SVN、本地文件等方式。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/67d9cf271b14dbafb0d36540a9177a46.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/67d9cf271b14dbafb0d36540a9177a46.png" alt="aa.png" title="aa.png"></a><br>
&emsp;&emsp;通过一个案例介绍下上面结构的实现。</p>
<h4>步骤1，构建git目录结构：</h4>
<p>我在oschina git上构建了一个仓库<a href="https://git.oschina.net/84649162/ws.spring.cloud.git">https://git.oschina.net/84649162/ws.spring.cloud.git</a><br>
git的目录结构为：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1a17847b7e3f4c244d3ae329166f7615.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1a17847b7e3f4c244d3ae329166f7615.png" alt="屏幕快照 2017-02-13 下午7.24.28.png" title="屏幕快照 2017-02-13 下午7.24.28.png"></a></p>
<p>上面configuration目录下面的4个文件的前缀都是config-server，后面为不同的环境加了后缀。</p>
<h4>步骤2，发布一个config server：</h4>
<p>Spring Cloud Config Server是spring cloud config的服务端，我们也可以通过简单的配置实现发布。</p>
<h5>步骤2.1，配置pom文件：</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/4495a51138aab0897cccbfb9865fb5b1.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/4495a51138aab0897cccbfb9865fb5b1.png" alt="屏幕快照 2017-02-13 下午7.30.01.png" title="屏幕快照 2017-02-13 下午7.30.01.png"></a></p>
<h5>步骤2.2，配置application.properties</h5>
<p>配置config server的发布端口为7001，application name为config-server，此处要和git库内的文件前缀对上：<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/39ff6011cc245511e08e8a99f96f74e1.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/39ff6011cc245511e08e8a99f96f74e1.png" alt="屏幕快照 2017-02-13 下午7.30.14.png" title="屏幕快照 2017-02-13 下午7.30.14.png"></a></p>
<h5>步骤2.3，编写starter文件，并启动：</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/4fc04e9a07cdd1daa71067eda00d682a.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/4fc04e9a07cdd1daa71067eda00d682a.png" alt="屏幕快照 2017-02-13 下午7.30.32.png" title="屏幕快照 2017-02-13 下午7.30.32.png"></a></p>
<h4>步骤3，客户端获取配置：</h4>
<h5>步骤3.1，配置pom文件：</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0619384cc23ad98ceecd58b26d2dd6d2.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0619384cc23ad98ceecd58b26d2dd6d2.png" alt="屏幕快照 2017-02-13 下午7.35.52.png" title="屏幕快照 2017-02-13 下午7.35.52.png"></a></p>
<h5>步骤3.2，配置bootstrap.properties</h5>
<p>下面的图中<br>
master是git上的branch，我们可以为不同的branch配置不同的配置项，实现branch维度的隔离。<br>
profile为配置文件的后缀，我们可以为不同的环境配置不同的配置文件，实现环境维度的隔离。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0baaa23c3feb44906ec282be64c3044d.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0baaa23c3feb44906ec282be64c3044d.png" alt="屏幕快照 2017-02-13 下午7.36.14.png" title="屏幕快照 2017-02-13 下午7.36.14.png"></a></p>
<h5>步骤3.3，使用：</h5>
<p>Spring Cloud兼容PropertySource，environment，降低旧代码的迁移成本。案例中我使用@Value注解<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/b0b9df0d121c47f97d1c7efc4e22bee3.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/b0b9df0d121c47f97d1c7efc4e22bee3.png" alt="屏幕快照 2017-02-13 下午7.44.45.png" title="屏幕快照 2017-02-13 下午7.44.45.png"></a><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/a8e4c2b77cae87198086d44c5970558c.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/a8e4c2b77cae87198086d44c5970558c.png" alt="屏幕快照 2017-02-13 下午7.44.52.png" title="屏幕快照 2017-02-13 下午7.44.52.png"></a></p>
<h5>Spring Cloud Config的单机版实现很容易，同样，我们可以为git仓库配置多个Spring Cloud Config Server，加上负载均衡以实现高可用。我们也可以直接将Config-Server 发布为微服务，发布方式和上面介绍的compute-service一样，然后借助Ribbon实现LB。</h5>
<h5>Spring Cloud Config的一个问题是配置项在git上变更后，客户端无法自动感知，需要借助Spring Cloud Bus以实现异步的刷新配置。</h5>
<h4>3. Spring Cloud的断路器组件</h4>
<p>&emsp;&emsp;在云或分布式系统环境中，任何对一致性或可靠性的表述就是谎言。我们必须假设微服务的行为或其服务器位置会经常变动，其结果就是组件有时会提供低质量服务甚至可能彻底无法提供服务。这些微服务的故障如果没有处理好，将导致整个系统的故障。<br>
&emsp;&emsp;断路器是常用的分布式容错的方案。<br>
<a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/fd0a0235cde990f0f0599390b3e7a359.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/fd0a0235cde990f0f0599390b3e7a359.png" alt="断路器.png" title="断路器.png"></a></p>
<h5>当服务正常调用时，断路器处于‘Close’状态，当服务的调用超过预先设置好的阈值（超时，异常，失败次数等），断路器将切换成‘Open’状态，而新的请求将fast failing，等到过了预先设定等待时间后，将切换到‘Half-Open’状态，下一次请求过来的时候，会再做一次尝试，如果成功调用，将切换回‘Close’状态，如果继续失败，断路器保存到‘Open’状态，等到设定的等待时间到达，而这期间服务将fast failing。</h5>
<h5>Spring Cloud的断路器实现依赖Netflix Hystrix。</h5>
<p>以上面的Ribbon的客服端调用例子进行改造：</p>
<h5>步骤1. pom文件改造：</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/f723897650214edc94753f6a2d74428c.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/f723897650214edc94753f6a2d74428c.png" alt="屏幕快照 2017-02-13 下午10.44.50.png" title="屏幕快照 2017-02-13 下午10.44.50.png"></a><br>
新增spring-cloud-starter-hystrix。</p>
<h5>步骤2. 开启断路器</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/ff59fbab3bcabeff265206af99bc54c3.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/ff59fbab3bcabeff265206af99bc54c3.png" alt="屏幕快照 2017-02-13 下午10.46.07.png" title="屏幕快照 2017-02-13 下午10.46.07.png"></a></p>
<h5>步骤3. 配置hystrix规则</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0cd570b78b3c4430ca7eb44d72966751.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/0cd570b78b3c4430ca7eb44d72966751.png" alt="屏幕快照 2017-02-13 下午10.36.13.png" title="屏幕快照 2017-02-13 下午10.36.13.png"></a><br>
为了提高失败的概率，上面的例子中设置一次调用内，失败一次就开启断路器，断路器设置的休眠时间为50s，在开启断路器的时间里，服务将快快速失败，直接调用addServiceFallback()。50s后会尝试一次服务调用。<br>
（更多Hystrix配置项定义可以参照<a href="https://github.com/Netflix/Hystrix/wiki/Configuration%E3%80%82%EF%BC%89">https://github.com/Netflix/Hystrix/wiki/Configuration。）</a></p>
<h4>4. Spring Cloud的服务网关</h4>
<p>网关封装了系统内部架构，为每个客户端提供一个定制的API，可以具有其它职责，如身份验证、监控、负载均衡、缓存等。Spring Cloud中Zuul提供网关的能力，可以接入Spring Cloud Security用于身份验证。</p>
<h5>步骤1. 假定已经发布了俩个服务service-A和service-B</h5>
<h5>步骤2. pom引入依赖</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/9e0552760ed929607c7077eda1011419.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/9e0552760ed929607c7077eda1011419.png" alt="屏幕快照 2017-02-13 下午11.02.26.png" title="屏幕快照 2017-02-13 下午11.02.26.png"></a></p>
<h5>步骤3. 开启Zuul</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/22699129586675a240a5574ed0521224.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/22699129586675a240a5574ed0521224.png" alt="屏幕快照 2017-02-13 下午11.03.06.png" title="屏幕快照 2017-02-13 下午11.03.06.png"></a></p>
<h5>步骤4. 配置application.properties</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/80481bd73122d7ee8be0fe02b60e26e3.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/80481bd73122d7ee8be0fe02b60e26e3.png" alt="屏幕快照 2017-02-13 下午11.04.09.png" title="屏幕快照 2017-02-13 下午11.04.09.png"></a><br>
上面的配置表示：<br>
用户请求/api-a/***地址的请求，会路由到service-A<br>
用户请求/api-b/***地址的请求，会路由到service-B</p>
<h5>步骤5. 配置ZuulFilter</h5>
<p><a href="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1188199f05e28aa5ea8394093bc8a311.png" target="_blank"><img src="http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/1188199f05e28aa5ea8394093bc8a311.png" alt="屏幕快照 2017-02-13 下午11.07.59.png" title="屏幕快照 2017-02-13 下午11.07.59.png"></a><br>
上面增加一个ZuulFilter，请求服务网关调用服务的时候，accessToken如果没有，认证失败。</p>

                      </div>

          
          <div class="_article-float hidden-xs" data-fixed="false">
            <div class="container">
              <div class="_article-float-inner">
                <aside class="_article-side">
    <a href="/articles/71754/follow" class="btn btn-primary btn-block" role="button" rel="nofollow" data-remote="true" data-method="post" data-done="$(this).parent().replaceWith(r)">关注</a>
    <section class="_article-followers">
    <h6 class="heading text-center">1人关注该文章</h6>
    <ul class="list-inline">
      <li><a href="/users/92131" class="trigger_avatar" avatar="http://img3.tbcdn.cn/L1/461/1/u_identicon_e5393badb97c8b7c4f313ee767537232.png_50x50">渡会</a></li>    </ul>
  </section>
</aside>
              </div>
            </div>
          </div>

          <footer class="actions">
            <a href="#comment" class="btn btn-primary js-scroll-to">评论文章
                              (0)
                          </a>
            <a href="/articles/71754/voteup" title="赞" class="btn btn-default" role="button" rel="nofollow" data-remote="true" data-method="post" data-done="$(this).next().addBack().toggle();$('#voters').html(r)" >
              <span class="glyphicon glyphicon-thumbs-up"></span>
              9
            </a>
            <a href="/articles/71754/unvoteup" title="取消赞" class="btn btn-primary" role="button" rel="nofollow" data-remote="true" data-method="post" data-done="$(this).prev().addBack().toggle();$('#voters').html(r)" style="display:none">
              <span class="glyphicon glyphicon-thumbs-up"></span>
              10
            </a>
                          <button class="btn btn-default" data-toggle="modal" data-target="#modal-share" data-article-id="71754" data-share-count-target="#share-count" title="分享">
                <span class="glyphicon glyphicon-share"></span>
                <span id="share-count">0</span>
              </button>
                          
            <!-- 分享到内外工作圈 -->
            <button class="btn btn-default" data-toggle="modal" data-target="#modal-aliwork-share" data-article-id="71754" title="分享到内外工作圈">
              <span class="glyphicon glyphicon-share-alt"></span>
            </button>

            <!-- 收藏 -->
            <span id="tools_favor_footer"></span>
          </footer>
        </article>

        <hr>


        <div id="voters" js-load-url="/articles/71754/voters"></div>

                <div js-load-url="/articles/71754/similar"></div>

                  <ul class="pager _article-pager">
                          <li class="previous cnzz_block" cnzz_event="上一篇"><a href="/articles/71028">上一篇：阿里妈妈-运营平台新人开发指引总结</a></li>
                                      <li class="next cnzz_block" cnzz_event="下一篇"><a href="/articles/72891">下一篇：初次接触ODPS总结</a></li>
                      </ul>
          <hr>
        
        <section class="comments-box clearfix">
          <div class="media-list comments" id="comments" js-load-url="/articles/71754/comments">评论正在加载...</div>

          <form target="_self" accept-charset="UTF-8" action="/comments" method="POST" data-remote="true" data-target="#comments" class="js-comment-create js-active-on-valid">
            <input type="hidden" name="type" value="article">
            <input type="hidden" name="isCheck" value="1">
            <input type="hidden" name="pid" value="71754">
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
    <form target="_self" action="/articles/71754/sharetoaliwork" method="POST" data-remote="true" data-done="$(this).closest('.modal').modal('hide');$('#share-group-or-team').selectize({plugins: ['remove_button']})[0].selectize.clear();$($(this).data('shareCountTarget')).inc();$.alert('分享成功', 'success')" class="js-active-on-valid" novalidate>
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
          <h4 class="modal-title">分享到内外工作圈</h4>
        </div>
        <div class="modal-body">
          <div class="form-group">
            <label for="share-group">Spring Cloud的微服务组件</label>
          </div>
          <div class="form-group share-detail">
            <div class="img-thumb"><img class="share-img" src="" width="60px" height="60px"></div>
            <div class="content-brief">下面的内容还得再做些整理，暂时先发布上来了。
&amp;emsp;前些天在小组里面做了次分享，效果不是很好，想再用文字记录下。
一、什么是Spring Cloud
&amp;emsp;官网的原话是：Spring Cl</div>
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
  
  <script data-group-id="" data-user-id="267310">require('module/mention')</script>  <script data-group-id="" data-user-id="267310">require('module/user-complete')</script>  <script>require('module/tools-favor')</script>
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
