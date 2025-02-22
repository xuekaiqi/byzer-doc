
## 接口权限控制

| 参数 | 说明 | 示例值 |
|----|----|-----|
|  spark.mlsql.auth.access_token  |  如果设置了，那么会开启token验证，任何访问引擎的接口都需要带上这个token才会被授权  | 默认不开启    |
|  spark.mlsql.auth.custom  | 设置接口访问授权的自定义实现类 |  默认无   |

用户可以将实现 `{def auth(params: Map[String, String]): (Boolean, String)` 的类使用 --jars 带上，然后通过 --conf spark.mlsql.auth.custom= YOUR CLASS NAME 来设置自定义的接口权限访问。

## 二层通讯参数

Kolo-lang 会在 Spark 之上构建一个二层通讯，方便driver直接控制executor.

| 参数 | 说明 | 示例值 |
|----|----|-----|
|  streaming.ps.cluster.enable  |  是否开启二层通讯  |  默认为true   |
|  spark.ps.cluster.driver.port  |  二层通讯driver端口 |  默认为7777   |
|  streaming.ps.ask.timeout |  通讯超时 |  默认为3600秒   |
|  streaming.ps.network.timeout |  通讯超时 |  默认为28800秒   |

## Hive支持参数

| 参数 | 说明 | 示例值 |
|----|----|-----|
| streaming.enableHiveSupport  |  是否开启hive支持  |  默认为false   |
|  streaming.hive.javax.jdo.option.ConnectionURL  | 用来配置hive.javax.jdo.option.ConnectionURL|  默认为空   |

## 自定义UDF jar包注册

如果我们将自己的UDF打包进Jar包里，我们需要在启动的时候告诉系统对应的UDF 类名称。
UDF的编写需要符合MLSQL的规范。我们推荐直接在Console里动态编写UDF/UDAF。

| 参数 | 说明 | 示例值 |
|----|----|-----|
| streaming.udf.clzznames  |  支持多个class,用逗号分隔  |     |

## 离线插件安装

确保插件的jar包都是用`--jars`带上。并且目前只支持app插件。

| 参数 | 说明 | 示例值 |
|----|----|-----|
| streaming.plugin.clzznames  |  支持多个class,用逗号分隔  |     |

可通过如下地址下载插件(填写插件名和版本)：

```
http://store.mlsql.tech/run?action=downloadPlugin&pluginType=MLSQL_PLUGIN&pluginName=mlsql-excel-2.4&version=0.1.0-SNAPSHOT
```

## Session设置

Kolo-lang 支持用户级别 Session, 请求级别 Session。每个 Session 相当于构建了一个沙盒，避免不同请求之间发生冲突。默认是用户级别 Session ,如果希望使用请求级别 Session ，需要在请求上带上 `sessionPerRequest` 参数。对此参看[Rest接口详解](http://docs.mlsql.tech/mlsql-engine/api/run-script.html)。


| 参数 | 说明 | 示例值 |
|----|----|-----|
| spark.mlsql.session.idle.timeout  |  session一直不使用的超时时间  |  30分钟   |
| spark.mlsql.session.check.interval  |  session超时检查周期  |  5分钟   |

## 分布式日志收集

Kolo-lang 支持将部分任务的日志发送到Driver。

| 参数 | 说明 | 示例值 |
|----|----|-----|
| streaming.executor.log.in.driver  |  是否在driver启动日志服务  | 默认为true|

## 权限校验

| 参数 | 说明 | 示例值 |
|----|----|-----|
| spark.mlsql.enable.runtime.directQuery.auth  |  开启directQuery语句运行时权限校验  | 默认为false|
| spark.mlsql.enable.runtime.select.auth  |  开启select语句运行时权限校验  | 默认为false|
| spark.mlsql.enable.datasource.rewrite  |  开启数据源加载时动态删除非授权列  | 默认为false|
| spark.mlsql.datasource.rewrite.implClass  |  设置自定义数据源列控制的实现类 | &nbsp;|
