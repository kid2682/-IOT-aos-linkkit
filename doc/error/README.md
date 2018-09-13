# 常见错误
 - ssl错误
    * -30848 -0x7880 云端断开连接
        * 两台设备使用同一设备信息，导致互踢。
        * 设备连接过于平凡，云端限流。关闭设备，等待一段时间后再发起连接（5分钟以后）
        * 设备保活出错，心跳爆没有及时发送
        
    * -30720 -0x7800 验证失败
        * 证书问题
        * ssl域名校验错误
        
    * -29184 -0x7200 数据接收出错
        * tcp/ip数据包出错
        * ssl内存配置太小
            * ssl.h 修改 MBEDTLS_SSL_MAX_CONTENT_LEN 宏定义。
            * 大小需要按照自己的业务配置来。多出现于从云端拉去tsl
    
    * -82 -0x0052 dns解析失败
        * 网络问题
    
    * -67 -0x0043 ssl内存太小
        * 参考 -29184 错误
        
    * -66 -0x0042 创建socket失败
        * socket已全部使用。检查socket相关的内存泄漏
        
    * -68 -0x0044 连接失败
        * 检查url和port是否正确
        
 - mqtt错误
    * -35 设备信息错误
        * 设备信息是否正确（四元祖/三元祖）
        * 检查服务器连接（国内/国际） 
        * 设备是否被禁用
        
    * -37 云端拒绝连接请求
        * 创建设备的账号是否有效
        
    * -42 订阅/发布太多太快，导致队列满了
        * 如果是订阅过程中发生的错误，说明当前订阅队列太小。请调整IOTX_MC_SUB_NUM_MAX的大小
        * 如果是发布的时候发生的错误，可能是频率太快，或者网络状态不佳。可以考虑使用QoS0来发布，或者调整IOTX_MC_REPUB_NUM_MAX的大小
    
    * subscribe fail topic定义过长
        * 调整IOTX_MC_TOPIC_NAME_MAX_LEN
        
    * NOT more @sub_handle space! 订阅太多，队列满了
        * 请调整IOTX_MC_SUB_NUM_MAX的大小
        
    * mqtt read buffer is too short, mqttReadBufLen : xxxx, remainDataLen : yyyy
        * aos 1.3.3/linkkit2.2.0 开始是修改config.rhino.make文件中的CONFIG_MQTT_TX_MAXLEN和CONFIG_MQTT_RX_MAXLEN。rhino是当前系统的文件。sdk可以找自己
        * aos 1.3.3/linkkit2.2.0 之前的。修改iotx_cloud_conn_mqtt.c 中的 MQTT_MSGLEN宏
        * 多出现于抓去云端tsl
        
 - 其他err
    * Invalid Parameter 
        * 上报时参数名写错。
        * 云端抓取tsl（aos 1.3.3)解析错误。使用本地tsl
    * ret_code = 6289 (!=200)
        * 多出现于动态注册（一型一密）。
        * 设备已经注册过了。云端后台，删除这个设备。
        