# linux安装ik分词

[linux系统安装es、head、kibana插件](/elasticsearchpian-ff085b-026a-02c8-l-ae-st-i-k-s-t/jiao-cheng/linuxxi-tong-an-zhuang.md)

### 官网近期版本详情

```
https://github.com/medcl/elasticsearch-analysis-ik/releases/
```

### 下载到本机

```
https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.5.2/elasticsearch-analysis-ik-6.5.2.zip
```

### 因为其源码使用的maven开发，故而使用maven编译项目

![](/assets/ik.png)

使用maven package 命令打包后，将target目录下，releases中的zip，传递到linux服务器中，我这里是两个版本，只需要传一个就行，传递与服务器匹配的版本，可以通过POM文件，版本号修改，打包不同的版本

### Elasticsearch的安装的plugin下创建文件夹ik

```
[root@localhost bin]# cd /home/elk/elasticsearch-6.5.2/plugins/
[root@localhost plugins]# mkdir ik
```

### 进入ik目录，把zip文件传递到该目录下并解压

```
unzip elasticsearch-analysis-ik-6.5.2.zip 
```

重启elasticsearch

```
./elasticsearch &
```



