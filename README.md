# magic-methods-php

## __contruct() & __destruct()
- __construct: Hàm này được gọi khi khởi tạo đối tượng 
- __desctruct: Hàm này được gọi khi hủy đối tượng
```php
Class People {
    public function __construct() {
        echo '__construct';
    }
    
    public function getName(){
        return "thanh";
    }
    
    public function __destruct(){
        echo "__destruct";
    }
}

$thanh = new People();
echo $thanh->getName();
// __construct
// thanh
// __destruct
```
## __set() và __get() 
- Với các thuộc tính private, protected, để truy suất / thay đổi ta sử dụng __get() / __set()
```php
Class People {
    private $name;
    private $age;

    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }

    public function __get($property) {
        return $this->$property;
    }

    public function __set($property, $value) {
        $this->$property = $value;
    }
}

$people = new People("thanh", 20);
$people->age = 20;

echo $people->name; // thanh
echo $people->age; //20
```
## __isset() và __unset() 
- __isset() sẽ được gọi khi chúng ta thực hiện kiểm tra một thuộc tính không được phép truy cập của một đối tượng, hay kiểm tra một thuộc tính không tồn tại trong đối tượng đó.
- __unset() sẽ được gọi khi chúng ta thực hiện hủy một thuộc tính không được phép truy cập của một đối tượng, hay hủy một thuộc tính không tồn tại trong đối tượng đó. 
```php
Class People {
    private $name;
    private $age;

    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }

    public function __isset($property) {
        echo "action isset";
    }

    public function __unset($property) {
        echo "action unset";
    }
}

$people = new People("thanh", 20);
isset($people->age);
// action isset

unset($people->ok);
// action unset
```
## _call() và __callStatic()
- __call() sẽ được gọi khi chúng ta gọi một phương thức không được phép truy cập từ bên ngoài hoặc không tồn tại.
- __callStatic() sẽ được gọi khi chúng ta gọi một phương thức <b>tĩnh</b> không được phép truy cập từ bên ngoài hoặc không tồn tại của .
```php
Class People {
    private $name;
    private $age;

    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }

    protected function getName() {
        return $this->name;
    }

    protected function __call($methodName, $arguments) {
        echo "call function here";
    }

}

$people = new People("thanh", 20);

echo $people->getName();
```
## __sleep() và __wakeup()
- Phương thức __sleep() sẽ được gọi khi chúng ta thực hiện serialize() object. 
- Phương thức __wakeup() sẽ được gọi khi chúng ta unserialize() object.
```php
class People
{
    private $name = 'thanh';
    private $age = 20;

    public function __sleep()
    {
        //trả về thuộc tính name
        return array('name');
    }
}

echo serialize(new People());
```
## __toString() và __invoke()
- __toString() sẽ được gọi khi chúng ta dùng đối tượng như một string.
- __invoke() sẽ được gọi khi chúng ta sử đối tượng như một hàm.
```php
class People
{
    private $name = 'thanh';
    private $age = 20;

    public function getName()
    {
        return $this->name;
    }
    
    public function __toString()
    {
        return 'Phương thức __toString() được gọi';
    }

    public function __invoke()
    {
        echo 'Phương thức __invoke() được gọi';
    }
}

$people =  new People;

echo $people;
//Phương thức __toString() được gọi
$people();
// Phương thức __invoke() được gọi
```
