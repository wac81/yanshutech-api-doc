# 机器人相关的接口一览
================

***所有的接口访问全部在/api/ 之下，以下所有接口请自动补全/api/。比如：wordFlag, 调用完整地址应该是http://host/api/wordFlag***


## 0.to all text api
如何请求：
***大多数请求只支持post，get方式请求会明文告知，如无注明默认post请求***
例如：精确分词请求
posturl = http://acnlp.com/api/exactCut
将在body内写入参数nl："请求的字符串"
装载后向 posturl 发送请求。

Headers 内需填上token的 Bearer 认证信息，例如：Bearer cd04d8cafcc8b0bc0d7e47a2fdc3155f783cdff10f36f70e7793947e2fcfxxx