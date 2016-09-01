# 图像相关的接口一览
================

***所有的接口访问全部在/api/ 之下，以下所有接口请自动补全/api/。比如：wordFlag, 调用完整地址应该是http://host/api/wordFlag***


## 0.to all vision api
如何请求：
***大多数请求只支持post，get方式请求会明文告知，如无注明默认post请求***
例如：精确分词请求
```
http post http://host/api/FaceRecognition Authorization:'Bearer access_token'
```

Headers 内需填上token的 Bearer 认证信息，例如：
```
Bearer cd04d8cafcc8b0bc0d7e47a2fdc3155f783cdff10f36f70e7793947e2fcfxxx
```

## 图片特殊之处：
### 如何上传图片：
在body中装载图片，图片格式必须为base64编码，请将图片大小控制在150k以内，否则计算复杂度太高。
```
img='data:image/jpeg:base64,xxxxxxxxxx'
```

* [图像请求例子](/example/vision_request_example/)


## 1.人脸识别
### 请求路径 FaceRecognition/
### request：post body
### response:
```javascript
{
  "FaceRecognition": {
    "face0": [      //识别到第一张脸
      280,         //x1
      122,          //y1
      137,          //x2
      137           //y2
    ],
    "face1": [      //识别到第二张脸
      67,
      120,
      151,
      151
    ]
  }
}
```
```javascript
    "face0": [      //识别到第一张脸
      280,         //x1
      122,          //y1
      137,          //x2
      137           //y2
    ],
```
x1,y1 是第一个点的坐标，x2,y2是第二个点的坐标,两个点确定一个框，也就是人脸位置

## 2.图片分类
### 请求路径 ImageClassification/
### request：post body
### response:
```javascript
{
  "ImageClassification": {
    "terrorist": 0.8655983805656433,        //恐怖分子，权重0.8655983805656433
    "other": 0.13433212041854858,
    "political": 0.00006675633630948141,
    "sexy": 0.0000023590039290866116,
    "adult": 4.3015668893531256e-7
  }
}
```


## 3.多物体识别（logo识别）
### 请求路径 ObjectRecognition/
### request：post body
### response:
```javascript
{
  "ObjectRecognition": [
    {
      "yili": {         //物体分类名称
        "bbox": [
          127,          //x1
          38,           //y1
          188,          //x2
          76            //y2
        ],
        "score": "0.999999"
      }
    },
    {
      "yili": {
        "bbox": [
          54,
          75,
          93,
          101
        ],
        "score": "0.994103"
      }
    }
  ]
}
```
```javascript
        "bbox": [       //物体位置
          127,          //x1
          38,           //y1
          188,          //x2
          76            //y2
        ],
```
x1,y1 是第一个点的坐标，x2,y2是第二个点的坐标，两个点确定一个框，也就是物体位置