

### idea删除module变灰色问题

https://blog.csdn.net/weixin_42884584/article/details/82156184

- 造成这个的原因可能是忽略了maven模块||将 一个web 子项目 删除后，重新创建后就变灰了
- 可以尝试如下解决方法：
    
    1）在idea中maven的setting中找到ignored files,看右边的面板中是否将变灰的maven模块忽略了。
    
    2）我的模块变灰就是因为这个原因，Settings–>Maven–>Ignored Files 看看是不是有勾选的。去掉就好了
- 可能导致的问题

    1）模块加了dependency不起作用