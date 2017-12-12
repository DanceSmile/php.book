## php  密码hash

```PHP
$password = "phpHashBcryptWithconst";
$hash_password = password_hash($password,PASSWORD_DEFAULT,["cost"=>12]);

echo $hash_password;

$verify = password_verify($password,$hash_password);

var_dump($verify);
```

## 对象基础

## 反射

PHP反射可以分析属性，函数和对象，它是由一系列内置的类组成

### 获取类的信息

使用反射类可以获取一个类的更多细节

```PHP
//定义一个简单的类
class Animal {

    private  $name;
    public  $age;

     /**
     * @return 获取$name
     */
    public function getName()
    {
        return $this->name;
    }
    public function run()
    {

        echo $this->name . "is running";

    }
}

//实例化一个反射类，反射类的构造函数接受一个类名作为它的唯一参数
$reflection =  new ReflectionClass("Animal");

//使用Reflection工具类的静态方法可以格式化输出反射对象的管理数据
//相比于var_dump 或者 var_export ，使用Reflection可以打印出更多的class细节，并且不需要实例化
Reflection::export($reflection);

```

### 分析类


>  使用 ReflectionClass 分析类


```PHP
//实例化一个反射类，反射类的构造函数接受一个类名作为它的唯一参数
$reflection =  new ReflectionClass("Animal");

//使用Reflection工具类的静态方法可以格式化输出反射对象的管理数据
Reflection::export($reflection);

// 分析类

//返回检查的类名
$class_name = $reflection->getName();
var_dump($class_name);

//检测类是否是用户定义
$is_user_define = $reflection->isUserDefined();
var_dump($is_user_define);

检测类是否是内置的类
$is_built_in = $reflection->isInternal();
var_dump($is_built_in);

//检测类是否是否是抽象类
$is_abstract = $reflection->isAbstract();
var_dump($is_abstract);

//检测类是否是接口类
$is_interfalce =$reflection->isInterface();
var_dump($is_interfalce);

//检测类是否是最终类
$is_final = $reflection->isFinal();
var_dump($is_final);

//检测类是否可以实例化
$is_able_instance = $reflection->isInstantiable();
var_dump($is_able_instance);

```

### 分析方法

>  使用 ReflectionMethod 分析方法


```PHP
//获取分析类的所有方法，返回一个 ReflectionMethod类型的数组
$fun_arr = $reflection->getMethods();
var_dump($fun_arr);

//获取方法名
$fun_name = $fun_run->getName();
var_dump($fun_name);

//获取特定的方法，返回一个 ReflectionMethod类型
$fun_run = $reflection->getMethod("run");
var_dump($fun_run);

//检测是否是用户定义
$is_user_define = $fun_run->isUserDefined();
var_dump($is_user_define);

//检测是否是内置方法
$is_built_in = $fun_run->isInternal();
var_dump($is_built_in);

//检测是否是抽象方法
$is_abstract  = $fun_run->isAbstract();
var_dump($is_abstract);

//检测是否是公开
$is_public = $fun_run->isPublic();
var_dump($is_public);

//检测是否是私有方法
$is_private = $fun_run->isPrivate();
var_dump($is_private);

//检测是否是受保护的方法
$is_protected = $fun_run->isProtected();
var_dump($is_protected);

//是否是最终方法
$is_final = $fun_run->isFinal();
var_dump($is_final);

//是否是构造方法
$is_constract = $fun_run->isConstructor();
var_dump($is_constract);

//检测是否是引用方法
$is_reference = $fun_run->returnsReference();
var_dump($is_reference);

```

### 分析方法参数

>  使用 ReflectionParameter 检测方法参数

```PHP

//获取方法的所有参数对象
$params = $fun_run->getParameters();
var_dump($params);

// 获取第一个参数对象
$distance = $params[0];
var_dump($distance);

//获取参数名
$param_name = $distance->getName();
var_dump($param_name);

//获取参数的类型限制
$class = $distance->getClass();
var_dump($class);


//获取参数位置
$position = $distance->getPosition();
var_dump($position);


//参数的默认值
$default_value = $distance->getDefaultValue();
var_dump($default_value);


//是否是引用参数
$is_reference = $distance->isPassedByReference();
var_dump($is_reference);

```
### 使用反射

使用反射实例化类,并且执行实例里的方法

```PHP
class Person{

    function greeting()
    {
        echo "hello !\n";
    }
}

// 获得一个反射类
$reflection = new  ReflectionClass("Person");

// 实例一个person类
$person = $reflection->newInstance(); //newInstanceArgs可以实例传入 构造函数的参数

// 执行方法
$greeting = $reflection->getMethod("greeting");
$greeting->invoke($person);

```
## 设计模式


