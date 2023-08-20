python快速配置IP代理池（ProxyPool）

关注我的公众号【**靠谱杨的挨踢生活**】回复**ProxyPool**可以免费获取网盘链接。
也可自行搜索下载：https://github.com/Python3WebSpider/ProxyPool.git

## 1、下载之后打开setting文件修改redis相关配置。

![image-20220924131533315](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051355930.png)



![image-20220924131603070](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051355932.png)

## 2、之后开启本机redis服务，就可以直接运行run文件

	  可以下载一个 Redis Desktop Manager redis可视化工具，关注我的公众号【小杨的挨踢IT生活】回复**redis**可以获取下载链接（文章末尾有公众号二维码），也可以自行百度下载。

![image-20220924132435133](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051355933.png)

![image-20220924132450103](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051355934.png)

## 3、使用redis中的IP

```python
import random

import redis

class my_redis:

    def get_ip(self):
        r = redis.Redis(host='127.0.0.1', port=6379, db=0,decode_responses=True)
        my_redis_data = r.zrange("proxies:universal",1,3000,True)
        return random.choice(my_redis_data)
        # print(len(my_redis_data))

if __name__ == '__main__':
    test_redis=my_redis()
    data=test_redis.get_ip()
    print(data)
```


![image-20220924132834606](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051355935.png)