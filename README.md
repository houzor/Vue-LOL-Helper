# Vue-LOL-Helper

基于Vue3+Vite+LCUAPI的英雄联盟小助手

## 软件界面

![](http://ru0opmihh.hn-bkt.clouddn.com/%E5%8A%9F%E8%83%BD%E5%88%97%E8%A1%A8.png)

## 功能列表

### 开启匹配房间

点击即可开匹配模式的房间

### 选人界面一键查询队友战绩

支持查询最近五把对局的KDA，计算胜率，牛马评判

![](http://ru0opmihh.hn-bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20230509152541.png)

![](http://ru0opmihh.hn-bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20230509152547.png)

### 更改段位

选择不同的游戏模式，不同段位，不同级别即可更改段位

![](http://ru0opmihh.hn-bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20230509152525.png)

### 更改游戏状态

可更改为在线，离线，游戏中，离开，手机在线状态，暂无图

### 更改生涯界面皮肤

输入六位编号即可切换生涯界面皮肤，前三位为英雄ID，后三位为皮肤ID，不满六位自动补全，如99007为拉克丝大天使的皮肤，13006为瑞兹至死不渝皮肤

![](http://ru0opmihh.hn-bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20230509152510.png)

### 自动接受对局

勾选即可自动接受对局，暂无截图

## 如何使用

### 必备环境

- Node.js

### 运行lol客户端

然后以管理员权限打开CMD终端，运行以下指令，在终端找到--app-port=XXX和--remoting-auth-token=XXX

```sh
wmic PROCESS WHERE name='LeagueClientUx.exe' GET commandline
```

### 修改项目配置文件

打开vite.config.js文件更改proxy代理中的端口号为终端中的

### 安装项目

```sh
npm install
```

### 运行项目

```sh
npm run dev
```

### 输入token

打开浏览器运行输入token即可正常使用

### 

### 
