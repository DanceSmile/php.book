## 密码HASH

```php
$password = 'phpHashBcryptWithconst';

$hash_password = password_hash($password,PASSWORD_DEFAULT,["cost"=>12]);

echo $hash_password;

$verify = password_verify($password,$hash_password);

var_dump($verify);
```

## 对象基础

### 类变量和类方法

```php
/**
 * 静态方法和属性
 * 静态变量和方法的好处在于不需要实例化就可以访问静态属性和方法，
 */
class Person
{
    public  static  $eye_count = 2;
    public  static  $hand_count;

    public  $name="zero";
    // 普通方法
    public function greeting()
    {
        echo self::$eye_count;
        echo "hello !!\n";
    }
    // 静态方法
    public static function go(){
        echo "go home !! \n";
        echo "we are have " . self::$eye_count ."eyes " . "\n";
    }
}


// 可以通过类来访问属性和方法，称之为静态方法或者类方法
// 静态方法不能访问普通属性，因为普通属性是绑定在对象上，但是可以访问静态属性
$eye_count = Person::$eye_count;

var_dump($eye_count);

Person::go();

// 在普通方法中可以调用到静态属性
$zero = new  Person();

$zero->greeting();

// 实例访问
var_dump($zero::$eye_count);

```
### 类常量

```php

/**
 *  常量属性
 *  常量属性是不可变的，一旦定义就没有办法修改
 *  按照管理常量使用const关键字定义，使用大写名，不需要使用美元符号
 *
 *
 * 使用途径
 * 当我们在类中所有的实例都需要访问，并且不可改变的时候就使用常量
 */
class Product
{
    const PRICE = 100;
    const  COLOR = "red";

    public function get_color()
    {
        echo self::COLOR."\n";
    }
}

// 类访问
var_dump(Product::COLOR);
// 类访问
var_dump(Product::PRICE);

$product = new Product();
// 普通方法访问
$product->get_color();
// 实例访问
var_dump($product::PRICE);

```
### 抽象类

```php

/**
 * 抽象类使用abstract关键词定义
 * 抽象类不能够被实例化，可以被继承
 * 抽象类中定义了子类需要的方法
 *
 */
abstract  class Logger
{
    // 抽象方法，继承的子类必须实现找个方法
    abstract  public function write();
}

class Application extends Logger
{
    public  function write()
    {
      // pass
    }
}


$application =  new Application();

var_dump($application);

```

### 接口类

```php

/**
 * Interface Chargeable
 * 定义接口类，接口类定义具体的实现标准
 * 接口类可以包含属性和方法，方法体必须是空
 * 任何实现接口的类都要实现接口类的所有方法，除非是抽象类
 * 实现接口的类接受了它继承的类和实现的接口的类型
 * 可以实现（继承）多个接口类
 */
interface  Chargeable
{
    public function getPrice();
}

abstract class Product{

}

class MilkProduct extends Product implements Chargeable
{
    public  function  getPrice()
    {
        // TODO: Implement getPrice() method.

    }
}

$milk = new MilkProduct();

//判断实例类型
var_dump( $milk instanceof MilkProduct );
var_dump( $milk instanceof Product );
var_dump( $milk instanceof Chargeable );
```

### final类和方法

```php
/**
 * final关键字可以终止类的继承
 * final关键字可以终止类的方法被复写
 */
final class Checkout
{
    //  不能被覆盖
    final public function  test()
    {

    }

}

```

### 延迟绑定

static 不但可以作为静态函数或静态属性的标识，还可以作为当前调用的类并且实例化

static 实例化对象，当被对象调用的时候才会绑定当前对象的属性

```php
class Document
{
    public static  function create()
    {
        // 此时的static表示调用的对象
        return new static();
    }
}

class  PhpDocument extends  Document
{
        //pass
}

class  PythonDocument  extends  Document
{
        //pass
}

$phpDoc = PhpDocument::create();

$pythonDoc = PythonDocument::create();

var_dump($phpDoc);

var_dump($pythonDoc);

```
### 拦截器

