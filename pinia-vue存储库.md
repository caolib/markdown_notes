---
title: pinia-vue存储库
date: 2024-02-20 01:01:31
categories: 
  - 前端
  - vue
tags:
  - pinia
cover: https://s2.loli.net/2024/12/30/kAezIathYTQs8cn.png
---

## [Pinia](https://pinia.vuejs.org/zh/)

> [!note]
>
> Pinia是什么？
>
> Pinia 是 ==Vue 的存储库==，它允许您跨组件/页面共享状态
>
> vue项目中有很多页面view，这些view之间相互独立，登录页面会拿到后端传回的token，但是其他页面并没有token，可以将token保存在`pinia`，其他页面都可以访问pinia(相当于全局变量)，另外pinia基于内存存储，刷新浏览器数据就会丢失，使用`persist`插件可以将数据==持久化==

![](https://s2.loli.net/2024/09/27/dThzcYPij8eMZWb.png)

## 1.安装

用你最喜欢的包管理器安装 `pinia`：

```shell
yarn add pinia
# 或者使用 npm
npm install pinia
```

安装pinia持久化插件`persist`

```shell
yarn add pinia-persistedstate-plugin
# 或者使用 npm
npm install pinia-persistedstate-plugin
```

## 2.使用

2.1 在`main.js`中导入pinia和persist并使用

```js
import {createApp} from 'vue'
import App from './App.vue'

import {createPinia} from "pinia";
import {createPersistedState} from "pinia-persistedstate-plugin";

const app = createApp(App)

// 使用pinia和persist保存状态并持久化
const pinia = createPinia();
const persist = createPersistedState();
pinia.use(persist);

// 使用pinia
app.use(pinia);

app.mount('#app')
```

2.2 以保存token为例，创建`token.js`文件

```js
import {defineStore} from "pinia";
import {ref} from 'vue';

export const useTokenStore = defineStore('token', () => {
        //1.定义描述token
        const token = ref('')
        //2.定义修改token的方法
        const setToken = (newToken) => {
            token.value = newToken
        }
        //3.定义移除token的方法
        const removeToken = () => {
            token.value = ''
        }
        return {
            token, setToken, removeToken
        }
    },
    {
        //使用persis插件持久化
        persis: true
    }
)
```

2.3 在其他页面读取token

```js
import { useTokenStore } from "@/stores/token";

const tokenStore = useTokenStore();

// 设置token
tokenStore.setToken('your token');
// 获取token
console.log("tokenStore:" + tokenStore.token);
// 清除token
tokenStore.removeToken();
```

## 3.其他示例

`reader.js`

```js
import {defineStore} from "pinia";
import {ref} from 'vue';

/**
 * 保存登录时用户的信息
 */
export const useReaderStore = defineStore('reader', () => {
        const reader = ref({
            id: '',
            username: '',
            nickname: '',
            gender: '',
            age: '',
            tel: '',
            token: ''
        });
        const setReader = (data) => {
            reader.value = data;
        }
        //清除信息
        const clearReader = () => {
            reader.value.id = '';
            reader.value.username = '';
            reader.value.nickname = '';
            reader.value.gender = '';
            reader.value.age = '';
            reader.value.tel = '';
            reader.value.token = '';
        }
        return {
            reader, setReader, clearReader
        }
    },
    // 持久化，pinia保存在内存中，刷新即丢失
    {
        persis: true
    }
)
```

`admin.js`

```js
import {defineStore} from "pinia";
import {ref} from 'vue';

/**
 * 保存登录时的管理员信息
 */
export const useAdminStore = defineStore('admin', () => {
        const admin = ref({
            id: '',
            username: '',
            nickname: '',
            token:''
        });
        // 是否为管理员
        let isAdmin = ref();
        const setIsAdmin = (flag)=>{
            isAdmin.value = flag;
        }
        const setAdmin = (data) => {
            admin.value = data;
        }
        //清除信息
        const clearAdmin = ()=>{
            admin.value.id = '';
            admin.value.username = '';
            admin.value.nickname = '';
            admin.value.token = '';
        }

        return {
            admin, setAdmin,isAdmin,setIsAdmin,clearAdmin
        }
    },
    {
        persis: true
    }
)
```

