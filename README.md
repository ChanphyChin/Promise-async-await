### es6 强大的 Promise 对象
* 很多的插件里面的.then() , .catch() 都是基于Promise对象封装的
* 主要使用场景：调用接口等待服务器返回数据之后才能进行下一步操作
* 优点：解决了依赖接口返回数据多层嵌套的回调地狱，使代码结构清晰，便于后期维护。
``` javascript
简单实例：

      var myPromise = new Promise(function(resolve, reject) {
          if(true) { resolve() };
          if(false) { reject() };
      })

注意：Promise一旦实例就会执行，所以一般promise对象写在函数里面，并且作为函数的返回值：

      function myPromiseFn (arg) {
        return new Promise(function(resolve,reject){
          if(typeof arg == 'string'){
            resolve(arg + 'is a string')
          }else{
            reject(arg + 'is not a string')
          }
        })
      }

promise里面的resolve和reject分别代表什么呢，resolve代表符合要求，reject代表不符合要求。
.then()有两个参数分别为两个函数
在调用了这个方法之后我们可以用.then()的方式来接收resolve和reject里面的返回值：

      myPromiseFn('hello world').then(function(res){ 
        console.log(res); //    hello world is a string   
      },function(err){
        console.log(err); //   不会进入到这里面
      })

      myPromiseFn(88).then(function(res){ 
        console.log(res); //    不会进入到这里面  
      },function(err){
        console.log(err); //   88 is not a string  
      })

.then()里面的第二个参数可以不写，在.catch()里面获取也是一样的。

      myPromiseFn(88).then(function(res){ 
        console.log(res); //    不会进入到这里面  
      }).catch(function(err){
        console.log(err); //   88 is not a string  
      })

.then()这个方法的执行结果也会返回一个Promise对象，上面的代码也可以写成 

      myPromiseFn(88).then(function(res){ 
        console.log(res); //    不会进入到这里面  
      }).then(null,function(err){
        console.log(err); //   88 is not a string  
      })

```
### Promise.all()
* promise.all()的主要使用场景在于：循环执行请求，等待循环的所有请求都执行完毕。
* promise.all()的参数为promise数组
``` javascript
简单实例：

      function myPromiseFn (){
        var promiseArr = [];
        for(var i = 0; i < 5; i++){
          promiseArr.push(
            new Promise(function(resolve,reject){
              setTimeout(function(){
                console.log(i);
                resolve();
              },i*500)
            })
          )
        }
        return Promise.all(promiseArr);
      }

把每一次循环调用接口变成一个promise对象，并且添加到数组中，Promise.all()这个方法会在你所有的promise对象都resolve或者reject之后再调用。

      myPromiseFn().then(function(){
        console.log('promiseAll')
      })

输出结果为: 0,1,2,3,4,promiseAll

```
### Promise.race()
* Promise.race()方法与.all()方法的唯一区别是只要传入的promise数组resolve或者reject了，.then()就会执行.

``` javascript
简单实例：

      function myPromiseFn (){
        var promiseArr = [];
        for(var i = 0; i < 5; i++){
          promiseArr.push(
            new Promise(function(resolve,reject){
              setTimeout(function(){
                console.log(i);
                resolve();
              },i*500)
            })
          )
        }
        return Promise.race(promiseArr);
      }

把每一次循环调用接口变成一个promise对象，并且添加到数组中，Promise.all()这个方法会在你所有的promise对象的其中一个resolve或者reject之后立即调用。

      myPromiseFn().then(function(){
        console.log('promiseRace')
      })

输出结果为: 0,promiseRace,1,2,3,4
