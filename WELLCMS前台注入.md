### ***\*WELLCMS前台注入\****

WellCMS 是一款具备亿级负载、开源、倾向移动端、轻量级、具有超快反应能力的高负载CMS，是大数据量、高并发访问网站最佳选择的轻CMS。wellcms具有安全、高效、稳定、速度超快、负载超强的特点。是大数据时代下诞生的CMS，低成本解决网站负载和性能问题，专为大数据量站点设计的高性能、高负载的CMS。前后台均可在移动端操作，自适应手机、平板、PC，也可以设置各端加载单独模板，并且URL保持不变，有着非常方便的插件机制。前台部分页面配备API，可通过JSON返回AJAX请求的数据，方便 APP 开发。



0x1 漏洞成因:

\wellcms\xiunophp\db.func.php文件,db_cond_to_sqladd函数在处理键的时候只要键不为LIKE那么则直接拼接键值

[![图片1.png](http://www.ttk7.cn/content/uploadfile/202010/b6c31602477592.png)](http://www.ttk7.cn/content/uploadfile/202010/b6c31602477592.png)





因为程序安装的时候默认是PDO模式所以找到PDO类文件中的Find方法

[![图片2.png](http://www.ttk7.cn/content/uploadfile/202010/71421602477616.png)](http://www.ttk7.cn/content/uploadfile/202010/71421602477616.png)





于是调用了find方法的就会调用db_cond_to_sqladd, 所以找到了db_find方法

[![图片3.png](http://www.ttk7.cn/content/uploadfile/202010/05261602477640.png)](http://www.ttk7.cn/content/uploadfile/202010/05261602477640.png)





所以调用了db_find方法的都会调用find方法,再调用到db_cond_to_sqladd造成注入,现在找调用了这些方法的就好。

 

0x2 漏洞利用点:

[![图片4.png](http://www.ttk7.cn/content/uploadfile/202010/1ed01602477669.png)](http://www.ttk7.cn/content/uploadfile/202010/1ed01602477669.png)





继续跟下去

[![图片5.png](http://www.ttk7.cn/content/uploadfile/202010/95101602477692.png)](http://www.ttk7.cn/content/uploadfile/202010/95101602477692.png)



[![图片6.png](http://www.ttk7.cn/content/uploadfile/202010/560d1602477709.png)](http://www.ttk7.cn/content/uploadfile/202010/560d1602477709.png)

[![图片7.png](http://www.ttk7.cn/content/uploadfile/202010/1e1b1602477727.png)](http://www.ttk7.cn/content/uploadfile/202010/1e1b1602477727.png)



0x3 漏洞证明

所以这里会调用到db_find方法继而调用db_cond_to_sqladd,造成注入

[![图片8.png](http://www.ttk7.cn/content/uploadfile/202010/914f1602477748.png)](http://www.ttk7.cn/content/uploadfile/202010/914f1602477748.png)