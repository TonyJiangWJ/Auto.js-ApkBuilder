# Auto.js-ApkBuilder

- 一个把Auto.js的js文件"打包"成apk文件的项目。
- 原作者@hyb1996，现已删库。
- `apkbuilder` 模块为lib代码。可以打包成aar引入到本体，或者直接将源代码复制到AutoJS代码中，反正jitpack上也下载不到了
- `sample` 为打包 `Demo App` 可以不关注
- `app` 模块为实际插件app，将自己的AutoJS中的 `inrt` 模块编译后得到apk，将自己需要的对应平台apk放到 `app/src/main/assets/template.apk`
- 修改 `app/build.gradle` 中的签名证书文件信息和applicationId。然后将它打包为apk安装到手机，此时就可以使用AutoJS中的打包功能调用插件了。
- 单独打包app的命令是 `./gradlew build -p app`

## 原理

- 该项目中的"打包"并不是真正的打包，而是用一个事先编译好的apk文件，通过替换里面的js文件、修改应用包名、名称、版本等信息再重新签名实现。
- 即在AutoJS本体中，会调用插件代码功能，获取插件App的context得到assets目录下的template.apk,然后解压替换相应的js脚本内容，重新签名
- 默认情况下只支持v1签名，因此 `inrt` 的目标sdk版本不能高于29，否则打包出来的将无法安装(targetSdk30开始必须v2以上签名)
- 当然因为种种原因特征太明显，打包出来也还是会报病毒的，请自行解决
- 如果打包后安装报错，可以通过apksigner检查一下签名是否正确：
```shell
apksigner verify --verbose --print-certs 打包出来的.apk
```

* 版本(versionName), 版本号(versionCode), 名称(label): 通过魔改[pxb axml](https://github.com/wtchoi/axml)项目实现AXML的解析、修改、重新编译
* 包名(packageName): 包名需要修改resources.arsc的包名([ArscEditor](https://github.com/seaase/ArscEditor))和AndroidManifest中的包名
* 签名: [TinySign](https://code.google.com/archive/p/tiny-sign/downloads)

## 感谢

感谢[Apk编辑器](https://www.coolapk.com/apk/zhao.apkmodifier)的作者[seaase](https://github.com/seaase)的支持和帮助！！