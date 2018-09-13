# 已知BUG或问题 可能已经修复
 - aos 1.3.3
    * 云端抓去tsl，json数组类型的数据下发后无法进入回调函数
    * 云端抓去tsl，在tsl还没有抓取完成时无法上报数据Invalid Parameter。接受应该也不会进回调（未验证）。