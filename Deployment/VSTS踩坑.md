# VSTS 踩坑

标签（空格分隔）： 运维

---

## 让.netCore 2.0 的测试在 VSTS 中正确运行需要注意几个地方

1. 要增加.netCore 的测试程序集目录；
   ![img](https://images2017.cnblogs.com/blog/282687/201801/282687-20180106102814362-1931048116.png)
2. 设置 Speceific location:`C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\TestPlatform` 设置 Other console options:`/Framework:".NETCoreApp,Version=v2.0"`
   ![img](https://images2017.cnblogs.com/blog/282687/201801/282687-20180106102836815-515444835.png)
3. 默认配置会包含 xunit（我是使用的 xunit）的测试程序集，这个是不需要的，会导致测试过程报错，需要将其排除。
   ![img](https://images2017.cnblogs.com/blog/282687/201801/282687-20180106212604346-548141091.png)
