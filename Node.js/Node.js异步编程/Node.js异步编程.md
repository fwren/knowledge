## 同步 API

只有当前 API 执行完成后，才能继续执行下一个API

```javascript
console.log('开始执行的内容');
console.log('最后执行的内容');
```

---

## 异步 API

当前 API 的执行不会阻塞后续代码的执行

```javascript
console.log('开始执行的内容');
setTimeout(() => {
    console.log('最后执行的内容');
}, 2000);
console.log('之后执行的内容');
```

> 同步 API 和异步 API 的最大区别是同步 API 可以从返回值中拿到 API 的执行结果，但是异步 API 不可以

- 示例

    ```javascript
    // 同步
    function sum (n1, n2) {
        return n1 + n2;
    }
    const result = sum(10, 20);  // result = 30

    // 异步
    function getMsg() {
        setTimeout(() => {
            return {
                msg : 'Hello Node.js!'
            }
        }, 2000);
    }
    const msg = getMsg();   // msg = undefined
    ```

### 回调函数

表示方法

```javascript
function get(callback){
    callback();
}

get(function() {
    console.log('回调函数');
})
```

异步 API 执行的结果可以通过回调函数的方式获得

例如

```javascript
function getMsg(callback) {
    setTimeout(() => {
        callback({
            msg : 'Hello Node.js!'
        });
    }, 2000);
};

getMsg(function(data) {
    console.log(data);
})
```