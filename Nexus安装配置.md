### 安装命令
```javascript
docker run -d --restart=always -p 8081:8081 -p 8082:8082 -p 8083:8083 -p 8084:8084 -v /usr/local/nexus-data:/nexus-data --name nexus3 --privileged=true sonatype/nexus3
```

### Nexus配置
可参考： https://blog.csdn.net/kidari/article/details/100059642

### 授权

#### 角色-nx-admin
Available
```javascript
nx-all
```

#### 角色-deployment
Available
```javascript
nx-component-upload
nx-repository-view-*-*-*
```

#### 角色-dev
Available
```javascript
nx-repository-view-*-*-read
```

#### 用户-nx-admin
Available
```javascript
nx-admin
```

#### 用户-deployment
Available
```javascript
deployment
dev
nx-anonymous
```

#### 用户-dev
Available
```javascript
dev
nx-anonymous
```


