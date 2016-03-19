# Promise

## Basic

```javascript
var promise = new Promise(function (resolve, reject) {
  // some code here
  if (/* pending => resolved */) {
    resolve();
  } else { // pending => rejected
    reject();
  }
});

promise.then(function () {
  // success
}, function () {
  // failure
});
```

## Promise.prototype.then()

其实这里有两种情况

1. `promise.then` 的 `resolve` 函数返回的不是 Promise 实例

    `then` 方法返回的是另一个 Promise 实例，所以我们可以继续调用另一个 `then` 方法，参数就是第一个 `then` 里函数返回的结果
    
    ```javascript
    getJSON('/posts.json').then(json => {
      return json.post;
    }).then(post => {
      // some code with `post`
    });
    ```

2. `promise.then` 的 `resolve` 函数返回一个 Promise 实例

    这时，第二个 `then` 的回调就要根据返回的 Promise 实例的状态来决定
    
    ```javascrpt
    getJSON('/posts.json').then(json => {
      return getJSON(json.url);
    }).then(json => {
      // resolved 
    }, json => {
      // rejected
    });
    ```
    
## Promise.prototype.catch()

`Promise.prototype.catch` 是 `.then(null, rejection)` 的别名，用于指定发生错误时执行函数，而且所有错误都会往上抛，直接我们处理为此

所以对错误的统一处理非常方便，只要在所有的 `.then` 最后，加上 `.catch` 就可以了

```javascript
getJSON('/posts.json').then(json => {
  return getJSON(json.url);
}).then(json => {
  // resovled
}).catch(err => {
  // 处理上面三个 promise
});
```

一般来说，不要在 `then` 方法里面定义 reject 状态的函数(即第二个参数)，总是使用 `catch` 方法

同样的 `catch` 方法，也会默认返回一个 Promise 对象，可以让我们接着调用 `then`，这样的作用是，当我们运行完 `catch` 里的函数后，会继续执行 `then` 里面的函数，如果没有错误，就跳过 `catch` 函数

## Promise.all()

`Promise.all` 方法可以将多个 Promise 实例包装成一个新的 Promise 实例

```javascript
let p = Promise.all([p1, p2, p3]);
```

这里 `p` 的状态有两种情况

1. `p1`，`p2` 和 `p3` 状态都变成 resolve，`p` 的状态才变成 resolve，此时 `p1`，`p2`，`p3` 的返回值组成一个数组，传递给 `p` 的 `then` 里
2. 只要 `p1`，`p2` 和 `p3` 其中一个状态为 rejected，`p` 就会变成 rejected，此时第一个被 rejected 的实例的返回值，就会传到 `p` 的 `catch` 里

```javascript
let promises = [2, 3, 5, 7, 11, 13].map(id => {
  return getJSON(`/post/${id}.json`);
});

Promise.all(promises).then(posts => {
  // ...
}).catch(err => {
  // ...
});
```

## Real World

1. 定时器

    ```javascript
    const timeout = ms => new Promise((resolve, reject) => {
      setTimeout(resolve, ms, 'done');
    });
    
    timeout(100).then(value => {
      console.log(value);
    });
    ```
    
2. 异步加载图片

    ```javascript
    const loadImageAsync = url => new Promise((resolve, reject) => {
      var image = new Image();
      image.onload = () => {
        resolve(image);
      };
      image.onerror = () => {
        reject(new Error(`Could not load image at ${url}`));
      };
      image.src = url;
    });
    
    loadImageAsync('/images/jason.png').then(image => {
      document.body.appendChild(image);
    });
    ```
    
3. Ajax

    ```javascript
    const getJSON = url => {
      return new Promise((resolve, reject) => {
        let xhr = new XMLHttpRequest();
        xhr.open('get', url, false);
        xhr.onreadystatechange = () => {
          if (xhr.readyState === 4) {
            if ((xhr.status >= 200 && xhr.status < 300) ||
                xhr.status === 304) {
              resolve(xhr.responseText);
            } else {
              reject(new Error(xhr.statusText);
            }
        }
        xhq.sent(null);
      });
    };
    
    getJSON('/post.json').then(json => {
      console.log(json);
    }).catch(error => {
      console.err('error', error);
    });
    ```