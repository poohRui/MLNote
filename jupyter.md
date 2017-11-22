# jupyter

> 简单的说是一个用python开发的可以运行编辑代码，查看运行结果的笔记本工具，具体介绍参见[官网](http://jupyter.org/)，系统基于Mac Os

## 环境搭建

* python3+virtualenv

  对于安装和操作`virtualenv`参照[文章](https://github.com/poohRui/DLNote/blob/master/%E5%9F%BA%E4%BA%8EVirtualenv%E5%AE%89%E8%A3%85TensorFlow.md)

* jupyter

  使用对应的pip版本安装

  ```
  pip install jupyter
  ```

  启动服务器，运行在网页

  ```
  jupyter notebook
  ```

  虽然很简单，但是坑还是比较多的。

## 前方有坑

### _sqlite3模块缺失

在安装`python`的时候，如果使用的是指令

```
sudo brew install python3
```

很可能在启动`jupyter`服务的时候，报出一下错误

```
ImportError: No module named '_sqlite3'
```

出现这种错误，原因可能有两种，一个是存在`sqlite3`这个模块，但是在安装`python`的时候没有链接进去里，二是不存在`sqlite3`这个模块，对于这种错误我的解决办法是先卸载已经安装的`python`版本。

```
sudo brew uninstall python3
```

确定已经安装了`sqlite3`模块

```
sudo brew install sqlite3
```

安装CLI

```
sudo xcode-select —install
```

重新安装`python`

```
sudo brew install python3
```

即可，具体我是从这个[issue](https://github.com/Homebrew/brew/issues/1171)中找到的解决办法

### 使用的是系统的`jupyter`，不是虚拟环境的

如果使用的是`virtualenv`一定要注意`jupyter`使用的是不是虚拟环境下的而不是系统环境下的。

```
which jupyter
# 如果结果是/usr/local/..，就似乎不太对了
```

由于我使用的是`virtualenv`没有用`Anaconda`，所以如果使用的方法不一样，下面我的解决方案仅仅是提供一个思路。

在配置虚拟环境的时候使用

```
virtualenv (--no-site-packages) -p python3 ~/test
```

这样配置一个完全干净的python环境以后再使用pip安装执行前面的操作即可解决。

### 在Mac10.12以上没法启动浏览器

在`~/.jupyter`目录下创建文件`jupyter_notebook_config.py`，文件内容如下：

```
c.NotebookApp.browser = u'Safari'
c.NotebookApp.token = ''
c.NotebookApp.password = ''
```

重启以后就好使

### 在jupyter中引入Tensorflow错误

创建适合的`kernel`内核

```
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

检测有没有新创建的`kernel`内核

```
jupyter kernelspec list
```

重启`jupyter`，然后在`kernel`选项中选择合适的环境。[参考](https://ipython.readthedocs.io/en/latest/install/kernel_install.html)