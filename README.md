
### mongod的配置

mkdir data,log,conf,bin

```
cp mongod&mongo -> bin
vim conf :
    port = 12345
    dbpath = data
    logpath = log/mongod.log
    //linux下启动一个后台进程，win下无效
    fork = true
```
### mongod启动

```
./bin/mongod -f conf/mongod.conf
```
### 使用mongo连接mongodb服务器
```
./bin/mongo 127.0.0.1:12345/mydb
```
### Schema
#### options   new Schema({},options)
- autoIndex: false 禁止自动创建索引
- bufferCommand: false 禁用缓冲
- capped: 1024     capped: {  size: 1024, max: 1000, autoIndexId: true } 文档的最大字节数
- collection: 'data' 设置集合名称
- emitIndexErrors: true  当索引建立失败后会在模型上发出一个error事件
- id: false  在Schema构建时禁止生成id属性
- _id: false  禁止在模式上加上_id
- minimize: false  存储空对象
-strict: false   构造函数中的值没有被指定在模式中，该值也会被保存
-validateBeforeSave   保存前验证

### Model
#### var Book = mongoose.model('Book', schema)  name为单数形式，mongoose会自动寻找复数形式，在数据库集合为Books
#### 文档是模型的
- .save()
- .remove()

### Documents
#### 检索 更新
- 更新
 ```
    Tank.findById(id, function (err, tank) {
  if (err) return handleError(err);

  tank.size = 'large';
  tank.save(function (err) {
    if (err) return handleError(err);
    res.send(tank);
  });
});
 ```

