# Microsoft Office LTSC 2024长期服务版安装教程

## 下载Office Deployment Tool并安装

1. office 软件部署工具：https://www.microsoft.com/en-us/download/details.aspx?id=49117

2. 下载安装到桌面新建文件夹OFFICE，并改名为setup

## 配置Office版本

1. office 版本自定义工具：https://config.office.com/deploymentsettings
2. 基于KMS的 GVLK：https://learn.microsoft.com/zh-cn/deployoffice/vlactivation/gvlks

3. 将xml文件重命名为config并移到OFFICE文件夹中（删除原本的xml）

## 安装

1. 以管理员身份运行CMD，并进入OFFICE文件夹中

2. 执行下载命令：`setup /download config.xml`

3. 执行安装命令：`setup /configure config.xml`

## 启动

依次执行以下命令（管理员）：
```cmd
cd C:\Program Files\Microsoft Office\Office16
cscript ospp.vbs /sethst:kms.03k.org 
cscript ospp.vbs /act
```


注意：如果你安装的是32位版本，那么启动命令第一个要改成：

`cd C:\Program Files (x86)\ Microsoft Office\ Office16`