## 个人博客系统技术亮点

### 页面效果：

主页面：

![image-20220710202031794](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710202031794.png)

![image-20220710202231419](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710202231419.png)

分类标签等：

![image-20220710202109911](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710202109911.png)

归档：

![image-20220710202122923](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710202122923.png)

写文章：

![image-20220710202202021](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710202202021.png)



----------



- ==jwt + redis 使用token令牌的登录==，访问认证速度快，实现session共享，安全性较高。 redis做了token令牌和用户信息的对应管理。1. 访问接口token验证，进一步增加了安全性 2. 登录用户做了缓存 3. 灵活控制用户的过期时间（可以续期，踢掉线等）

> jwt 可以生成 一个==加密的token==，做为用户登录的令牌，当用户登录成功之后，发放给客户端。
> 当请求需要登录的资源或者接口的时候，将token携带，后端验证token是否合法。
>
> 首先在登陆之前在redis数据库中对数据进行查询，看是否存在该条数据，==如果不存在的话，就去数据库查找，然后在查找到之后，在正常登录的时候将数据存储到redis中，当然这个存储信息的键值对也就是在redis查询的那个数据==，然后下次如果再次执行访问的时候，在redis中就有了此数据，进而提高了访问的效率。细节：存储用户的登录信息，存储在redis中的时候使用的是hash数据结构，【hash数据结构其实就是，对应的键值对的值是一个字典类型。】此时就可以将用户携带的唯一标识作为值的键，将用户的其他某个信息作为该键的值存储起来。
>
> 
>
> ```java
> private RedisTemplate<String, String> redisTemplate;
> redisTemplate.opsForValue().set("TOKEN_"+token, JSON.toJSONString(sysUser),1, TimeUnit.DAYS);
> ```



- ==ThreadLocal 本地线程变量==保存用户信息副本到每一个线程中【线程封闭，不会出现线程并发安全问题】。

    在请求的线程之内，可以随时获取登录的用户，在使用完ThreadLocal之后，**做了对value的删除，防止了内存泄漏**。

    【内存泄露】

    > `ThreadLocalMap` 为 `ThreadLocal` 的一个静态内部类，里面定义了`Entry` 来保存数据。而且是继承的弱引用。在`Entry`内部使用`ThreadLocal`作为`key`，使用我们设置的`value`作为`value`。
    >
    > 如果 `key threadlocal` 为 `null` 了，这个 `entry` 就可以清除了。
    >
    > ThreadLocalMap中的key为ThreadLocal的弱引用，当key为`null`时，ThreadLocal会被当成垃圾回收 。
    >
    > ```java
    > //首先获取当前线程对象
    > Thread t = Thread.currentThread();
    > //获取线程中变量 ThreadLocal.ThreadLocalMap
    > ThreadLocalMap map = getMap(t);	//弱引用
    > ```
    >
    > **虽然ThreadLocalMap的key没了，但是value还在，这就造成了内存泄漏。**

![image-20220710175107187](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710175107187.png)



- 线程安全 update table set value = [newValue] where id=[xx] and value=[oldValue]

    - > **CAS**：CAS是Compare And Swap的简称，即：比较并交换。这是当前的处理器基本都支持的一种指令。每个CAS指令包括三个运算符，一个内存地址V，一个期望值A和一个新值B，CAS指令执行的时候是去判断这个地址V上的值和期望值A是否相等，相等则将地址V上的值修改为新值B，不等则不作任何操作。CAS操作实际实现是在一个循环中不断执行CAS指令，直到成功为止。

    - 线程池**@Async** 对当前的主业务流程无影响的操作，放入线程池执行。

        比如加载文章详情和文章阅读数更新，这两个业务流程需要分开执行，互不影响。



- 文章发布 ==@Transactional 事务处理==。



- 后台权限管理系统 **Spring Security**

    - Role-Based Access Control：基于角色的访问控制）模型 RBAC

    ![image-20220711160946539](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220711160946539.png)

    - **用户 -> 角色- > 权限**
    - 由具体到抽象，一个用户（可以理解为账号）可以对应多个角色，一个角色可以有多个权限
    - ==创建用户（账号） -> 为用户绑定角色 -> 角色对应不同的权限【可以对权限进行微调】==


![image-20220711160737540](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220711160737540.png)

![image-20220710202726918](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710202726918.png)



- 统一日志记录
- 定义切点、实现切面

![image-20220710203446152](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710203446152.png)

- 统一缓存处理
    - 定义切点、实现切面

![image-20220710203703333](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710203703333.png)

- 统一异常处理
    - @ControllerAdvice 对所有带Controller注解的类实施异常拦截

![image-20220710203759963](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710203759963.png)

- 统一登录拦截
    - 先写Handler拦截类，再到WebConfig类配置生效

![image-20220710204028104](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710204028104.png)

![image-20220710204301660](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220710204301660.png)

- 分布式ID