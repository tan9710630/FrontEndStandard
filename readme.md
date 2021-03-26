# 讯视前端规范项目

 <p align="left">
    <img src="https://img.shields.io/badge/GitLab%20Pages-10.0.2-red.svg" />
     <img src="https://img.shields.io/badge/GitLab%20CI-10.0.2-red.svg" />
     <img src="https://img.shields.io/badge/docsify-10.0.2-green.svg" />
 </p>  

该文档项目基于`Docsify`实现（[参考文档](https://docsify.js.org/#/?id=docsify)），选择它的原因无非是简单简直不要太好用，你去试试`gitbook`就知道什么是绝望了，反正效果也还可以就是不很有所谓。通过`Docsify`完成`markdown`文档转为静态网页进行查看，一方面提供了完整导航目录，另一方面加入了很多wiki特性，当然我们这里使用的算多，但是整体效果还是相当不错。

`HTML`文件静态部署本来想在`K8S`上面去搭建一个`Nginx`实现的，但是转念一想就几个静态文件而已又不需要什么后端还单独去搞一个容器跑着实是有点亏，所以寻找了一下其他方案，最终发现`GitLab`的`pages`可以完美符合我们的要求。

目前我们公司的`Gitlab`已经配置了域名和对应的解析到服务器上面，同时我也重新配置了`Gitlab`并且创建一个独立的`runner`来进行静态文件的发布，按照`Docsify`的要求进行`CI/CD`配置，配置文件`.gitlab-ci.yml`如下：

```yml
pages: # GitLab的CI/CD模板
  stage: deploy # 直接部署
  script:
  - mkdir .public
  - cp -r * .public
  - mv .public public # 这里是为了解决循环发布的问题
  artifacts:
    paths:
    - public # 定义pages主页
  only: # 肯定是master提交之后再发布
  - master
  tags:
  - Document #指定这个runnder进行发布
```

上面这些是为了其他人员使用pages时我留下的文档，这个项目的目的还是定义一套完整的前端规范出来，最终发布的规范文档地址是：http://document.pages.saylooks.com/FrontEndStandard