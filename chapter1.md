# 第一个目标

_分布式节点下应用日志统一收集至日志服务器下log文件。_

#### 关键技术

_logstash、kafka_

#### 实现步骤

_1.搭建日志服务器_

此处省略

2.日志服务器安装kafka

2.1下载 kafka官网：[http:\/\/kafka.apache.org\/quickstart](http://kafka.apache.org/quickstart)

`> tar -xzf kafka\_2.11-0.10.1.0.tgz`

`> cd kafka\_2.11-0.10.1.0`

2.2 启动服务

启动zookeeper

`> bin/zookeeper-server-start.sh config/zookeeper.properties`

启动kafka

`> bin/kafka-server-start.sh config/server.properties`

3.日志服务器安装logstash

3.1下载logstash：https:\/\/www.elastic.co\/downloads\/logstash

3.2 配置文件logstash-kafka-input.conf
<pre>
input {
  kafka {
    bootstrap_servers => "localhost:9092"
    topics => ["log-test"]
 }
}

output {
  file {
    path => "/home/admin/tools/logs/kafaka-log.log"
  }
}
</pre>

3.3 启动logstash

bin\/logstash -f logstash-kafka-input.conf

4.应用服务器安装logstash

4.1下载logstash：https:\/\/www.elastic.co\/downloads\/logstash

4.2 配置文件logstash-kafka-output.conf
<pre>
input { 
  file {
    path => "/usr/local/webserver/tengine/logs/*.log"
  }
}

output {  
  kafka {    
    bootstrap_servers => "localhost:9092"    
    topic_id => "log-test"    
    codec => plain {      
      format => "%{message}"   
    } 
  }
}
</pre>

4.3 启动logstash

bin\/logstash -f logstash-kafka-output.conf

5.系统验证

ssh连接日志服务器，tail -f kafaka-log.log,访问测试应用，查看日志输出情况

