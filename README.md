# manageQuadTitan2
llp和wt的四卡泰坦说明文档

## 硬件概况

| 条目 | 详情 | 备注 |
| ------ | ------ | ------ |
| 处理器 | Intel® Core™ i7-7820X CPU @ 3.60GHz × 16  |  |
| 主板 |  |  |
| 内存 | 64GB |  |
| 硬盘 | 1TB NVMe SSD + 4TB HDD |  |
| 显卡 | Nvidia Titan Xp 12GB x4 |  |

## Ubuntu系统

### 环境
Ubuntu：16.04.6 LTS  
Nvidia Driver:**to install**
CUDA：**to install** 9.0  
cuDNN：**to install** cuDNN v7.4.2 (Dec 14, 2018), for CUDA 9.0  
IP：192.168.1.118

### root 账户
Username：byjw
Password：byjw

### 个人的账户和密码（推荐通过SSH登录）
Username：姓名首字母缩写（小写）  
Password：姓名全拼（小写）  
请尽快修改密码  

| 姓名 | 用户名 | 密码 | 组 | 初始路径 | sudo权限  |
| ------ | ------ | ------ | ------ | ------ | ------ |
| 王腾 | wt | wangteng | student | /home/wt | 是 |
| 罗利鹏 | llp | luolipeng | student | /home/llp | 是 |

## 注意
0. 由于服务器共享使用，请不要随意重启服务器以免带来不必要的麻烦。  

1. 如果有必要，使用形如`nohup python -u trian.py &`命令保证进程不间断运行（如断开SSH连接等），`-u`可加可不加，它能保证python程序的输出可以无缓存、及时地更新到nohup.out文件中，对于不会主动关闭的进程，训练后使用`kill`。  
  更多用法参考[其他教程](https://blog.csdn.net/fang_chuan/article/details/82017470)
  
2. 使用`conda`或 `pip`命令来创建和管理**你的环境**。  
  **conda**：使用`conda create -n yourenvname python=pythonversion`命令创建属于你的python环境，例如`conda create -n wtkeras python=2.7`。 创建的环境路径位于`/home/root1root/anaconda3/envs/yourenvname/bin/python`。使用`conda env list`来查看目前存在和激活的的conda环境，使用`source activate yourenvname`来激活你的环境。安装GPU版的tf等框架建议使用pip命令，因为使用conda命令会自动下载对应的cuda和cudnn，不知道时候会有影响。（注：本机当前安装的驱动程序不支持cuda9.2及以上）关于conda、pip命令的更多使用方法，请参考其他教程。 
    
3.  在python中，务必在程序中加入如下代码来指定抢占的GPU，x为0或1或2或3或0,1或0,2等（一共四块，为0123），尽量不占用大量显卡。    
```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = 'x' 
```  
  
  也可以在Terminal中直接在命令前加上`CUDA_VISIBLE_DEVICES=0`来指定GPU  
  
  
4. 在Ubuntu系统中，可以使用如下代码来查看GPU的占用情况, `-n`后指定刷新的时间间隔， `-d`高亮刷新部分，建议运行代码前查看空闲的GPU
```linux
watch -n 1 -d nvidia-smi
```
  
5. 如果使用Pycharm Professional进行远程开发、调试，可以参考[这篇文章](https://blog.csdn.net/yejingtao703/article/details/80292486)（配置时使用自己的Username和环境）  
  并可以在Pycharm Professional的Remote Host中进行图形化文件管理。  
  关于怎样免费获取Pycharm Professional Edition，可在搜索引擎中搜索 **Jetbrain 学生** 或 **Github 学生（推荐）**   ，github学生认证更方便，通过后学生包中直接授权到Jetbrain的学生认证。  
    
5.1 Clion远程配置[官方教程](https://www.jetbrains.com/help/clion/remote-projects-support.html)  
6. Windows轻松使用：由于服务器没有安装FTP服务，所以无法在Windows资源管理器中直接添加网络位置，需借助第三方软件。  
  欲在Windows资源管理器中“挂载”该网络位置，在搜索引擎中搜索**SFTP Net Drive（推荐，并在Internet选项中将本服务器IP地址加入本地Intranet来消除安全提示）**或**Swish - Easy SFTP for Windows**了解一下，都基于SSH的子协议SFTP；或使用**WinSCP**来进行图形化的文件管理。  
  下图为Swish的集成效果，可以支持更多本地化操作。  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/SFTP%20Net%20Drive.png)
  
  下图为Swish的集成效果。  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/swish.png)
  
  下图为WinSCP的使用效果  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/winscp.png)  
    
7.如果提示`libcublas.so.9.0: cannot open shared object file: No such file`的话，试一下在自己目录下的`./bashrc` 中末尾添加 `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64/` 然后执行 `source  ~/.bashrc` 一般可以解决。下次运行还会报错，只要执行那条source命令即可，我也不知道为啥。。。。求高人指点。在Pycharm中运行时，将 `LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64/`添加到运行配置的环境变量中即可 。另外，有一位Mac用户需要在Pycharm的configuration中的环境变量中加上LD_LIBRARY_PATH，使用mac的同学注意一下。  
使用nvcc使用下面三行命令
```linux
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64
export PATH=$PATH:/usr/local/cuda-9.0/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-9.0
```

8.Windows下PUTTY+Xming实现远程程序窗口转发[教程](https://blog.csdn.net/u013554213/article/details/79885792)，可用于查看程序popup的图片视频等。  