> php常用的基础知识总结，不断更新

!> 一段重要的内容，可以和其他 **Markdown** 语法混用。


?> *TODO* 完善示例

[link](/demo/)

[link](/demo/ ":ignore")

### 单例模式

```PHP
/**
 * 构造一个配置类
 */
class Config {

    // 项目的配置项属性
    private $props = array();

    // 类的实例
    private static  $instance =  null ;

    // 将构造函数设置成私有，防止外部实例化
    private  function  __construct(){ }

    // 内部实例化对象，单例防止多次实例化
    public  static function getInstance()
    {
        if( !self::$instance ){
            self::$instance = new Config();
        }
        return self::$instance;
    }

    // 设置属性
    public  function  setProperty( $key, $value )
    {
        $this->props[$key] = $value;

    }
    // 返回属性
    public  function getProperty( $key )
    {
        return $this->props[$key] . "\n";
    }


}

// 实例化配置1⃣，设置配置值
$prop1 = Config::getInstance();
$prop1->setProperty("name","test");

// 实例化配置2，配置项目值
$prop2 = Config::getInstance();
$prop2->setProperty("age","12");

// 共享配置
echo $prop1->getProperty("name");
echo $prop2->getProperty("name");

echo $prop1->getProperty("age");
echo $prop2->getProperty("age");

```

> 静态方法不可以访问对象属性，但是可以访问一个静态属性

### 适配器模式

```PHP
/**
 * 适配器模式
 * 适配器模式就是将某个对象(ErrorObject)的接口适配成另一个对象所期望的接口(LogToExcelAdapter)
 */
/**
* error class
*/	
class  ErrorObject
{
	private $_error;
	public function __construct($error)
	{
		$this->_error = $error;
	}

	public function getError()
	{
		return $this->_error;
	}
}

/**
* log console
*/
class LogToConsole
{
	
	private $_errorObj;
	function __construct(ErrorObject $error)
	{	
		$this->_errorObj  = $error;
	}

	public function write()
	{
		echo $this->_errorObj->getError()."\n";	

		return $this->_errorObj->getError();
	}
}

/**
* 
*/
class LogToExcel 
{
	private $_errorObj;

	public function __construct(ErrorObject $error)
	{
		$this->_errorObj = $error;
	}

	public function write()
	{
		$line = $this->_errorObj->getCode();
		$line .=  "====";
		$line .= $this->_errorObj->getMessage() ."\n";
		echo $line;
		return $line;
	}

}

/**
* 
*/
class LogToExcelAdapter extends ErrorObject
{
	private $_errorCode;
	private $_errorMessage;	
	function __construct($error)
	{
		parent::__construct($error);
		$parts = explode(":", $this->getError());
		$this->_errorCode = $parts[0];
		$this->_errorMessage = $parts[1];
	}
	public function getCode()
	{
		return $this->_errorCode;
	}
	public function getMessage()
	{
		return $this->_errorMessage;
	}
}


$error = new ErrorObject("404 : not found");

$log_to_text = new LogToConsole($error);

$log_to_text->write();


$error_adapter = new LogToExcelAdapter("404 : not found");
$log_to_excel = new LogToExcel($error_adapter);


$log_to_excel->write();

```

### 建造者模式

```PHP
/**
* 建造者模式
*/
class Product 
{

	protected  $_name = "";
	protected  $_size = "";
	protected  $_color = "";

	public function setSize($size)
	{
		$this->_size = $size;
	}
	public function setName($name)
	{
		$this->_color = $name;
	}
	public function setColor($color)
	{
		$this->_color = $color;
	}
	
	
}


/**
*  product 建造类
*/
class ProductBuilder
{

	private $_product = NUll;
	private  $_configs = array();
	
	public 	function __construct($configs)
	{
		$this->_product = new Product();
		$this->_configs = $configs;
	}

	public function build()
	{
		$this->_product->setName($this->_configs["name"]);
		$this->_product->setSize($this->_configs["size"]);
		$this->_product->setColor($this->_configs["color"]);
	}


	public function getProduct()
	{
		return $this->_product;
	}
}

$configs = array("name" => "build" , "size" => 90 , "color" => "red" );
$builder = new ProducrBuilder( $configs );
$builder->build();
$product = $builder->getProduct();
var_dump($product);
```
### 数据库访问模式

```PHP
/**
* 数据访问对象模式
*/
abstract class Model
{
	protected $_connection = NULL;
	public function __construct(argument)
	{

	}

	private function _connectDb( $user, $password, $host, $port , $database )
	{
		
	}
}

```
