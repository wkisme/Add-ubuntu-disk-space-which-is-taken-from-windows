# Add-ubuntu-disk-space-which-is-taken-from-windows
I follow a tutorial, which is helpful.  
After finish this tutoril, I use command htop to check my swap usage, which is not used.  
Actually, ubuntu is different from windows, in which RAM and swap are used together.  
ubuntu will use swap till the RAM is all used.  
In physical world, RAM is much faster than swap(just disk). So don't worrry about that swap is not used on ubuntu till you RAM is filled.  

版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
本文链接：https://blog.csdn.net/Carina_Cao/article/details/90270389

本文使用图文并茂的方式,提供让你一看就明白的扩容教程,并且超级安全,不用担心系统损坏或文件丢失啦~
首先安装gparted: sudo apt-get install gparted. 启动gparted:sudo gparted,或者在Dash里搜索gparted打开,看到如下界面:
在这里插入图片描述点击右上角, 选择当前磁盘(小C这里是250G固态和1T机械,ubuntu装在机械硬盘里),如下图:
在这里插入图片描述可以看到320G未分配区域,这块区域是小C从windows系统下腾出来(也可以不用腾),要一步步挪到/根目录上方,最后与根目录合并的.后面给出合并过程.在此之前,我们需要将ext4前面的小钥匙去掉,有它存在,我们就不能对根目录操作了呢~
接着,制作ubuntu16.04的U盘启动盘,从U盘启动,进入安装ubuntu界面,选择"try ubuntu"(切记不要安装),直接进入试用ubuntu界面,如下:
在这里插入图片描述
进入GParted,发现小钥匙不见了!
在这里插入图片描述
接下来,选中linux-swap,右键,选择"禁用交换空间/swapoff",小钥匙又不见了! 终于可以放心合并空间了.
在这里插入图片描述
右键fat32,选择"调整大小/移动":
在这里插入图片描述在这里插入图片描述
这里有三个编辑框，分别是：Free Space Preceding, New Size, Free Space following
Free Space Preceding代表从sda4压缩出未分配空间，并将其放在sda4的上方，即sda4与sda3之间
New Size表示sda4的容量，若要从sda4压缩出未分配空间，该处为减去压缩值后的值.
Free Space following代表从sda4压缩出未分配空间，并将其放在sda4的下方，即sda4与sda5之间
由于小C已经提前压缩了未分配空间,这里只需将Free Space Preceding的容量剪切到Free Space following里,然后选择"resize/move"就可以了.
tip:若没有提前压缩,选择你想腾出空间的磁盘,只需将Free Space Preceding或Free Space following里填入需要压缩的容量(New Size里会自动计算剩余值),再把未分配空间一步步挪到根目录上方或下方就可以了.
在这里插入图片描述
可以看到未分配空间已经挪到了sdb4和sdb5之间,中间提示的警告可以忽略.同样的方法将其挪到根目录上方.如下:
在这里插入图片描述
右键ext4,选择"调整大小/移动":
在这里插入图片描述
将上方的条形框拉到最大,此时Free Space Preceding和Free Space following都为0,选择"resize/move".如下:
在这里插入图片描述
至此,调整工作完成! 如下图(根目录扩充到了490G). 但是别急,后面还有重要的
在这里插入图片描述
如果发现调整的不对,不用担心,点击上方橙色的箭头可以撤销,检查没有问题,点击绿色对号,将调整结果应用到整个系统.
在这里插入图片描述
进度条走完后,还有项非常重要的工作,敲小黑板啦~ 将linux-swap的小钥匙添加回来(右键-启动交换空间/swapon), 否则,重启系统会出错!
在这里插入图片描述
最后,重启系统,享受在大海里遨游的感觉吧!
————————————————
版权声明：本文为CSDN博主「Carina_Cao」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Carina_Cao/article/details/90270389
