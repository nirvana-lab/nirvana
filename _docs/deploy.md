---
title: 安装部署
category: Nirvana
order: 5
---

## 运行环境

Nirvana 是一个基于 Python 开发的测试框架，可以运行在 macOS、Linux、Windows 系统平台上。  


**Python 版本**：支持 Python 3.7及以上的所有版本。

**操作系统**：推荐使用 macOS/Linux。

## 安装

### 1.构建镜像
- UI
```
git clone https://github.com/nirvana-lab/nirvana-ui.git
docker build . -t nirvana-ui:latest
```
- Service 
```
git clone https://github.com/nirvana-lab/nirvana7.git
docker build . -t nirvana-server:latest 
```      
   

### 2.部署

- 目录结构
```  
.
├── docker-compose.yaml
├── nirvana_pgdata  用来挂载pg的数据
└── script   用来挂载python脚本
```

- docker-compose.yaml
<pre>
  <code>
version: "2"
services:
  nirvana-db:
    image: postgres:10
    container_name: nirvana-db
    restart: always
    ports:
    - 5432:5432
    environment:
      POSTGRES_DB: nirvana
      POSTGRES_PASSWORD: password
      PGDATA: /var/lib/postgresql/data/pgdata
    volumes:
    - ./nirvana_pgdata:/data/pg_data
    
  nirvana-service:
    image: nirvana-server:latest
    container_name: nirvana-service
    restart: always
    ports:
    - 9999:9090
    volumes:
    - ./script:/workspace/openapi/script
    environment:
      PG_HOST: nirvana-db
      PG_NAME: nirvana
      PG_USER: postgres
      PG_PASSWORD: password
      PG_PORT: 5432
      GITLAB_URL: https://git.lug.ustc.edu.cn 
    depends_on:
    - nirvana-db
    
  nirvana-ui:
    image: nirvana-ui:latest
    container_name: nirvana-ui
    restart: always
    ports:
    - 8888:80
    environment:
      GIT: https://git.lug.ustc.edu.cn
      TEST: http://nirvana-service:9090
      DB_USER: postgres
      DB_PASSWORD: password
      DB_HOST: nirvana-db
      DB_NAME: nirvana
      DB_PORT: 5432
    depends_on:
    - nirvana-db
   </code>
 </pre>

### 3.初始化GitLab Applications

登录GitLab，Setting-》Applications:  
`Name` 自定义名字  
`Redirect URI` Nirvana UI的地址    
`Scopes` 勾选api、read_user、read_api、read_repository、openid、profile、email    

![Tracker](/images/gitlab.png)

创建完成获得Application ID, Secret, Callback URL用于发送post请求
```
curl --location --request PUT 'http://{nirvana_ui_url}/api/sso'
	--header 'Content-Type: application/json'
	--data-raw '{
	"id": {Application ID},
	"secret": {Secret},
	"callback": {Callback URL}
	}'

```


如果没有gitlab请自行安装「[安装指南](https://github.com/lunamagic1978/document/blob/master/middleware/GitLab-ce%E5%AE%89%E8%A3%85.md)」

