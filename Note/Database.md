==MongoDB==
- 安装
    - ubuntu 安装
    ```
    home路径下 vi.bashrc
    export PATH=/usr/local/mongodb/bin:$PATH
    source ~/.bashrc
    ```
    - 创建data目录，接着创建db目录
    - 运行Mongodb服务器
    ```
    C:\mongodb\bin\mongod --dbpath c:\data\db
    或在bin路径下执行mongod
    ```
    - 连接Mongodb
    ```
    C:\mongodb\bin\mongo.exe
    ```
    - 后台管理shell
    ```
    bin\mongo
    ```

- 分片
    - 在data目录下创建文件夹

    ```mkdir data\shard\s0```
    -
    ```
    >mongod --port 27020 --dbpath=d:\data\shard\s0
    ```


- ubuntu 安装 sudo chmod -R go+w /data/db
```
#指定testdb分片生效 需要use admin
db.runCommand( { enablesharding :"testdb"});
#指定数据库里需要分片的集合和片键
use testdb
设置成id:1 递增插入，数据都变到一个shard里了
db.runCommand( { shardcollection : "testdb.table1",key : {id: hashed} } )

```