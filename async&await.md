### es7 async & await
* 自从es7加入了async和await之后，前端异步的处理就简单方便了很多了。
* async await 作用基于es6添加的promise对象。
* 可以用try 和 catch 语法捕获成功和失败。

```
    首先实例一个promise对象

    var mypromise = i=>{
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                console.log('async')
                resolve();
               
            },3000)
        })
    }

    在需要异步的方法前加上async，promise对象前await

    var myasync = async ()=>{
        console.log('start')
        await mypromise();
        console.log('end')
    }

    myasync();
    控制台打印 start  async  end
```

* async和await不需要.then捕获，用try catch捕获结果
```
    var mypromise = time=>{
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                // 模拟出错了，返回 ‘error’
                reject('error');
            }, time);
        })
    }

    var myasync = async ()=>{
        try {
            console.log('start');
            await mypromise(3000); // 这里得到了一个返回错误
            
            // 所以以下代码不会被执行了
            console.log('end');
        } catch (err) {
            console.log(err); // 这里捕捉到错误 `error`
        }
    }

    myasync();

```
* 感触最深的一个使用场景
    1. 需求说明：在一个循环里面重复调用一个接口，但是必须在前一个接口返回数据，并且成功之后才能继续调用下一次，如果失败，那么循环还没结束的部分取消，不再调用接口。
    2. 代码示例：
    ```
        var getData = i=>{
            return new Promise((resolve,reject)=>{
                setTimeout(()=>{
                    //这里假设第三个接口是有问题的，模拟错误信息
                    if(i==2){
                        reject('error');
                    }else{
                        resolve('success');
                    }
                },3000)
            })
        }

        var loopGetData = async ()=>{
            for(let i = 0; i < 5; i++){
                try{
                    await getData(i);
                    console.log('success');
                }
                catch(err){
                    console.log('error');
                    break;
                }
            }
        }
    ```
    ### 一个字 爽！

