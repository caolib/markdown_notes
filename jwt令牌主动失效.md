---
title: redis二次校验实现jwt主动“失效”
date: 2024-02-19 17:34:34
categories: 后端
tags: 
  - redis
  - jwt
---

> 问题：jwt令牌一旦生成，就不能再更改，有时候想让令牌提前失效该怎么办？
>
> 解决方案：使用redis对token进行二次校验，由redis来管理token的过期时间

## 1.保存token到redis

在用户登录方法中，生成token，并保存一份到redis中

```java
@Override
public Result<ReaderVo> login(LoginDto reader) {
    Reader r = readerMapper.selectByName(reader.getUsername());
    //用户不存在
    if (r == null) {
        return Result.error(Excep.USER_NOT_EXIST);
    }
    String pwd = r.getPassword();
    //密码错误
    if (!pwd.equals(reader.getPassword())) {
        return Result.error(Excep.WRONG_PASSWORD);
    }

    //生成令牌,在有效载荷中存储用户名和id
    Map<String, Object> claims = new HashMap<>();
    claims.put(Common.ID, r.getId());
    claims.put(Common.USERNAME, reader.getUsername());
    String token = JwtUtils.generateJwt(claims);

    // 将令牌保存到redis中
    redisTemplate.opsForValue().set(token, token, Jwt.EXPIRE_TIME);

    // 封装信息并返回
    ReaderVo readerVo = new ReaderVo();
    readerVo.setToken(token);
    BeanUtils.copyProperties(r, readerVo);

    return Result.success(readerVo);
}
```

## 2.二次校验token

在jwt拦截器中，校验redis中是否存在token，如果不存在，说明token已经过期

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
    // 静态资源直接放行
    if (!(handler instanceof HandlerMethod)) {
        return true;
    }

    //token为空,不放行
    String token = request.getHeader(Common.TOKEN);
    log.debug("token:{}", token);
    if (!StringUtils.hasLength(token)) {
        log.error(Excep.TOKEN_NOT_EXIST);
        response.setStatus(Code.NOT_LOGIN_CODE);
        return false;
    }

    try {
        // 校验redis中的令牌是否过期
        String redisToken = redisTemplate.opsForValue().get(token);
        if (redisToken == null) {
            throw new BaseException(Excep.TOKEN_ALREADY_EXPIRED);
        }
        //解析
        Claims claims = JwtUtils.parseJWT(token);
        // 保存用户信息
        ThreadLocalUtil.set(claims);
        String format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(claims.getExpiration());
        log.debug("id:{},username:{},令牌到期时间:{}", claims.get(Common.ID), claims.get(Common.USERNAME), format);
    } catch (Exception e) {
        //解析失败不放行
        String message = e.getMessage();
        log.error("非法令牌 " + message);
        if (message.contains("expire")) {
            log.error("令牌过期!");
        }
        response.setStatus(Code.IDENTITY_EXPIRES);
        return false;
    }
    return true;
}
```

## 3.创建删除token接口

创建一个logout退出登录接口，前端退出登录时调用此接口，将token从redis中删除

```java
@DeleteMapping("/logout")
public Result<String> logout(@RequestHeader(Common.TOKEN) String token) {
    log.debug("token:{}", token);

    redisTemplate.delete(token);
    return Result.success();
}
```

## 4.前端调用

### 4.1 定义登出接口，调用后端

```js
import request from "@/util/request.js";
// 用户退出登录
const logoutService = function () {
    // 调用后端接口清除redis中的令牌
    return request.delete('/logout');
}

export {logoutService}
```

### 4.2 登出功能中调用上面接口

```js
const logout = async () => {
  await logoutService();
  ElMessage.success("已退出登录!")
  // 退出后清除token、reader和admin信息
  tokenStore.setToken(null);
  readerStore.clearReader();
  adminStore.clearAdmin();
  await router.push("/login");
};
```

## 5.联调测试
