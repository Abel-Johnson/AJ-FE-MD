# [X]promise高级


```js
var q = require('q');

var a = function(){
  var d = q.defer();

  d.resolve(1);
  return d.promise;
};

a().then(function(r){
  console.log(r); // 此处是１
  return 2;
}).then(function(r){
  console.log(r);  // 此处2，是由上一个then返回的
  var d = q.defer();
  d.resolve(3);
  return d.promise;
}).then(function(r){
  console.log(r); // 此处是3，由上一个then返回的promise的resolve提供．当需要异步调用时直接return的值肯定不够用，这时就需要返回promise对象．
});
```

then里面的函数的返回值如果是一个直接量，则会作为下一个链式调用的then的参数
如果返回值具有promise的接口，则返回该promise的resolve的结果