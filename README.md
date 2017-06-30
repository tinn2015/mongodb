
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
- init()
- validate()
- count()
- find()
- findOne()
- findOneAndRemove()
- findOneAndUpdate()
- update

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
 ### query 
 #### 模型的几个静态方法检索文档，
 - 在Person中查询name.last匹配'Ghost'的，选出name和occupation的数据
 ```
 Person.findOne({ 'name.last': 'Ghost' }, 'name occupation', function (err, person) {
  if (err) return handleError(err);
  console.log('%s %s is a %s.', person.name.first, person.name.last, person.occupation) // Space Ghost is a talk show host.
})
 ```
 - 所有mongosoe 的回掉模式为callback(error,result)
 - findone()  一种单文档
 - find() 一个文档列表
 - count() 文档数量
 - update() 文件数量
 
 ### 验证
 - 验证定义在Schema Type中
 - 验证时中间件，mongoose在每次保存会在schema的pre.('save')中触发
 - 手动使用doc验证，validate(callback)或者doc.validateSync()
 - 除了require验证器，验证程序不在未定义的值上触发
 - 验证异步递归
 - 验证时可定制的
 -
 ```
 var breakfastSchema = new Schema({
      eggs: {
        type: Number,
        min: [6, 'Too few eggs'],
        max: 12
      },
      bacon: {
        type: Number,
        required: [true, 'Why no bacon?']
      },
      drink: {
        type: String,
        enum: ['Coffee', 'Tea']
      }
    });
    var Breakfast = db.model('Breakfast', breakfastSchema);

    var badBreakfast = new Breakfast({
      eggs: 2,
      bacon: 0,
      drink: 'Milk'
    });
    var error = badBreakfast.validateSync();
    assert.equal(error.errors['eggs'].message,
      'Too few eggs');
    assert.ok(!error.errors['bacon']);
    assert.equal(error.errors['drink'].message,
      '`Milk` is not a valid enum value for path `drink`.');

    badBreakfast.bacon = null;
    error = badBreakfast.validateSync();
    assert.equal(error.errors['bacon'].message, 'Why no bacon?');
 ```
 - 自定义验证器
 ```
  var userSchema = new Schema({
      phone: {
        type: String,
        validate: {
          validator: function(v) {
            return /\d{3}-\d{3}-\d{4}/.test(v);
          },
          message: '{VALUE} is not a valid phone number!'
        },
        required: [true, 'User phone number required']
      }
    });
 ```
 ### 中间件
 #### 有两种类型的前置钩子，串行（serial）和并行（parallel）
 - 串行 next()
 ```
 var schema = new Schema(..);
  schema.pre('save', function(next) {
  // do stuff
  next();
});
 ```
 - 并行
 ```
 var schema = new Schema(..);

// `true` means this is a parallel middleware. You **must** specify `true`
// as the second parameter if you want to use parallel middleware.
schema.pre('save', true, function(next, done) {
  // calling next kicks off the next middleware in parallel
  next();
  setTimeout(done, 100);
});
 ```
 - 错误处理
 ```
 schema.pre('save', function(next) {
  // You **must** do `new Error()`. `next('something went wrong')` will
  // **not** work
  var err = new Error('something went wrong');
  next(err);
});

// later...

myDoc.save(function(err) {
  console.log(err.message) // something went wrong
});
 ```
 
 
 
 

