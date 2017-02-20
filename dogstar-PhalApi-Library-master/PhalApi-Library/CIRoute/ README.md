#PhalApi-CIRroute移植于CI框架的路由拓展
###前言
因为PhalApi框架自己在工作之外会使用到,但是PhalApi框架本身的路由功能不强,所以从CI上移植了该路由,删去了原路由本身的文件检测部分,将路由原本复杂的配置统一到PhaliApi的配置文件中.
附上:

官网地址:http://www.phalapi.net/

开源中国Git地址:http://git.oschina.net/dogstar/PhalApi/tree/release

开源中国拓展Git地址:http://git.oschina.net/dogstar/PhalApi-Library

###1.安装
配置方式非常简单只需要把拓展下载下来放入Library文件内即可,然后就可以使用如下方法进行实例
```php
//显式初始化，并调用分发
DI()->CIRoute = new CIRoute_Lite();
DI()->CIRoute->dispatch();
```
### 2.配置
配置文件存放在 app.CIRoute
```php
//默认配置如下
/***
     * 扩展类库 CI路由配置
	 */
	'CIRoute'=>array(
		'config'=>array(
			'default_controller'=>'default',//默认控制器
			'uri_protocol'	=> 'REQUEST_URI',
			'url_suffix' => '.html',//默认后缀
			//uri
			'permitted_uri_chars' => 'a-z 0-9~%.:_\-',
			'allow_get_array'=>true,
			'enable_query_strings'=>false,
			'controller_trigger'=>'c',
			'function_trigger'=>'m',
			'directory_trigger'=>'d',
			//class
			'subclass_prefix'=>'',
		),
		'routes'=>array()
	)
```
CIRroute 中的 config为路由的初始配置,routes存放自定义路由配置.
###使用实例
>使用实例 1. CIRoute默认路由支持优雅链接
http://localhost:8080/stackheap/phalapi-release/Public/index.php/default/index2/id/2
此链接可以访问到本机下default控制器下的index2方法,并传入一个参数id=2;
等同于
http://localhost:8080/stackheap/phalapi-release/Public/index.php/default/index2?id=2
>使用实例 2.CIRroute 的enable_query_strings模式
设置config配置中的
```
'enable_query_strings'=>true
```
开启enable_query_strings模式
想要成功访问上面的链接,必须访问
http://localhost:8080/stackheap/phalapi-release/Public/index.php?c=default&m=index2&id=2
路径中的c和m分别表示类和方法,可以通过设置config配置的
```
'controller_trigger'=>'c',
'function_trigger'=>'m',
```
修改为理想中的关键词
>使用实例 3.后缀. 支持实现伪静态化的后缀
修改config配置中的
```
'url_suffix' => '.html',//默认后缀
```
可以修改路由路径的后缀
访问上述方法:
http://localhost:8080/stackheap/phalapi-release/Public/index.php?c=default&m=index2&id=2.html
###自定义路由配置
>自定义路由配置部分可以参考CI官方文档,目前只作部分实例讲解
ci_url文档http://codeigniter.org.cn/user_guide/general/routing.html

### 实例一
设置路由配置中的routes部分
```
'routes'=>array('product/:num' => 'default/index2')
```
此配置可以实现访问任意product/数字的方式匹配到default控制器下的index2方法
(自定义路由配置时,传参必须使用?query_string的方式)
如:http://localhost:8080/stackheap/phalapi-release/Public/index.php/product/2.html?id=2
http://localhost:8080/stackheap/phalapi-release/Public/index.php/product/2?id=2
### 实例二,路由配置中添加参数
设置路由配置中的routes部分
```
//支持多个自定义路由
'routes'=>array('product/:num' => 'default/index2',
				'login/(:any)'=>'default/index2/id/$1'
)
```
访问路径http://localhost:8080/stackheap/phalapi-release/Public/index.php/login/2
####可以实现访问上诉链接的效果
