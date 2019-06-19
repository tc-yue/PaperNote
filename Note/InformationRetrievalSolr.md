[solr-zhihu](https://zhuanlan.zhihu.com/p/28855188)

```
solr start
solr create -c qa
solr restart -p 8983
```

- 上传xml\csv 文件:

```
java -Dc=qa -Dtype=text/xml -jar post.jar book.xml
java -Dc=corename -Dtype=text/xml -jar post.jar xxx.csv
```

- sql上传
    - 报错：
org.apache.solr.common.SolrException:org.apache.solr.common.SolrException: Error loading class 'solr.DataImportHandler'
    - 包未导入，加入路径到solrconfig.xml
    ```
    <lib dir="../../../contrib/dataimporthandler/lib" regex=".*\.jar" />
    <lib dir="../../../dist/"regex="solr-dataimporthandler-.*\.jar" />```

- Ik中文分词部署参考知乎
- 删除已建索引
```
<delete><query>*:*</query></delete>
<commit/>
```

- searching
    - 用户search，Request Handler（一个插件定义处理request的逻辑）处理query。query parser，默认是StandardQueryParser
    - query parser         包含搜索字符串微调参数增加某个特定字符串或者fields的权重通过采用布尔逻辑在搜索词之间或者排除搜索结果中的文本，控制query响应的表达参数，例如特定展示结果的顺序或限制响应到某个搜索域的特定领域。
    - search 参数也可以特定一个==filter query==







- schema
    - configsets/collection-name/conf/managed-schema
        - field guessing：solr索引的时候猜测field中数据的类型。会走动创建新的fields对于进入的文档中的新field。








报错：
org.apache.solr.common.SolrException:org.apache.solr.common.SolrException: Error loading class 'solr.DataImportHandler'
包未导入，加入路径到solrconfig.xml
<lib dir="../../../contrib/dataimporthandler/lib" regex=".*\.jar" />
 <lib dir="../../../dist/" regex="solr-dataimporthandler-.*\.jar" />

indexing too long
上传xml 文件，java -Dc=qa -Dtype=text/xml -jar post.jar book.xml




- ubuntu 命令
    - 启动 ./bin/solr start -e cloud
    - 删除 bin/solr delete -c techproducts
    - 创建 bin/solr create -c <name> -s 2 -rf 2
    - 停止 bin/solr stop -all
    - 添加索引 bin/post/ -c <collection name> example/filename.csv

- manage-schema
    - text_ws 以空格分割
    - 以分号分割
    ```
    <fieldType name="semicolonDelimited" class="solr.TextField">
  <analyzer type="query">
    <tokenizer class="solr.PatternTokenizerFactory" pattern="; "/>
  </analyzer>
  </fieldType>
  <field name="question" type="semicolonDelimited">
    ```
- Document,Fields,and Schema Design
    - Field Analysis 当创建索引时对输入数据进行分析预处理，包括小写化，去停用词等等。
    - manage-schema

        - <fieldType> 可以自定义域的tokenizer分析类型如中文分词等
        - <field> 定义域的类型，是否索引存储等。
        - text_ws 以空格分割
        - 以分号分割
    ```
    <fieldType name="semicolonDelimited" class="solr.TextField">
  <analyzer type="query">
    <tokenizer class="solr.PatternTokenizerFactory" pattern="; "/>
  </analyzer>
  </fieldType>
  <field name="question" type="semicolonDelimited">
    ```
        - cloud模式需要用ZooKeeper管理


- Analyzers
- By default, when you search for multiple terms and/or phrases in a single query, Solr will only require that one of them is present in order for a document to match. Documents containing more terms will be sorted higher in the results list.
- q.op=AND 查询url参数


- 10.8.128.42:4322
    - bin/solr stop -all
    - bin/solr restart -p 8983