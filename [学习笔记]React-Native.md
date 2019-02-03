## ios的rn的hello world

**react-native No bundle URL present解决办法**

- 当我直接输入命令react-native run-ios 出现了错误


- 解决办法
    最好在做之前先把ios/build文件夹先删除, 总共需要启动两个terminal 
    第一个窗口输入命令react-native start 
    第二个窗口输入命令react-native run-ios 


[react native http的图片在ios上不显示
](https://segmentfault.com/a/1190000002933776)



# 小年糕融合项目

路由访问这个: http://dev-wx.xiaoniangao.cn:9000/v3/?state=0_1#/discover/index

跑ios的时候:

0. cd /Users/Johnson/XNG_work/MIX-RN/xngAppBase/script => bash xng-init.sh
1. cd xngAppBase/ios
2. sudo gem install cocoapods
3.  git clone https://github.com/CocoaPods/Specs.git master(1个多G)
3. pod install(还要下一些包)

npm start,  启动个服务
xcode 添加项目 /Users/Johnson/XNG_work/MIX-RN/xngAppBase/ios/xiaoniangao.xcworkspace

运行

-----------------
提示缺包

./script/xng-init.sh  装自己的依赖库
启动服务  npm start
运行(报错后不能reload, 要点击左上角的播放按钮重启)

账号和密码都是test