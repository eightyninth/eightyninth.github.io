---
title: github + hexo 简单搭建 github.io 网站
date: 2022-07-02 21:22:45
tags: [网站, hexo, github]
categories: [网站]
---

# 系统配置
这里先说一下我的系统配置以供参考

> Ubuntu 20.04 5.13.0-51-generic x86_64

# 本地前置要求

* node.js >= 10.13

* Git

* hexo


## node.js

按照 [官方安装方式][1] , Ubuntu 采用安装命令行, 这里安装的版本是18.4.0.

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

进行测试

```bash
npm --version
```

出现版本号, 即为安装成功.

## Git

Ubuntu 安装命令

```bash
sudo apt-get install git
```

## hexo

创建一个空文件夹作为 hexo 安装路径的同时也会是网站根目录.

> 为了后述具体,此时假设新建的目录路径为: <font color=red>/example</font>

# github前置要求

* github账号
  > 为了后述具体,此时假设github账号
  > 
  > 用户名为: test
  > 
  > 邮箱为: test@163.com
  > 
  > 密码为: 123456

* ssh密匙

## 新建网站远程仓库

1. New Repository, 命名为

    ```bash
    test.github.io
    ```

2. 打开 **test.github.io** , 在与 <font color="red">code</font> 同级的 <font color="red">Settings</font>下打开与 <font color="red">General</font> 同级的 <font color="red">Pages</font>. 在<font color="red">Theme Chooser</font> 下选择<font color="red">Choose a theme</font>.


3. 选择好主题后, 把新弹出的界面拉到底部, 选择**commit changes**, 网站就可以访问了.


4. 输入 test.github.io 访问.

## 设置ssh密匙

为了hexo能够顺利push到我们的仓库, 需要在本地电脑设置ssh密匙

1. 检查本机是否有ssh密匙
    
    ```bash
    cd ~/.ssh #检查本机已存在的ssh密钥
    ```
  
    如果存在 **No such file or directory**, 说明你是第一次使用git, 需要创建ssh.
    
    <br />

    或者

    ```bash
    cd ~/.ssh #检查本机已存在的ssh密钥
    ls
    ```

    没有任何输出, 需要创建ssh.

    <br />

    若有ssh密匙, 直接跳转至第 3 步.


2. 创建ssh密匙

    ```bash
   ssh-keygen -t rsa -C test@163.com
    ```

    然后**连续3次回车**, 最终会生成一个文件在 <font color=red>~</font> 下.


3. 打开 <font color=red>~</font> , 找到 <font color=red>.ssh\id_rsa.pub</font> 文件.

    > 注: 如果打开 <font color=red>~</font> ,没有 <font color=red>.ssh</font> 文件夹, 需要 <kbd>Ctrl</kbd> + <kbd>H</kbd> 显示隐藏文件夹.

    使用记事本一类的程序打开, 复制所有内容.


5. 打开 github , 点击右上角个人头像, 在弹出的窗口中选择 <font color=red>Settings</font>.
然后选择与<font color=red>Public profile</font> 同级 <font color=red>Access</font> 下的
<font color=red>SHH and GPG keys</font>.


6. 选择 <font color=red>New SSH key </font>.

   > title: 随便填
   > 
   > Key: 粘贴从 <font color=red>.ssh\id_rsa.pub</font> 文件复制的内容

    <font color=red>Add SSH key </font>.


7. 测试
    ```bash
    ssh -T git@github.com # 注意邮箱地址不用改
   ```

    如果提示<font color=blue>Are you sure you want to continue connecting (yes/no)?</font>，输入**yes**, 然后会看到:

    > Hi test! You’ve successfully authenticated, but GitHub does not provide shell access.

    看到这个信息说明SSH已配置成功!

## Git本地配置：

打开命令行, 输入

```bash
git config --global user.name "test"            // 你的github用户名，非昵称
git config --global user.email  "test@163.com"  // 填写你的github注册邮箱
```

# 网站搭建

## 初始化

在 <font color=red>/example</font> 目录下, 打开命令行, 输入

```bash
sudo npm install -g hexo-cli
hexo init
```

## 加载主题

在[Hexo官网][2]选择喜欢的主题, 这里以[Ayer 主题][3]为例, 输入

```bash
npm i hexo-theme-ayer -S
```

## 主题设置

1. 主题使用

    <br />

    将 <font color=red>/example</font> 目录下的 **_config.yml** 里的 theme 值修改成 ayer

    ```bash
    theme: ayer
    ```

