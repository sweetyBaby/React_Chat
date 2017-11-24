# 问题记录

## 安全  

1. 用户密码采用bcrypt-nodejs模块进行加密
2. 用户登录成功后，采用jsonwebtoken生成token发送给客户端保存起来，客户端在以后的一些具有修改行为的请求中携带此token,服务端验证token是否合法，是则进行后续的处理，否则返回错误消息。

## webpack无法对组件里以相对路径引用的图片进行打包

## presentation component与container component的组织问题

## 在一个点击事件中无法连续调用两次相互依赖的dispatch

## 刷新浏览器会自动掉线，丢失登录信息

> 解决办法， 用户每次手动登录之后，将token存在localStorage, 打开该应用时首先将token发送至服务器，看token是否合法，合法则查找数据库中是否存在该用户， 存在便取出用户信息，发送给客户端，并保存此时连接的socket.id，此时便自动重新登录成功，否则，若token不合法，便给客户端返回错误通知，使页面自动跳转到登录注册页面，等待用户重新手动登录

## 往七牛云上传文件始终无法成功

> 原来只是参考了旧版的Node.js SDK说明文档，所以难怪反复的比对也找不出来啥错。。。类似react-router也是，总是参考了router3的文档，router4做的改变又比较大，一定要先找对文档= =

## 如何在接受到新消息reducer调用结束的时候，紧接着调用RoomMsg组件内的方法，自动滚动到div底部，
不能直接把这个组件export出去，
https://github.com/redsx/zz-plugin/blob/master/src/components/UI/NotificationApi/Notification.tsx

## 接口说明

### 1.注册

#### 事件   

> 'signUp'

#### 请求参数  

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | nickname | true | String  | 昵称 |
> | email | true | String  | 邮箱 |
> | password | true | String  | 密码 |

#### 返回字段

> 成功: {isError: false, msg: {token: ..., user: ...}}
> 失败：{isError: true, msg: ...}

----------

### 2.登录

#### 事件   

> 'login'

#### 请求参数  

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | nickname | false | String  | 昵称 |
> | email | false | String  | 邮箱 |
> | password | true | String  | 密码 |
> 昵称和邮箱任选其一

#### 返回字段

> 成功: {isError: false, msg: {token: ...} }
> 失败： {isError: true, msg: ...}

-----------

### 3.注销登录

#### 事件   

> 'disconnect'

#### 请求参数  

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | token | true | String  | 无 |


#### 返回字段

> 成功: {isError: false, msg: {msg: '已注销'}}
> 失败: {isError: true, msg: ...}

----------------

#### 4. 得到所有用户列表

#### 事件   

> 'getUsers'

#### 请求参数  

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | token | true | String  | 无 |


#### 返回字段

> 成功: {isError: false, msg: {users: [...]}}
> 失败: {isError: true, msg: ...}


### 5.发送消息

#### 事件   

> 'new message'

#### 请求参数  
```javascript
{
    type: String  
    content: String 
    toId: String 
    msgType: String
}
```


### 4.接受消息

#### 事件   

> 'new message'

#### 返回数据
```javascript
{
    sender: String,
    createAt: String, 
    content,
    avatar: String, 
     _id: String, 
    type: String,  // private or group
    from: String, 
    msgType: String  // text or file
}
```

### 5.获取历史消息

#### 事件   

> 'getHistory'

#### 请求参数
```javascript
{
    token: String,
    to: String,
    type: String,
    limit: Number,
    timestamp: Number
}
```
#### 返回数据 (List)
```javascript
[{
    sender: String,
    avatar: String,
    content: String,
    createAt: String,
    _id:String,
    msgType: String
}]
```


#### 6.根据昵称查找某用户   

> 'find user'

#### 请求参数

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | nickname | true | String  | 对方昵称 |

#### 返回数据 

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | _id | true | String  | 用户id |
> | createAt | true | String  | 无 |
> | sex | true | String  | 无 |
> | type | true | String  | 无 |
> | avatar | true | String  | 无 |
> | onlineState | true | boolean  | 无 |
> | lastOnlineTime | true | String  | 无 |

#### 根据昵称查找某用户   

> 'build group'

#### 请求参数

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | nickname | true | String  | 群组昵称 |
> | avatar | 可选 | String  | 群组头像 |

#### 返回数据 

该group的所有信息

> 'join group'

#### 请求参数

> | 参数      | 必选   | 类型   | 说明   |
> | :------ | :--- | :--- | ---- |
> | nickname | true | String  | 群组昵称 |

#### 返回数据 
> 成功: {isError: false, msg: {group}}
> 失败: {isError: true, msg: ...}












