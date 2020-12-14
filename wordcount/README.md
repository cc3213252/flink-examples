## 工程创建

curl https://flink.apache.org/q/quickstart.sh | bash  
pom文件均参考quickstart工程



## 问题处理

【ok】本地运行时报找不到类
quickstart工程pom中依赖类scope设置成了provided，运行时会找不到，去掉即可