```php
/**
 * 拦截器
 */
class People
{
    // 访问一个未定义的属性
    public function __get($name)
    {
        // TODO: Implement __get() method.
        echo $name . "\n" ;
    }

    // 设置一个未定义的属性
    public  function __set($name, $value)
    {
        // TODO: Implement __set() method.
        echo $name ."\n" , $value ."\n";
    }
    //  调用一个未定义的方法
    public  function __call($name, $arguments)
    {
        // TODO: Implement __call() method.

        var_dump($arguments);
        echo $name;
    }
    // 对为定义的属性使用isset
    public function __isset($name)
    {
        // TODO: Implement __isset() method.
        echo $name . "\n";
    }

    // 对未定义的属性使用unset
    public  function __unset($name)
    {
        // TODO: Implement __unset() method.
        echo $name ."\n";
    }

    // 直接输出perospn 对象
    public  function  __toString()
    {
        // TODO: Implement __toString() method.
        return "Propele";
    }

}

$person = new  People();

$person->name = "zero"; //  name  \n zero

$person->age;  // age

$person -> run("zero","mile");  //array("zero","milk")  run

isset($person->eye);  // eye

unset($person->name); // name

echo $person;
```



## 反射

PHP反射可以分析属性，函数和对象，它是由一系列内置的类组成

### 获取类的信息

使用反射类可以获取一个类的更多细节

```php
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


```php
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


```php
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

```php

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

```php
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

```php
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

> 将一个类的接口转换成客户希望的另外一个接口,使用原本不兼容的而不能在一起工作的那些类可以在一起工作。

```php
/*驱动接口*/
interface db_driver
{
    public  function connect();
    public  function query();
}

/*mysql的数据库实现*/
class db_mysql implements db_driver
{

    public function connect()
    {
        /*具体代码实现*/
    }

    public function query($sql)
   {
        /*具体代码实现*/
    }
}
/*pdo的数据库实现*/
class db_pdo implements db_driver
{

    public function connect()
    {
        /*具体代码实现*/
    }

    public function query($sql)
    {
        /*具体代码实现*/
    }
}
/*定义适配器类*/
class db_adapter
{

    private $db;

    public function __construct($db_obj)
    {
        $this->db = $db_obj;
    }

    public function connect()
    {
        $this->db->connect();
    }

    public function query($sql)
    {
        $this->db->query($sql);
    }
}

/*客户端应用*/
$db = new db_adapter(new db_mysql());
```

```php
/**
 * 适配器模式
 * 适配器模式就是将某个对象(ErrorObject)的接口适配成另一个对象所期望的接口(LogToExcelAdapter)
 */
class  ErrorObject
{
	private $_error;

  // 传入实例信息
	public function __construct($error)
	{
		$this->_error = $error;
	}

  // 获取错误信息
	public function getError()
	{
		return $this->_error;
	}
}

/**
* 输出错误类
*/
class LogToConsole
{

	private $_errorObj;

  // 存入错误类的实例
	public function __construct(ErrorObject $error)
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
* 输出excel对象
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
*  适配excel的接口
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

> 创建一个很复杂的对象可以使用创建者模式

```php
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

	public function __construct($configs)
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

```php
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

### 观察者模式

```php
/**
 * 一个购买牛奶的例子；
 * 购买的动作会造成库存的减少 同时 也会有账户金额的增加
 * 所以购买的动作，可以用多个添加多个观察者 （库存观察者和账户观察者）
 */
class MilkProduct
{
    public $name;
    public $price;
    public $store;
    public $buy = 0 ;
    public $account = 0;

    // 存储观察者
    public  $_observer = array() ;
    public   function  __construct($name,$store,$price)
    {
        $this->name = $name;
        $this->store = $store;
        $this->price = $price;
    }
    // 添加观察者
    public  function  attachObserver($observer){
        $this->_observer[] = $observer;
    }
    // 通知观察者
    public function notify()
    {

        foreach ($this->_observer as $_obsever)
        {
            $_obsever->update($this);
        }

    }

    public function  buy()
    {

        $this->notify($this);

    }
}

class  BuyMilkStoreObserver
{
    public function  update(MilkProduct $product){
        $product->store -= 1;
    }
}

class  BuyMilkMoneyObserver
{
    public function  update(MilkProduct $product){
        $product->account += $product->price;
    }
}

$milk = new MilkProduct("zero",100,10);

