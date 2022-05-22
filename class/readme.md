* 什么是类？
类是面向对象编程语言的通用结构。

类是一种结构：将现实世界中存在的关系，采用一种{}的形式，将各种数据和数据的操作捆绑到一起：外界不需要知道里面到底是怎么实现的，只需要调用里面提供的可操作的方法（封装）。

比原型方式要简洁的多，结构层面更加清晰。
* 语法操作：
1. 类class是ES6的语法
2. 类的语法：class 类名 {}
3. 类名不能重复
实例化
4. 类不会自动运行：需要new的时候才会触发
5. 只要对象产生：new出一个对象：里面的构造器就会被触发
方法实现
6. 类里面的方法：方法名 + () + {}，不需要function关键字（代表当前方法挂载Student.prototype）
7. 类实例化得到的对象，可以直接调用方法
————————————————
版权声明：本文为CSDN博主「维多利亚_」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/l13640698113/article/details/109658286
```
         class Student {
           // 构造器：当用户new Student对象的时候：new出来的对象会自动调用的方法
            constructor(stuNo, stuName, stuGender, stuAge) {
                // 创建属性：this永远代表对象
                this.stuNo = stuNo;
                this.stuName = stuName;
                this.stuGender = stuGender;
                this.stuAge = stuAge;
            }
            // 其他方法：方法名 + () + {}
            say() {
                console.log(this.stuName + '很美');
            }
        }

 // 构造器被调用了：只要有new对应的类就会被触发，产生一个对象 -- 实例化
 let s1 = new Student('001', '希希子', '女', 'secret'); 
 //调用方法
 s1.say();

```
* 类的继承extends
子类继承父类：extends关键字
继承效果：子类对象可以访问父类中提供的属性或者方法
```
        class Mother {
            constructor(name, age) {
                this.name = name;
                this.age = age;
                console.log('我是父类的构造器');
            }

            say() {
                console.log(this.name + this.age);
            }
        }

        // 继承
        class Daughter extends Mother {

        }

        // 实例化子类对象
        let d1 = new Daughter('娜娜子', 16);    // 调用类构造器（父类的：继承得来的）
        d1.say();


```
* 子类拥有父类的同名属性或者方法：叫做重写override

继承的：属性、方法访问自己有，用自己的，自己没有的找父类
```
lass Father {
            name = 'father';
            age = 50;

            say() {
                console.log(this.name + this.age);
            }
        }

        // 只有同名属性
        class Son1 extends Father {
            name = 'son1';
        }

        let s1 = new Son1();
        console.log(s1.name); // son1

```
* 子类的属性如果与父类重名：会替换掉。
子类的方法如果与父类重名：在使用的时候只能访问到自己，如果非要访问父类的，必须使用super()。

使用方式1：普通方法的覆盖，super.父类被覆盖的方法()
```
class Father {
            name = 'father';

            say() {
                console.log(this.name);
            }
        }

        class Son extends Father {
            name = 'son';       // 属性的覆盖：替换，内存中只有一个name属性：值为son

            // 方法重写
            say() {
                // 需要在子类的方法中使用super.被子类覆盖的方法say()
                super.say();                // 调用父类方法（方法存在于原型：Son和Father的原型不同，所以不可能覆盖）
            }
        }

        // 实例化：子类
        let s = new Son();
        s.say();//son

```
使用方式2：构造方法的覆盖，构造方法的名字一定是constructor：直接super()：如果子类有构造方法，父类也有构造方法：子类必须在构造方法中，先调用父类的构造方法，然后才能使用this
        // 父类
        class Person {
            constructor(name, age) {
                this.name = name;
                this.age = age;
                console.log('我是父类的构造方法');
            }
        }

        class Student extends Person {
            // 重写父类的构造方法
            constructor(stuNo) {
                // 父类有构造方法：那么子类的构造方法必须先访问父类的构造方法
                super('张三', 10);          // 正常开发：super()放最前面
                console.log('我是子类的构造方法');

                // 只要再构造方法使用this之前调用父类的构造方法就可以运行代码
                // super('张三', 10);
                this.stuNo = stuNo;

                // 构造方法的this之后，再调用父类的构造方法是不行的
                // super('张三', 10);
            }
        }

        let stu = new Student('001');
        console.log(stu);
