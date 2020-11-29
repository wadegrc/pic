MySQL下载安装和配置：

+ MySQL下载地址：

  链接：https://pan.baidu.com/s/1T-psRK0WDFPdgjmBhIeqBg 
  提取码：z8lz 

+ 图形化工具Navicate下载地址：

  链接：https://pan.baidu.com/s/19swpDSM_SEbNuptwBGXS2A 
  提取码：7sjv

##### Step 1：

 下载完成后，第一步我们得来编辑我们的配置文件:**my.ini**,按以下步骤来：

+ 新建 **文本文档**，修改文件名为**my.ini**，ini是它的后缀名，然后它的图标变成**带齿轮**的，就说明修改成功了。

  ![ini](https://raw.githubusercontent.com/wadegrc/pic/图床/ini.png)

  ​	若未修改成功，请检查是否勾选下图中的选项，勾选后再修改文件名，修改时要连后缀一起修改

  

  + <img src="https://raw.githubusercontent.com/wadegrc/pic/图床/2.png" alt="2" style="zoom:67%;" />

  + 然后在**打开配置文件**添加以下代码，其中**basedir**和**datadir**后的路径修改为**你自己的文件路径**，例如我的就会修改为D:\MySQL\mysql-8.0.19-winx64，这一步非常重要，后面配置失败很可能是这里的原因：

    ```
    [mysqld]
    # 设置3306端口
    port=3306
    # 设置mysql的安装目录
    basedir=路径
    # 设置mysql数据库的数据的存放目录
    datadir=路径\data
    # 允许最大连接数
    max_connections=200
    # 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
    max_connect_errors=10
    # 服务端使用的字符集默认为UTF8
    character-set-server=utf8
    # 创建新表时将使用的默认存储引擎
    default-storage-engine=INNODB
    # 默认使用“mysql_native_password”插件认证
    default_authentication_plugin=mysql_native_password
    
    [mysql]
    # 设置mysql客户端默认字符集
    default-character-set=utf8
    [client]
    # 设置mysql客户端连接服务端时默认使用的端口
    ```

    到这里my.ini文件就配置完了，继续下一步.

    ##### Step 2:

    在系统环境变量中添加MySQL(/bin)路径，添加时，如果你是直接在其他路径后面添加的话，要记得**先加一个英文的分号**，如下图:

    ​	<img src="https://raw.githubusercontent.com/wadegrc/pic/图床/4.png" alt="4" style="zoom:67%;" />

    

      <img src="https://raw.githubusercontent.com/wadegrc/pic/图床/cmd.png" alt="3" style="zoom: 67%;" />

    

    ##### Step3:

    ​	在命令行中进行配置，以下步骤如果失败了，请检查前面步骤是否有误：

       + 以管理员身份打开命令行

       + 输入`mysqld --initialize-insecure --user=mysql`，输入这一行不会为你设置默认密码，

       + 成功了，则继续输入`mysqld -install`，此时可能会出现already exists的情况，这是因为你已经安装了mysqld，可参照下图解决：

         <img src="https://raw.githubusercontent.com/wadegrc/pic/图床/3.png" alt="cmd" style="zoom:67%;" />

         

         出现Service successfully installed.表示配置完成。
    
    + 启动数据库`net start mysql`：
    
      ![1cmd](https://raw.githubusercontent.com/wadegrc/pic/图床/1cmd.png)
    
    + 输入`mysql -u root -p`，不用输入密码直接回车：
    
      ![3cmd](https://raw.githubusercontent.com/wadegrc/pic/图床/5.png)
  

  

​	出现`mysql>`表示配置完成



   + 接下来修改密码，输入`alter user user() identified by "密码";`：
     
       ![cmd4](C:%5CUsers%5CGRC2001%5CDesktop%5Ccmd4.png)
       
   + 最后输入`quit`，再输入`net stop mysql`退出MySQL。



#####           到这里MYSQL就配置完成了，为了方便使用，可继续连接图形化工具Navicate。


#### Navicate连接:

​	直接参考[这里](https://blog.csdn.net/Z_97_97/article/details/103324780?utm_source=app)，记得连接之前要打开MySQL服务

<img src="https://raw.githubusercontent.com/wadegrc/pic/图床/3cmd.png" alt="5" style="zoom: 50%;" />

到这里，MySQL的配置就基本完成了，中间有表述不清楚的还请对照图片。









​         

​         

​         

​    

  