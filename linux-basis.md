## 1.day-01
> ### 1. 决心书：
你想大学4年毕业3-4k;
培训5个月毕业10k;
每天玩手机，看视频，闲着无聊。 肯定是不行的。

> 给自己写份决心书:
抛弃慵懒无聊的状态，自己坚定目标，自信，做强自己。
每天学习2个小时；
完成有什么奖励，完成了吃饭；完不成有什么惩罚；完不成不睡觉。

> 勤复习，每一天复习一天的知识笔记。
日期：2018-04-16
blog 51cto .com
把握时间！hold on!

> ### 2.运维任务
主要干活：
维护服务器， 数据不能丢；
24小时*7在线。

buffer cache 区别是什么、 buffer 是写数据，从cpu写数据到内存、硬盘
cache 是读数据，从磁盘，内存读数据如cpu;

网站处理高并发，一般用什么方法，如天猫双十一；
采用集群，有很大的内存和硬盘，数据交互首先在内存里实现，内存快；然后达到一定临界值将数据冲进硬盘；
这样就可以实现高并发访问；

内存能实现这么高的数量吗？可以。因为实际网站读写比例约为10:1；所以并发写入一般不是问题；读操作相对比较容易，这里还可以联想读写分离，主从同步。

运维：上文提到的内存和硬盘，是多个机器组成的集群架构环境，memcahed(纯内存)，redis(内存加磁盘)。具体并发操作是由软件控制的。

磁盘：固态硬盘》机械硬盘；机械硬盘慢。

http://oldboy.blog.51cto.com/2561410/926983
淘宝cdn大规模并发优化学习与点评。

淘宝开发团队发明了一套算法，把那些集中访问的80%的数据放在ssd；把剩下的图片我们放在传统sas或更低廉的sata盘。 这样我们真个节点性能非常好，单机可以支撑3千到4千的io;系统没有任何访问慢，或者其他不好的表现。

raid工具：
将几十个硬盘集体管理，不是各自为战。方便管理，把他们看做一个大的磁盘，然后在分区。有raid卡后，磁盘一般插在raid卡上，而不是直接插到主板上。

光驱可以不买； 买远程管理卡，远程管理进程。

## 2.day-02
> ### 1. 常用软件安装
常用软件安装 picpick 截图，everything 快速搜索本地工具； 主要用法：安装，设置快速弹出快捷键；
word 快捷键，ctrl+1,ctrl+2，设置一级标题，二级标题等..

## 3.day-03
> ### 1. 老学员分享
往下混 很容易，往上混 很难；
在企业中也是一样，态度很重要，早去一会，不会损失多少，每一天都尽全力度过，因为你放松了，会在某一天自己被生活逼的付出全力也追不上生活的快节奏，比如物价，房子，医疗。
老男孩 老师 blog，就是比较精彩的博客，足够学习使用linux运维；
> ### 2.linux介绍
linux 是开源，可自由传播的，商业可免费用，多用户，多任务，支持多线程，多cpu的操作系统；
网络协议tcp/ip就是在unix上开发和发展起来的.
自由软件不完全和免费相同，比如linux，交费可用获取服务，或者获取更好的版本.
linux操作系统=linux内核+GNU软件及系统软件+必要的应用程序

## 4.day-04
## 5.day-01
## 6.day-01
## 7.day-01
## 8.day-01
## 9.day-01