$storeObserver = new  BuyMilkStoreObserver();

$moneyObserver = new BuyMilkMoneyObserver();

$milk->attachObserver($storeObserver);
$milk->attachObserver($moneyObserver);

echo $milk -> store . "\n";    // 100
echo $milk -> account . "\n";  // 0

$milk -> buy ();

echo $milk -> store . "\n";    // 99
echo $milk -> account . "\n";  // 10

$milk -> buy ();

echo $milk -> store . "\n";    // 98
echo $milk -> account . "\n";  // 20

```

### 外观模式

> 在应用程序中 下一个步骤需要负载的逻辑步骤和方法调用的时候，可以创建一个外观对象

```php
/**
 * 外观模式
 * 外观模式隐藏了调用对象的复杂度
 * 在应用程序中 下一个步骤需要负载的逻辑步骤和方法调用的时候，可以创建一个外观对象
 */
class Article
{
    public  function  __construct( $title, $descript, $content )
    {
        $this->title = $title;
        $this->descript = $descript;
        $this->content = $content;
    }

    public  function  save(){

        return $this->title . PHP_EOL . $this->descript . PHP_EOL . $this->content;
    }
}

class FormatTitle {

    public  static function makeTitle(Article $article )
    {
        $article->title = strtoupper($article->title);
    }
}

class FormatDescript
{
    public static  function makeDescript( Article $article)
    {
        $article->descript  = substr($article->descript,0,100);
    }
}

class FormatContent
{
    public  static  function makeContent(Article $article)
    {
        $article->content = $article -> content ."\n ". "author by zero";
    }
}


class ArticleFacade
{
    public  static function  create(Article $article){

        FormatTitle::makeTitle($article);
        FormatDescript::makeDescript($article);
        FormatContent::makeContent($article);

        return $article;

    }
}

$article = new Article(" hello word ","php is best language!","blablaalball");

$instance = ArticleFacade::create($article);

var_dump($instance);
```

### 工厂模式

```php
class  RoadBike {

    public  function  run()
    {
        return " Road bike is running!!";
    }
}

class  MountainBike
{
    public  function run()
    {
        return "moutain bike is running!!";
    }
}

class  BikeFactory
{
    public  static  function  build($type){

        switch ($type){
            case "Mountain":
                return new MountainBike();
                break;
            case "Road":
                return new RoadBike();
            default:
                throw new  Exception("error type");
            break;
        }
    }
}

$roadbike = BikeFactory::build("Road");
var_dump($roadbike);

$moutainbike = BikeFactory::build("Mountain");
var_dump($moutainbike);

$nonebike =BikeFactory::build("rand");
```
### 装饰器模式

- 在不修改对象结构的情况下，对原有的对象进行内容或者功能的修改，就应该使用装饰器模式
- 当现有的需求要求不影响现有的基本代码的稳定的前提下，对程序的内容和功能进行很小的该动，最好使用装饰器模式

```php
/**
*  创建一个班级学生类
*  添加班级成员
*  展示班级成员列表
*/
class  Student
{
    public  $member;

    // 初始化班级成员
    public  function  __construct()
    {
        $this->member = array();
    }

    // 添加成员
    public function  addMember($stuent)
    {
        $this->member[] = $stuent;
    }

    // 展示成员
    public function showMember()
    {
        $member = '';

        foreach ($this->member as  $key => $value)
        {
            $member .=  $key . " : " .$value . PHP_EOL;
        }
        return $member;
    }
}

// 成员数据
$member = array( 'zero' , 'one' , "two" );

// 实例化班级成员类
$class = new Student();

// 循环添加成员进对象
foreach ($member as $value)
{
    $class->addMember($value);
}

// 展示成员
echo ( $class->showMember() );


/**
* 班级成员装饰器
* 对每个成员的名称大写开头
*/
class StudentDecorator
{
    public  $student;
    public  function  __construct( Student $student )
    {
        $this-> student = $student;
    }
    public function uppercase()
    {
        foreach ($this->student->member as $key => &$value )
        {
            $value = strtoupper($value);
        }
    }

}

// 创建一个修饰器
$decorator = new StudentDecorator($class);

// 对成员的名称大写
$decorator->uppercase();

// 输出
echo $class->showMember();
```
