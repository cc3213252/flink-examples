## 工程创建

curl https://flink.apache.org/q/quickstart.sh | bash  
pom文件均参考quickstart工程

wordcount代码参考flink源码examples，源码由于scala和java混在一起，和其他工程也混在一起，故pom文件特别复杂  
主程序中由于支持控制台输入，代码变复杂，做了精简

## 部署

mvn clean package

## 问题处理

【ok】本地运行时报找不到类
quickstart工程pom中依赖类scope设置成了provided，运行时会找不到，去掉即可

【ok】ERROR StatusLogger No log4j2 configuration file found
log4j2只支持xml和json两种格式的配置，所以配置log4j.properties是没有作用的

【ok】[ERROR] Failed to execute goal org.apache.maven.plugins:maven-shade-plugin:3.1.1:shade (default) on project wordcount: Unable to parse configuration of mojo org.apache.maven.plugins:maven-shade-plugin:3.1.1:shade for parameter resource: Cannot find 'resource' in class org.apache.maven.plugins.shade.resource.ManifestResourceTransformer -> [Help 1]
quickstart使用shade-plugin这个插件打包，其实springboot自带打包插件更好用

【ok】org.apache.flink.streaming.runtime.tasks.StreamTaskException: Could not instantiate outputs in order
Caused by: java.lang.ClassCastException: cannot assign instance of java.lang.invoke.SerializedLambda to field
这个问题在stackoverflow上看到，意思是不要去怀疑程序问题，肯定是打包的问题  
其实就是前面一个问题用了错误的方法解决，引起了这个问题
这个问题从flink界面上，submit new job上能看到就是Entry class出错了  
springboot框架有自己的entry class，需要配置修改，看https://stackoverflow.com/questions/49215416/maven-shade-plugin-cannot-find-resource-in-class-org-apache-maven-plugins-sh
最根本就是不要用springboot框架
