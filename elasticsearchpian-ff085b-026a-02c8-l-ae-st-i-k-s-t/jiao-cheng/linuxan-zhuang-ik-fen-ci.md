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

2018-12-11T22:49:42,426][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [aggs-matrix-stats]
[2018-12-11T22:49:42,461][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [analysis-common]
[2018-12-11T22:49:42,462][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [ingest-common]
[2018-12-11T22:49:42,462][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [lang-expression]
[2018-12-11T22:49:42,462][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [lang-mustache]
[2018-12-11T22:49:42,462][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [lang-painless]
[2018-12-11T22:49:42,463][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [mapper-extras]
[2018-12-11T22:49:42,463][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [parent-join]
[2018-12-11T22:49:42,463][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [percolator]
[2018-12-11T22:49:42,463][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [rank-eval]
[2018-12-11T22:49:42,464][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [reindex]
[2018-12-11T22:49:42,464][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [repository-url]
[2018-12-11T22:49:42,464][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [transport-netty4]
[2018-12-11T22:49:42,464][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [tribe]
[2018-12-11T22:49:42,465][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-ccr]
[2018-12-11T22:49:42,465][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-core]
[2018-12-11T22:49:42,465][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-deprecation]
[2018-12-11T22:49:42,465][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-graph]
[2018-12-11T22:49:42,465][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-logstash]
[2018-12-11T22:49:42,466][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-ml]
[2018-12-11T22:49:42,466][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-monitoring]
[2018-12-11T22:49:42,466][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-rollup]
[2018-12-11T22:49:42,466][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-security]
[2018-12-11T22:49:42,466][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-sql]
[2018-12-11T22:49:42,467][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-upgrade]
[2018-12-11T22:49:42,467][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded module [x-pack-watcher]
[2018-12-11T22:49:42,467][INFO ][o.e.p.PluginsService     ] [uFHKjzU] loaded plugin [analysis-ik]
```

看到loaded plugin analysis-ik 代表加载成功

再通过访问查看

![](/assets/ik-001.png)

参考：

https://www.cnblogs.com/hanyinglong/p/5409003.html

http://www.cnblogs.com/xing901022/p/5910139.html





