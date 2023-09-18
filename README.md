# 发现
Hunau的有线校园网是可以通过两种方式认证登录，一种是连上网线后使用网页认证（具体网址不记得了，但是应该打开浏览器随便进一个网页会自己跳转），另一种是通过锐捷客户端进行认证。某天我像以往一样打开电脑点开锐捷客户端连上校园网，可是我使用浏览器进入b站时却显示无网络，于是我在锐捷客户端尝试重新连接，是可以显示认证成功的，再次使用浏览器进入b站同样显示无网络。于是我尝试使用网页认证校园网（此时并未退出锐捷客户端的认证），网页显示认证成功，同时锐捷客户端被网页认证挤下来，此时使用浏览器打开网站有网络。我便以为是锐捷客户端的问题，我在b站上看了一个视频大概五分钟的样子，当看完出来时发现点击其他视频时又显示没网络。我再次进入网页认证的网页看到显示认证成功（但此时没网），我尝试网页认证断线重连，依然时没网。我便又打开锐捷客户端进行认证，此时显示认证成功的网页认证并挤下，神奇的事情发生了，又有网了。感到很奇怪，经过多次的尝试后，发现朋友的校园网其实那天正好过期了，但仍然可以通过两种认证方式（我本人的校园网无法做到，尝试认证会显示已过期类似的提示），只是即使认证成功也没用网络，但是如果使用任一种方式认证后，用另一种认证方式挤下前一种方式后会获得五分钟的网络使用时间。**所以如果你的校园网过期后还能认证通过，认证通过后页面可用时长显示不限。** 我们便可以利用上文所说的机制进行对校园网进行白嫖。
# 原理
如上所述，使用任一种方式认证后，用另一种认证方式挤下前一种方式后会获得五分钟的网络使用时间。使用脚本进行校园网网页认证，每隔3-5分钟发送一次请求，同时配合上具有被挤后能够自动重连的功能的校园网认证软件（mentohust），就能实现白嫖校园网。测速是100Mbps，比花钱买的网的速度还快。。只是每隔五分钟（取决于你脚本设置的时间间隔）会断一下网，用来下载、看视频直播没啥问题，有用过来玩LOL，每隔五分钟会掉1，2秒的样子，除非你正好在打团，不然基本没影响，玩玩匹配可以。

**MacOS mentohust**：https://github.com/vincenttsang/mentohust-macos

**linux mentohust**：https://www.bilibili.com/video/BV1bW411r7MP/ 参考这个视频

**windows**：https://www.bilibili.com/video/BV1Vt411U7Sp/ 参考这个视频
## 网页认证脚本

```python
import requests
import time

loginUrl = 'http://10.100.0.12:9090/zportal/login/do'

# 具体参数使用chrome 认证前按F12 认证后在Network里面查看替换
data = {
    'qrCodeId': '',
    'username': '',    # 即学号
    'pwd': '',            # 即密码
    'validCode': '',
    'validCodeFlag': 'false',
    'ssid': '',
    'mac': '',
    't': "wireless-v2",
    'wlanacname': '00aab905808bf54238202dd3074e226b',
    'url': '56fbdadc6ac57d97d64a3ecaeb68d21c65c4cd695485e63d98f45ecb3d1cc835',
    'nasip': '3a55a6e233ce66a3e3c9d19c2572b2ea',
    'wlanuserip': '0decdfb4b564c03bcf770570aebe4146',
}

while True:
    response = requests.post(url=loginUrl, data=data)

    # 验证是否登录成功
    print(response.text)
    # 结果为Json数据{"message":"","nextPage":"goToAuthResult","result":"success"}

    # 休眠五分钟
    time.sleep(180)
```
