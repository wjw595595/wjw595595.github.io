# c盘瘦身

## 瘦windwos更新

删除：C:\Windows\SoftwareDistribution\Download  目录下文件

## 关闭休眠文件

- powercfg /h off #开启

- powercfg /h on  #关闭

  

方法一：在设置中开启【存储感知】

　　1、从Win10开始菜单打开【设置】-【系统】-【储存】，把储存感知功能的开关打开，系统会自动清理临时文件、更新缓存；

　　2、下面还有一个【立即释放空间】的按钮，点击后会自动进入手动删除，删除【以前的Windows安装】勾选后点击删除即可；

　　方法二：删除安装缓存文件

　　1、首先按下WIN+R，然后在运行中输入services.msc 回车；

　　2、在服务中找到【Windows update服务】，右键点击停止；

　　3、然后打开文件夹C:\Windows\SoftwareDistribution\Download，这个文件夹中的就是更新后留下的更新文件，将其全选删除；

　　4、删除后重新在服务中开启【Windows update服务】。