# Grafana使用手册
## 简介
一个开源的数据可视化工具，有丰富的插件和功能强大的仪表盘配置。
## 安装和使用
使用Docker进行安装十分简单，启动对应的镜像即可。此处使用dockerhub的镜像monitoringartist/grafana-xxl:latest
## 配置参数说明
```ini
GF_DEFAULT_INSTANCE_NAME=grafana
GF_DATABASE_URL=mysql://<DBUSER>:<DBPASS>@<DBHOST>:3306/<DBNAME>
GF_DATABASE_TYPE=mysql
GF_USERS_ALLOW_SIGN_UP=false
GF_USERS_ALLOW_ORG_CREATE=false
GF_USERS_AUTO_ASSIGN_ORG=true
GF_USERS_AUTO_ASSIGN_ORG_ROLE=Viewer

# 在对接OAuth之前，暂时允许所有人访问，如果以后对接了OAuth，需要删除下列区域配置
GF_AUTH_ANONYMOUS_ENABLED=true
GF_AUTH_ANONYMOUS_ORG_NAME=Main Org.
GF_AUTH_ANONYMOUS_ORG_ROLE=Viewer

# 邮箱配置
GF_SMTP_ENABLED=true
GF_SMTP_HOST=smtp.exmail.qq.com:465
GF_SMTP_USER=<USER>
GF_SMTP_PASSWORD=<PASSWORD>
```
