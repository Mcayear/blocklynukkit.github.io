# 与nk和java互调  

## 前言  

正如您所知，Nukkit是由java写成，运行在java上的，与nk和java互调就显得尤为必要了。  
BlocklyNukkit与java无缝集成，您可以通过`javascript`、`python`、`lua`和`php`来实现与nk和java的无缝调用，调用的性能损耗几乎为零。  
这一点也是BlocklyNukkit与其他服务端的解释型语言加载器相比最突出的优点，这意味着，您不光可以享受使用BlocklyNukkit提供的上千个便捷开发的API，nukkit中上万个API、java中数十万个API都可以任您尽情调用，您可以通过BlocklyNukkit实现您希望的一切。
这包括但不限于编写所有类型的服务器插件或mod、制作服务器面板，检测硬件信息(甚至是在虚拟机/vps/面板服中检测宿主机信息)、甚至是加载其他的编程语言、写一个新的mc服务端都是可以的。  

## 基础概念

### 类与对象  

java是一门面向对象语言，nk也是用java编写而成。java中，所有的函数都在类的里面，类是对象的模板，你可以通过类来制造对象。  
用更通俗的话来解释：  

- 对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。  
- 类是一个模板，它描述一类对象的行为和状态。
- P.S.对象不是异性朋友

下图中男孩(boy)、女孩(girl)为类，而具体的每个人为该类的对象：

