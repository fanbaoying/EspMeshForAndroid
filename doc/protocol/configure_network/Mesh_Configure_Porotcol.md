# Mesh BLE 配网协议
协议基于 Blufi  

## 1. 设置 OP mode
- Ctrl 包, subtype 为 0x02
- 数据为 {0x01}, 即 Station Mode

## 2. 发送配置信息
- Data 包, subtype 为 0x09
- 数据使用 TLV 格式, 其中 Type 占一个字节, Length 占一个字节
    - SSID:
        - Type 为 0x01
        - 路由器的 SSID 数据
        - 必需
    - Password:
        -  Type 为 0x02
        -  路由器的密码数据
        -  非必需
    - MeshID:
        - Type 为 0x03
        - 长度为 6 的随机数据
        - 必需
    - WhiteList:
        - Type 为 0x05
        - 允许接入此 Mesh 网络的设备 Mac 地址数据
            - 每个地址为 6 个字节的二进制数据
            - 多个地址将字节数组拼接起来即可
        - 因为长度字节只占一个字节，只能表示最大长度255，所以当地址个数超过 42 个时，后续的地址需要重新组一个 TLV 数据拼接在后面
        - 非必需
- 例  
```  
假设 SSID 为 "abc", 转为字节数组为 {0x61, 0x62, 0x63}  
假设 Password 为 "111111", 转为字节数组为 {0x31, 0x31, 0x31, 0x31, 0x31, 0x31}  
假设 MeshID 为 {0xa0, 0xa1, 0xa2, 0xa3, 0xa4, 0xa5}  
假设 WhitList 地址有两个 "11:22:33:44:55:66", "aa:bb:cc:dd:ee:ff", 转为字节数组为 {0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0xaa, 0xbb, 0xcc, 0xdd, 0xee, 0xff}  
  
组成配网数据 {0x01, 0x03, 0x61, 0x62, 0x63, 0x02, 0x06, 0x31, 0x31, 0x31, 0x31, 0x31, 0x31, 0x03, 0x06, 0xa0, 0xa1, 0xa2, 0xa3, 0xa4, 0xa5, 0x05, 0x0c, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0xaa, 0xbb, 0xcc, 0xdd, 0xee, 0xff}  
```

