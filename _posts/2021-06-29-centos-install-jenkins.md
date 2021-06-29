---
title: CentOS7安装Jenkins
date: 2021/06/29
---

最近抽空集成Jenkins以实现公司项目的自动化部署，此文内容仅供参考。

<!-- more -->

原文是我写在notion中的一篇日志，后续可能会与其它安装说明整合为一体，具体参见[CentOS7安装Jenkins](https://www.notion.so/CentOS7-Jenkins-1e50b4c6ccd94117848f60cb0ec31abb)，此文作为单独的安装说明，将不再进行变动。

## 安装

下载依赖

```
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

![1](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0b144fbb-7fdf-4334-9c9c-4fe35e3630ea%2FUntitled.png?table=block&id=164b562d-72de-4676-b0f7-afb6d0c68c50&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2410&userId=&cache=v2)

## 导入秘钥

```
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```
![2](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8875fe39-2fe5-41cd-8ea4-d77cb6d27c26%2FUntitled.png?table=block&id=d393e209-c4d8-4c29-9d85-cc5988024452&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2410&userId=&cache=v2)

## 开始安装
```
yum install jenkins
```
![3](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F132918c3-b591-4474-bff8-7e9cb58485a4%2FUntitled.png?table=block&id=5fafcfdb-075c-4b95-b596-9fc6b6f4d3c5&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2400&userId=&cache=v2)

## 查看安装目录

```
rpm -ql jenkins
```
![4](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F5479e722-77a1-46c6-a529-0d9e9aa8eb0e%2FUntitled.png?table=block&id=d5a6c42e-1e47-4176-8b01-caee65fe21fc&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2400&userId=&cache=v2)

## 修改Jenkins端口号
```
vi /etc/sysconfig/jenkins
```
默认端口为8080，此处我修改其为8888
![5](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2e7d628c-160a-4344-8c38-4e166fa465a2%2FUntitled.png?table=block&id=851eff9f-3b47-4347-9b11-5bb7aa9cfb9d&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2400&userId=&cache=v2)

##  设置开机自动启动

```
chkconfig jenkins on
```

##  启动Jenkins
```
service jenkins start
```

![6](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F697acdf4-c60a-4944-a8ff-42b82b1404c9%2FUntitled.png?table=block&id=f45f01dd-519f-474f-8627-23b667ae4c12&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2410&userId=&cache=v2)

此处启动的时候有报错，如果成功启动，则忽略报错解决步骤

## Jenkins启动报错问题解决

首先查看错误日志

```
journalctl -xe
```

![7](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F564365a4-9c86-494c-a2c2-4df35d170753%2FUntitled.png?table=block&id=6af639f1-faa8-4abc-adbf-0a48e3ee3822&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2400&userId=&cache=v2)

修改Jenkins启动配置文件，指定Java安装路径（如果不知道Java安装路径的话，可以使用命令查询：which java）

```
vi /etc/init.d/jenkins
```

![8](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fadb49125-7799-4ddb-9338-a54d1efe71b5%2FUntitled.png?table=block&id=3b8d1e04-711e-47cd-8bf6-0ac9437011b7&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2400&userId=&cache=v2)

再次启动Jenkins

```
systemctl start jenkins
```

![9](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6e4cd046-a64a-4026-9d08-2ca71ff3503e%2FUntitled.png?table=block&id=80660c1d-ae87-40e3-a66c-11323128b81e&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2410&userId=&cache=v2)

启动仍然报错，但是本次报错是因为Jenkins服务发生了变化，只需要执行重新加载的命令即可移除报错信息

```
systemctl daemon-reload
```

![10](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ffb82af71-a7ef-4122-bd79-f0ad5d89e5fd%2FUntitled.png?table=block&id=55d6dab3-350d-4df5-b5e2-d8d6abd3e16d&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2400&userId=&cache=v2)

再次启动Jenkins没有出现任何提示，此时我们执行命令查看Jenkins运行状态

```
systemctl status jenkins
```

![11](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb1997d6c-fc88-4771-bed7-5c543114d2bf%2FUntitled.png?table=block&id=6b5b3631-1679-477f-b935-81b403b4aacc&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2410&userId=&cache=v2)

Jenkins此时已经处于启动状态，我们可以在浏览器输入Jenkins地址进行访问：[http://ip:8888](http://192.168.0.55:8888/)

如果页面无法访问，则需要我们手动打开防火墙端口（我设置的8888，具体端口请根据自身配置）

```
firewall-cmd --zone=public --add-port=8888/tcp --permanent
firewall-cmd --reload
```

此时再次刷新浏览器页面，就可以进入解锁Jenkins的页面啦

## 解锁Jenkins

根据页面提示，访问/var/lib/jenkins/secrets/initialAdminPassword，然后复制粘贴在“管理员密码”框中，再点击继续即可进入下一步

![12](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7a4bcf95-dd1b-4faa-b8a6-896be4eff220%2FUntitled.png?table=block&id=e220fc5c-7b41-4716-a358-572cf1a5850e&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2800&userId=&cache=v2)

## 自定义Jenkins

- 安装建议的插件：安装推荐的一组插件，这些插件基于最常见的用例
- 选择要安装的插件 ：选择安装的插件集。当你第一次访问插件选择页面时，默认选择建议的插件。

如果不确定需要那些插件，则选择“安装建议的插件”

![13](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F41f9d040-c1f7-436c-a9d8-a5e3201c90f8%2FUntitled.png?table=block&id=02886c97-bd1d-4241-aedc-c04d6533dc4d&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2300&userId=&cache=v2)

## 创建第一个管理员用户

根据实际情况填写各项字段

![14](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F09579c05-56e8-417b-8cdc-9e9528d12e22%2FUntitled.png?table=block&id=8292d921-afb9-4a24-a671-898e4961bd86&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2780&userId=&cache=v2)

## Jenkins实例配置

![15](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F68f11c06-c0c6-4e9a-9114-1bdbf007fbc3%2FUntitled.png?table=block&id=3857bf2c-88a0-47da-b20b-d9acfbd7b540&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2590&userId=&cache=v2)

## 开始使用Jenkins

![16](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4225b03b-cd4f-4b92-b00d-2cd2fe2d12cd%2FUntitled.png?table=block&id=230fa187-edf0-4fb7-80cf-f10cb992fdd3&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=2510&userId=&cache=v2)

点击开始使用Jenkins，即可进入Jenkins工作台

![17](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2ea2f2d2-aee3-4c27-99ba-1be93386ee8b%2FUntitled.png?table=block&id=dad7aa0a-24da-4dc5-aa15-be01b531149f&spaceId=a5f79601-ae0d-48db-9f4d-80bb7c3d6ea4&width=5120&userId=&cache=v2)

## 结束语

:)

