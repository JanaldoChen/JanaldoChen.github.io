---
layout: post
title: 远程访问Jupyter notebook
excerpt: "远程访问Jupyter notebook"
tag:
- 远程访问
- Jupyter
- Notebook
---

# 远程访问 $$Jupyter Notebook$$

### 登陆远程服务器

* ssh 远程登陆$$Linux​$$服务器

  ```shell
  ssh username@address_of_remote
  ```

* 退出远程服务器登陆

  ```shell
  exit
  ```

### 生成配置文件

```shell
jupyter notebook --generate-config
```

### 生成密码

* 打开$$ipython$$, 创建一个密文的密码

  ```shell
  ipython
  ```

  ```python
  In [1]: from notebook.auth import passwd
  In [2]: passwd()
  Enter password:
  Verify password:
  Out[2]: ‘sha1:6db2140a07d2:d190a4ed755043d596edba853b28869f513c16a4'
  ```

### 修改默认配置文件

* 使用 $$vim$$ 编译器修改配置文件

  ```shell
  vim ~/.jupyter/jupyter_notebook_config.py
  ```

* 进行如下修改:

  ```python
  c.NotebookApp.ip='*'
  c.NotebookApp.password = u'sha:ce...刚才复制的那个密⽂'
  c.NotebookApp.open_browser = False
  c.NotebookApp.port =8888 #随便指定⼀个端⼝
  ```

### 启动$Jupyter Notebook$

```shell
jupyter notebook
```

### 远程访问

此时应该可以直接从本地浏览器直接访问 $$http://address\_of\_remote:8888$$就可以看到$$jupyter$$的登陆界面。

### 建立ssh通道

如果登陆失败，则有可能是服务器防⽕墙设置的问题，此时最简单的⽅法是在本地建立⼀个$$ssh$$ 通道： 在本地终端中输⼊$$ssh \ username@address\_of\_remote -L127.0.0.1:1234:127.0.0.1:8888 $$便可以在 $$localhost:1234$$ 直接访问远程的$$jupyter$$了。

