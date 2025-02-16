---
title: axios不入门直接起飞
date: 2024-02-19 15:23:51
categories: 
    - 前端
    - vue
tags: axios
---

## [Axios](https://www.axios-http.cn/)

> Axios是什么？
>
> Axios 是一个基于 *[promise](https://javascript.info/promise-basics)* 网络请求库，作用于[node.js](https://nodejs.org/) 和浏览器中。 它是 *[isomorphic](https://www.lullabot.com/articles/what-is-an-isomorphic-application)* 的(即同一套代码可以运行在浏览器和node.js中)。在服务端它使用原生 node.js `http` 模块, 而在客户端 (浏览端) 则使用 XMLHttpRequests

## 1.安装

使用npm安装：

```sh
npm install axios
```

使用yarn安装：

```sh
yarn add axios
```

## 2.包装统一请求工具

因为后端地址是一样的，假设是localhost:8080，只是请求路径不一样，我们可以定义一个baseURL，此处使用`/api`是为了解决**跨域问题**

1.先包装一个工具`request.js`

```js
import axios from "axios";

const baseURL = "/api";
const instance = axios.create({baseURL});
export default instance;
```

2.在`vite.config.js`文件中添加配置，将`/api`删除，替换为`http://localhost:8080`,这样就相当于使用前端服务发送请求而不是浏览器，解决了跨域请求问题

```js
export default defineConfig({
    plugins: [
        vue(),
        AutoImport({
            resolvers: [ElementPlusResolver()],
        }),
        Components({
            resolvers: [ElementPlusResolver()],
        }),
    ],
    resolve: {
        alias: {
            '@': fileURLToPath(new URL('./src', import.meta.url))
        }
    },
    //代理http请求，解决跨域问题
    server: {
        host: 'localhost',
        port: 5173,
        proxy: {
            '/api': { //匹配请求路径中含有 /api 的请求
                target: 'http://localhost:8080', //后端服务地址
                changeOrigin: true,
                rewrite: (path) => path.replace(/^\/api/, '') //去除路径中的/api，还原请求路径
            }
        }
    },
})
```

## 3.拦截器

这是axios发送请求的一个示例，项目中会有很多这样的请求要发送，我们会发现发送请求和接收响应数据之前，我们都会做一些相同的事情，比如，**发送请求之前我们都会给请求头中带上token**，**接收响应时我们都会先判断状态码**，我们不妨将这些动作用一个统一的函数来实现，这就需要使用拦截器

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

这是一个响应拦截器的示例，axios会自动适配响应的http**状态码为4xx或5xx的请求为失败回调**，在失败回调中我们可以**对各种状态码的失败回调统一处理**，成功回调中如果自定义的code为0也表示有错误，这种统一处理的方式类似于Spring中的`AOP`

```js
//响应拦截器，状态码为2xx时执行成功回调，否则执行失败回调
instance.interceptors.response.use(
    //成功回调
    (result) => {
        // 如果状态码为0，后端发生异常
        if (result.data.code = 0) {
            ElMessage.error(result.data.msg);
            return Promise.reject(result);
        }
        return result.data;
    },
    //失败回调
    (error) => {
        // 状态码为401,419都跳转到登录界面
        if (error.response) {
            const code = error.response.status;
            if (code = 401) {
                ElMessage({message: '请先登录！', type: "error",});
                router.push('/login');
            } else if (code = 419) {
                ElMessage.error("身份已过期,请重新登录！");
                router.push('/login');
            } else {
                ElMessage.error("服务器异常！" + code);
            }
        }
        // 将异步的状态设置为失败状态
        return Promise.reject(error);
    }
);
```

这是一个请求拦截器的示例，在发送请求之前先判断是否有token，如果有就在请求头上带上token再发送请求，否则跳转到登录页面，当然，**登录和注册请求都不需要token**，可以直接发送请求

```js
// 请求拦截器
instance.interceptors.request.use(
    (config) => {
        //登录请求不需要token
        if (config.url.endsWith('/login') || config.url.endsWith('/register')) {
            return config;
        }
        //如果有token，将token放入请求头中
        const token = tokenStore.token;
        if (token != null) {
            config.headers['token'] = token;
        } else {
            router.push('/login');
            ElMessage({message: '请先登录！', type: "error",});
            return Promise.reject('token不存在！');
        }
        return config;
    },
    (err) => {
        //请求错误的回调
        return Promise.reject(err);
    }
);
```

## 4.完整axios包装工具示例

```js
import axios from "axios";
import {ElMessage} from "element-plus";
import router from "@/router";
import {useTokenStore} from "@/stores/token.js";

const baseURL = "/api";
const instance = axios.create({baseURL});
const tokenStore = useTokenStore();


//响应拦截器，状态码为2xx时执行成功回调，否则执行失败回调
instance.interceptors.response.use(
    //成功回调
    (result) => {
        // 如果状态码为0，后端发生异常
        if (result.data.code = 0) {
            ElMessage.error(result.data.msg);
            return Promise.reject(result);
        }
        return result.data;
    },
    //失败回调
    (error) => {
        // 状态码为401,419都跳转到登录界面
        if (error.response) {
            const code = error.response.status;
            if (code = 401) {
                ElMessage({message: '请先登录！', type: "error",});
                router.push('/login');
            } else if (code = 419) {
                ElMessage.error("身份已过期,请重新登录！");
                router.push('/login');
            } else {
                ElMessage.error("服务器异常！" + code);
            }
        }
        // 将异步的状态设置为失败状态
        return Promise.reject(error);
    }
);

// 请求拦截器
instance.interceptors.request.use(
    (config) => {
        //登录请求不需要token
        if (config.url.endsWith('/login') || config.url.endsWith('/register')) {
            return config;
        }
        //如果有token，将token放入请求头中
        const token = tokenStore.token;
        if (token != null) {
            config.headers['token'] = token;
        } else {
            router.push('/login');
            ElMessage({message: '请先登录！', type: "error",});
            return Promise.reject('token不存在！');
        }
        return config;
    },
    (err) => {
        //请求错误的回调
        return Promise.reject(err);
    }
);

export default instance;
```

在其他接口文件中导入使用，因为使用的是`export default instance`默认导出的，所以导入时可以自定义名字，这个文件中定义为`request`,其实就是`instance`实例

```js
import request from "@/util/request.js";
// 用户退出登录
const logoutService = function () {
    // 调用后端接口清除redis中的令牌
    return request.delete('/logout');
}

export {logoutService}
```
