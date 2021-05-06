## 安装NVM （node包管理器）
  * 从Github获取nvm最新版本自行安装：<https://github.com/coreybutler/nvm-windows/releases>
  * 设置淘宝镜像
  
    * 本地查找目录 C:\Users\callme\AppData\Roaming\nvm
    * 打开 settings.txt 文件 添加 
      ```javascript
        root: C:\Users\callme\AppData\Roaming\nvm
        path: C:\Program Files\nodejs

        node_mirror:npm.taobao.org/mirrors/node/
        npm_mirror:npm.taobao.org/mirrors/npm/
      ```
  * DOS命令窗口测试
    * nvm信息 
      ```javascript
       nvm 
      ```
    * nvm查看可用node版本
      ```javascript
       nvm list available
      ```
    * nvm 使用指定node版本
      ```javascript
       nvm use 版本号
      ```
    * 注意： 
      * 问题： nvm安装node版本时候，会有问题，不知道是不是nvm的问题。
      * 解决方式： 在node官网下载想要的node版本包，然后放在 C:\Users\callme\AppData\Roaming\nvm 文件夹下
    