![](https://www.runoob.com/wp-content/uploads/2013/12/object-class.jpg)

下图中汽车为类，而具体的每辆车为该汽车类的对象，对象包含了汽车的颜色、品牌、名称等。

![](https://www.runoob.com/wp-content/uploads/2013/12/class-object2020-10-27.png)

而具体到服务器插件开发，这里列举一些例子：

|类|对象|
|-|-|
|玩家类|穿着铁甲在挖矿的一个史蒂夫|
|物品类|一组钻石|
|附魔类|附魔在钻石剑上面的一个锋利II附魔|
|实体类|一只正在燃烧的僵尸|
|方块类|一个放在地上的草方块|
|方块实体类|一个大箱子对应的数据|
|世界类|你服务器的主城地图(希望你服有主城)|
|物品栏类|你的背包|
|事件类|小明一次进服产生的玩家进服事件|
|......|......|


### 成员函数

成员函数在java中还有一个名字，叫做方法。  
成员函数，顾名思义，就是一个对象所拥有的函数。这些函数在执行时，多数情况下，作用的对象不仅是你传入的参数，还有拥有这个成员函数的对象。  
成员函数在类里面被定义，定义时分为两种：静态成员函数(静态方法)和非静态成员函数(对象方法)。静态方法是执行时和普通函数一样，
仅仅只对你传入的参数进行作用，而对象方法就是上文提到的多数情况。  
为什么有了普通函数还要有静态方法呢？因为java不允许普通函数，所以用静态方法替代普通函数。  

### 成员变量  

成员变量在java中也有另一个名字，叫字段。
成员变量，顾名思义，就是一个对象所拥有的变量，多数情况下，不同的对象可以拥有相同变量名的成员变量，这些相同变量名的成员变量值可以不同。  
成员变量也是在类里面被定义的，定义时也分为两种：静态成员变量(静态字段)和非静态成员变量(对象字段)。静态成员变量跟普通的变量一样，所有同一个类产生的对象，静态字段的值都是相同的
但是不同的类，可以拥有相同名称但是变量值不同的静态字段。  

### 实例化与构造函数  

实例化，就是通过类创建对象的过程，这个过程我们通过构造函数进行。  
每个类都有一个或者多个构造函数，构造函数负责将通过类来创建一个新对象，请注意，构造函数是一种静态成员函数。 

### 包名与类名  

类名，顾名思义，就是一个类的名字。  
包名，是为了区分不同的类而在类前面添加的前缀，因为有些类的名字是相同的(虽然这并不是推荐的做法)。
一个类通过它的构造函数产生的对象拥有相同的类名和包名。  
类的全类名，又称完整类名，就是`类的包名`+`.`+`类的类名`。
比如nukkit中玩家类的类名是`Player`，包名是`cn.nukkit`，那么这个玩家类的完整类名就是`cn.nukkit.Player`

## 在bn插件中调用java  

### 调用对象方法

听起来很高大上，其实你已经不知不觉中通过了！  
还记得基对象不？基对象就是一个java中的对象！而你调用基对象中的函数就是在bn插件中调用java函数!  
没错，bn插件的开发就是在与java的互调用中进行的。而调用java类产生的java对象的对象方法非常简单，
只需要通过在对象后面加上`.`(js&python)、`:`(lua)、`->`(php)，在加上对象的非静态成员函数的函数名，后面跟着括号传入参数就行了！  
{% capture method %} 
```javascript
var 返回值 = java对象.对象方法函数名(参数列表);
```  
---NEWTAB--- 
```python
返回值 = java对象.对象方法函数名(参数列表)
```
---NEWTAB--- 
```lua
返回值 = java对象:对象方法函数名(参数列表)
```
---NEWTAB--- 
```php
$返回值 = $java对象->对象方法函数名(参数列表);
```
{% endcapture %}
{% include tab.html tabId="method" tabTitles="JavaScript,Python,Lua,PHP" tabContents=method %}
### 调用静态方法  

调用静态方法需要直接从类调用而不是从对象调用。需要先使用每个语言中对应的方式导入java类，然后才能调用。  
{% capture staticmethod %} 
javascript中可以使用`Java.type`来根据全类名获取类，而后可以通过`.`来调用静态方法。  
```javascript
var xxx = Java.type("java类的全类名");
var 返回值 = xxx.静态方法函数名(参数列表);
```  
也可以通过`require`来获取，效果跟上面是一样的(只能在1.2.9.2及更高版本使用)  
```javascript
var xxx = require("java类的全类名");
var 返回值 = xxx.静态方法函数名(参数列表);
```  
---NEWTAB--- 
python中直接通过import语句就可以导入，导入后使用类名加上`.`来调用静态方法。  
```python
import java类的全类名
返回值 = java类的类名.静态方法函数名(参数列表)
```
也可以使用`as`来指定一个别名。  
```python
import java类的全类名 as xxx
返回值 = xxx.静态方法函数名(参数列表)
```
---NEWTAB--- 
lua中可以使用`luajava.bindClass`来根据全类名获取类，而后可以通过`:`来调用静态方法。 
```lua
xxx = luajava.bindClass("java类的全类名")
返回值 = xxx:静态方法函数名(参数列表)
```
---NEWTAB--- 
php中可以使用`java_class`来根据全类名获取类，而后可以通过`->`来调用静态方法。 
```php
$xxx = java_class("java类全类名");
$返回值 = $xxx->静态方法函数名(参数列表);
```
也可以直接使用`import`语句，而后通过全类名加`::`来调用静态方法。  
```php
import java类全类名;
$返回值 = java类的类名::静态方法函数名(参数列表);
```
{% endcapture %}
{% include tab.html tabId="staticmethod" tabTitles="JavaScript,Python,Lua,PHP" tabContents=staticmethod %}

### 调用对象字段  

调用对象字段，即获取对象的非静态成员变量的值，调用的方式跟调用对象方法类似。  
{% capture field %} 
```javascript
var xxx = java对象.对象字段名;
```  
---NEWTAB--- 
```python
xxx = java对象.对象字段名
```
---NEWTAB--- 
```lua
xxx = java对象.对象字段名
```
---NEWTAB--- 
```php
$xxx = $java对象->对象字段名;
```
{% endcapture %}
{% include tab.html tabId="field" tabTitles="JavaScript,Python,Lua,PHP" tabContents=field %}

### 调用静态字段  

调用静态字段，即获取对象的静态成员变量的值，调用的方式跟调用静态方法类似。  
{% capture staticfield %} 
javascript中可以使用`Java.type`来根据全类名获取类，而后可以通过`.`来调用静态字段。  
```javascript
var xxx = Java.type("java类的全类名");
var 值 = xxx.静态字段名;
```  
也可以通过`require`来获取，效果跟上面是一样的(只能在1.2.9.2及更高版本使用)  
```javascript
var xxx = require("java类的全类名");
var 值 = xxx.静态字段名;
```  
---NEWTAB--- 
python中直接通过import语句就可以导入，导入后使用类名加上`.`来调用静态方法。  
```python
import java类的全类名
值 = java类的类名.静态字段名
```
也可以使用`as`来指定一个别名。  
```python
import java类的全类名 as xxx
值 = xxx.静态字段名
```
---NEWTAB--- 
lua中可以使用`luajava.bindClass`来根据全类名获取类，而后可以通过`:`来调用静态方法。 
```lua
xxx = luajava.bindClass("java类的全类名")
值 = xxx:静态字段名
```
---NEWTAB--- 
php中可以使用`java_class`来根据全类名获取类，而后可以通过`->`来调用静态方法。 
```php
$xxx = java_class("java类全类名");
$值 = $xxx->静态字段名;
```
也可以直接使用`import`语句，而后通过全类名加`::`来调用静态方法。  
```php
import java类全类名;
$值 = java类的类名::静态字段名;
```
{% endcapture %}
{% include tab.html tabId="staticfield" tabTitles="JavaScript,Python,Lua,PHP" tabContents=staticfield %}