2. 必要插件

    <br />
    
    在 <font color=red>/example</font> 目录下, 打开命令行, 输入
    
    <br />
   
    - [搜索][4]

    ```bash
    npm install hexo-generator-searchdb --save
    ```
    
    - [RSS 订阅][5]

    ```bash
    npm install hexo-generator-feed --save
    ```
    <br />
   
    打开<font color=red>/example</font> 目录下的 **_config.yml**, 输入
        
    <br />
   
    - [搜索][4]
   
     ```
    # hexo-generator-searchdb
    search:
    path: search.xml
    field: post
    format: html
    ```

    - [RSS 订阅][5]

    ```
    feed:
    type: atom
    path: atom.xml
    limit: 20
    hub:
    content:
    content_limit: 140
    content_limit_delim: " "
    order_by: -date
    ```
   
3. 可选插件

    <br />
    
    在 <font color=red>/example</font> 目录下, 打开命令行, 输入
    
    <br />
   
    - [文章置顶][6]

    ```bash
    npm install hexo-generator-index-pin-top --save
    ```
    
    - [二次元看板娘][7]

    ```bash
    npm install --save hexo-helper-live2d
    npm install npm install --save live2d-widget-model-xxx  # xxx为你选择的模型
    ```
    <br />
   
    打开<font color=red>/example</font> 目录下的 **_config.yml**, 输入
        
    <br />
   
    - [文章置顶][6]
   
     ```
    # 已存在
    index_generator:
    path: ''
    per_page: 10
    order_by: -date
    ```

    - [二次元看板娘][7]

    ```
    live2d:
    enable: true
    scriptFrom: local
    pluginRootPath: live2dw/
    pluginJsPath: lib/
    pluginModelPath: assets/
    tagMode: false
    debug: false
    model:
      use: live2d-widget-model-wanko  # 选择模型
    display:
      position: right
      width: 150
      height: 300
    mobile:
      show: true
    react:
      opacity: 0.7
    ```

    更多插件查看 [hexo 插件市场][8].


4. 菜单处理
    
    <br />
   
    Ayer 初始情况下, 菜单的<font color=blue>分类(categories), 标签(tags), 旅行(tags/旅行), 友链(friends), 关于我(2019/about)</font>, 点击全部显示 <font color=red>error</font>, 需要初始化.

    模板, 对 XX(path) 的初始化:
    
    <br />
   
    a. 在 <font color=red>/example</font> 目录下, 打开命令行, 输入
    
    ```bash
    hexo new page XX
    ```
   
    b. 打开 <font color=red>/example/source/XX</font>, 复制 **index.md** 到 <font color=red>/example/source/path</font>, 删除 <font color=red>/example/source/XX</font> 目录.
    
    <br />
   
    c. 打开 **index.md**, 全部替换输入

    ```
    ---
    title: XX
    type: "XX"
    layout: "XX"
    ---
    ```
   
5. 本地测试

    <br />
   
    在完成上述操作后网站模板搭建完毕, 可以在本地查看网站效果.

    <br />

    在 <font color=red>/example</font> 目录下, 打开命令行, 输入

    ```bash
    # hexo 网站生成命令
    hexo g
    # hexo 网站本地部署命令
    hexo s -p 8008
    ```

   打开<font color=blue> http://localhost:8008/ </font> 即可查看部署效果.



6. 部署处理
    
    <br />

    在push网站到git之前, 要在<font color=red>/example</font> 目录下的 **_config.yml**, 输入

    ```
   deploy:
   type: git
   repository: git@github.com:test/test.github.io.git
   branch: master
   ```
   
   在 <font color=red>/example</font> 目录下, 打开命令行, 输入下述命令部署到云端.
   
   <br />
   
   ```bash
   # hexo 网站生成命令
   hexo g
   # hexo 网站云端部署命令
   hexo d
   ```
   
    打开<font color=blue> https://test.github.io/ </font> 即可查看部署效果.
    
    
## 新增页面
   
   文章页面的新增和菜单的处理不同.
   
   1. 在 <font color=red>/example/source/_posts</font> 目录下, 新建md文件.
   
   
   2. md写入:

      ```
      ---
      title: 文章名
      date: 2022-07-02 21:22:45       # 时间
      tags: [网站, hexo, github]      # 标签, 可以填入多个
      categories: [网站]              # 只能填入一个
      ---
      正文
      ```

以上, 网站搭建完成.

[1]: https://github.com/nodesource/distributions
[2]: https://hexo.io/themes/
[3]: https://shen-yu.gitee.io/2019/ayer/#%E4%BB%8B%E7%BB%8D
[4]: https://github.com/theme-next/hexo-generator-searchdb
[5]: https://github.com/hexojs/hexo-generator-feed
[6]: https://github.com/netcan/hexo-generator-index-pin-top
[7]: https://github.com/EYHN/hexo-helper-live2d/blob/master/README.zh-CN.md
[8]: https://hexo.io/plugins/
