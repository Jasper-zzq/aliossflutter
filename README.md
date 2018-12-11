# aliossflutter

example里的配置文件自行添加配置
```dart
class Config{
 static final  String stsserver="";
 static final String endpoint="";
 static final String cryptkey="";
 static final String bucket="";
 static final String key="";
}
 ```

阿里云oss
初始化一次就可以了，可以在main.dart里初始化，插件会自动管理sts token过期    
后台返回sts格式：
```
{
 "StatusCode": 200,
 "AccessKeyId":"STS.iA645eTOXEqP3cg3VeHf",
 "AccessKeySecret":"rV3VQrpFQ4BsyHSAvi5NVLpPIVffDJv4LojUBZCf",
 "Expiration":"2015-11-03T09:52:59Z",
 "SecurityToken":"CAES7QIIARKAAZPlqaN9ILiQZPS+JDkS/GSZN45RLx4YS/p3OgaUC+oJl3XSlbJ7StKpQ...."
}
```
加密sts返回格式：
```
{
 "Data": "3des加密后的 sts"
}
```
初始化
```dart
import 'package:aliossflutter/aliossflutter.dart';
AliOSSFlutter  alioss=AliOSSFlutter();
alioss.init("sts url", "http://oss-cn-hangzhou.aliyuncs.com");
//可选一种 sts token 3DES加密方式
//alioss.init("sts url", "http://oss-cn-hangzhou.aliyuncs.com",cryptkey:"key");
//监听初始化
alioss.responseFromInit.listen((data){
      if(data) { 
          _msg="初始化成功"; 
      }else{
        _msg="初始化失败";
      }
    });
 ```
上传
```dart
AliOSSFlutter  alioss=AliOSSFlutter();
alioss.upload("bucket", file.path, "key");
//监听上传
alioss.responseFromUpload.listen((data) {
      if(data.success) {
        setState(() {
          _msg="上传成功 key:"+data.key;
        });
      }else{
        _msg="上传失败";
      }
    });
 ```
下载
```dart
AliOSSFlutter  alioss=AliOSSFlutter();
alioss.download(bucket, key, path,{process = ""});
//监听下载回调
alioss.responseFromDownload.listen((data) {
      if(data.success) {
        setState(() {
          _path=data.path;
          _msg="下载成功："+_path;
        });
      }else{
        _msg="下载失败";
      }
    });
url签名：
```dart
//type=1 签名私有资源
//type=0 签名公开的访问URL
alioss.signUrl(bucket, key,{type = "0",interval = "1800",process = ""})

//监听url签名
alioss.responseFromSign.listen((data){
      if(data.success) {
        setState(() {
          _msg="url 签名 ："+data.url;
        });
      }else{
        _msg="url 签名失败";
      }
    });
```

 ```
 监听进度
```dart
alioss.responseFromProgress.listen((data){
 setState(() {
  _progress=data.getProgress();
 });
});
  ```
## Getting Started

For help getting started with Flutter, view our online
[documentation](https://flutter.io/).

For help on editing plugin code, view the [documentation](https://flutter.io/developing-packages/#edit-plugin-package).
