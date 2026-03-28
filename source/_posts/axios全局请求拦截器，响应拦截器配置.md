---
title: axios全局请求拦截器，响应拦截器配置
date: 2024-07-12 10:36:01
tags:
---

# axios全局请求拦截器，响应拦截器配置

直接上代码

```js
import axios from 'axios'
import { useUserStore } from '@/stores/index'
import { ElMessage } from 'element-plus'
import router from '@/router'
const isDev = process.env.NODE_ENV === 'development'

const instance = axios.create({
	// 基地址
  baseURL: isDev ? 'http://localhost:8081/api' : '生产环境地址',
  // 请求超时时间
  timeout: 100000
})

// 请求拦截器
instance.interceptors.request.use(
  (config) => {
    // 请求前可以做一些东西 比如携带token什么的
    // 携带token
    const userStore = useUserStore()
    if (userStore.token) {
      config.headers.Authorization = userStore.token
    }
    return config
  },
  (err) => Promise.reject(err)
)

// 响应拦截器
instance.interceptors.response.use(
  (res) => {
    // 处理业务成功 如果code=1代表成功
    if (res.data.code === 1) {
      return res.data
    }
    // 未登录则跳转到登录页 如果code=40100代表未登录
    if (res.data.code === 40100) {
        router.push('/login')
    }
    // 处理业务失败
    ElMessage({ message: res.data.message || '服务异常', type: 'error' })
    return Promise.reject(res.data)
  },
  (err) => {
    // 响应错误可以做一些东西 比如打印提示信息，跳转到登录页等
    // 处理401错误
    ElMessage({
      message: err.response.data.message || '服务异常',
      type: 'error'
    })
    // 401状态跳转到登录页
    if (err.response.status === 401) {
      router.push('/login')
    }
    return Promise.reject(err)
  }
)

export default instance
export { baseURL }
```

