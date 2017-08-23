---
layout: post
title:  三大开源运维监控工具zabbix、nagios和open-falcon优缺点详细比较
category: 工具
tags: 监控
keywords: 监控
---

## 三大开源运维监控工具zabbix、nagios和open-falcon优缺点详细比较  
借鉴别人记录下，避免需要用的时候到处找

![zabbix-nagios-falcon](img/zabbix-nagios-falcon.png)


#### zabbix介绍  
   zabbix是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。
   zabbix能监视各种网络参数，保证服务器系统的安全运营；并提供灵活的通知机制以让系统管理员快速定位/解决存在的各种问题。
   zabbix由2部分构成，zabbix server与可选组件zabbix agent。
   zabbix server可以通过SNMP，zabbix agent，ping，端口监视等方法提供对远程服务器/网络状态的监视，数据收集等功能，它可以运行在Linux，Solaris，HP-UX，AIX，Free BSD，Open BSD，OS X等平台上。
   
   1. zabbix的主要特点：
   - 安装与配置简单，学习成本低
   - 支持多语言（包括中文）
   - 免费开源
   - 自动发现服务器与网络设备
   - 分布式监视以及WEB集中管理功能
   - 可以无agent监视
   - 用户安全认证和柔软的授权方式
   - 通过WEB界面设置或查看监视结果
   - email等通知功能
   
   2. Zabbix主要功能：
   - CPU负荷
   - 内存使用
   - 磁盘使用
   - 网络状况
   - 端口监视
   - 日志监视
   
   3. 优点
   - 支持分布式监控
   - 自带绘图功能，获取到数值型的数据，可自动生成图
   - Web配置方式，操作易用性较好。添加监控项或机器时速度很快。
   - 有报警时无论在任何界面会弹出小窗口报警，同时有报警的声音提示，同时可对监控项的快速查看。
   - 自带内置函数较为丰富，同时也支持脚本及nagios等脚本的调用。
   - 出现问题时，可自动远程执行命令(需对agent设置执行权限)
   
   4. 缺点
   - 批量修改不方便，可用数据库辅助
   - 深入后，中文资料相当少，大部分问题需看官方的文档及论坛。
   - 缺少数据汇总功能，如无法查看一组服务器平均值，可考虑对其进行二次开发。
   - zabbix较cacti来说，画图功能较差些、流量获取较为复杂
   
   5. 选择Zabbix的理由
   - 报警及时，报警与设定时间吻合，没有报警延迟的情况。
   - Web管理方式，上手较为容易。
   - 安装、配置简单。
   - 功能较为全面，同时支持2000台服务器(国内测试)，官方测试5000台服务器。Nagios国内测试仅为400-500台。
   - 后端数据库支持多样，Mysql，oracle等。
   - 可基于监控的数据，做二次开发。
   - 支持远程行执命令。（安全及风险需考虑）
   
#### open-falcon介绍  
  [open-falcon教学文档](https://book.open-falcon.org/zh/intro/index.html)
  
#### nagios介绍