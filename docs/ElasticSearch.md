# ElasticSearch

> 安装

```shell
# 安装 ElasticSearch
docker run --name elasticsearch -p 9200:9200 \
-p 9300:9300 \
-e "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms128m -Xmx128m" \
-v /data/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /data/elasticsearch/data:/usr/share/elasticsearch/data \
-v /data/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-itd elasticsearch:7.6.1

# 安装 Kibana
docker run -d \
--name=kibana \
--restart=always \
-p 5601:5601 \
-v /data/kibana/config/kibana.yml:/usr/share/kibana/config/kibana.yml \
  kibana:7.6.1
```

> elasticsearch.yml

```yaml
http.host: 0.0.0.0
http.cors.allow-origin: "*"
http.cors.enabled: true
```

> kibana.yml

```yaml
server.name: kibana
server.host: "0.0.0.0"
# ElasticSearch地址
elasticsearch.hosts: [ "http://192.168.169.130:9200" ]
i18n.locale: "zh-CN"
```

> ElasticSearch 是面向文档的 关系型数据库与 ElasticSearch 客观对比

|  Relational DB   | ElasticSearch  |
|  :----:  | :----:  |
| 数据库(database)  | 索引(index) |
| 表(table)  | 类型(type) |
| 行(rows)  | 文档(documents) |
| 字段(columns) | 分片(fields) |

> IK 分词器

> 安装

```text
https://github.com/medcl/elasticsearch-analysis-ik
```

> 文件导入 ElasticSearch 插件目录 命名为ik


> 在 Kibana 控制台测试分词器

```text
GET _analyze
{
  "analyzer": "ik_smart",
  "text": "中国共产党"
}
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "中国共产党"
}
```

> 问题来了 当我最小粒拆分`超级喜欢薛之谦唱演员` 薛之谦被拆开了 这种情况我们需要把这些词加入到分词的字典中即可

```text
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "超级喜欢薛之谦唱演员"
}
```

```json
{
  "tokens": [
    {
      "token": "超级",
      "start_offset": 0,
      "end_offset": 2,
      "type": "CN_WORD",
      "position": 0
    },
    {
      "token": "喜欢",
      "start_offset": 2,
      "end_offset": 4,
      "type": "CN_WORD",
      "position": 1
    },
    {
      "token": "薛",
      "start_offset": 4,
      "end_offset": 5,
      "type": "CN_CHAR",
      "position": 2
    },
    {
      "token": "之",
      "start_offset": 5,
      "end_offset": 6,
      "type": "CN_CHAR",
      "position": 3
    },
    {
      "token": "谦",
      "start_offset": 6,
      "end_offset": 7,
      "type": "CN_CHAR",
      "position": 4
    },
    {
      "token": "唱",
      "start_offset": 7,
      "end_offset": 8,
      "type": "CN_CHAR",
      "position": 5
    },
    {
      "token": "演员",
      "start_offset": 8,
      "end_offset": 10,
      "type": "CN_WORD",
      "position": 6
    }
  ]
}

```

> 新建分词器字典 加入到分词器配置文件中

[![IY3Hn1.png](https://z3.ax1x.com/2021/11/09/IY3Hn1.png)](https://imgtu.com/i/IY3Hn1)
[![IY8Z9g.png](https://z3.ax1x.com/2021/11/09/IY8Z9g.png)](https://imgtu.com/i/IY8Z9g)

> 重启 ElasticSearch 效果如下

[![IY8QH0.png](https://z3.ax1x.com/2021/11/09/IY8QH0.png)](https://imgtu.com/i/IY8QH0)

> 常用操作

[![IY884U.png](https://z3.ax1x.com/2021/11/09/IY884U.png)](https://imgtu.com/i/IY884U)