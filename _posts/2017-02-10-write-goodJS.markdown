---
layout: post
title:  "编写可维护的JS笔记（1）"
subtitle: "\"上班闲暇之余卡看的书，至今没有看完，已经还给之光\""
date: 2017-02-10 14:31:00
author: "marillo"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - JavaScript 
---



> 可用的sublime插件： JSLINT JSHINT



# 正文

- #### 缩紧层级

  ###### 一个tab或者2个空格，但要一致

- #### 语句结尾

  ###### 不要省略分号

- #### 行的长度

  ###### 一般一行的长度限定在80个字符，免得出现滚动条

- #### 换行

  ###### 当一行长度达到了单行最大限度，需手动换行。通常在运算符后换行，下一行增加2个层级的缩进

  **eg:  if的主体仍然是一个缩进，只有换行是2个缩进**

  ```javascript
  if (isLeapYear && isFebruary && day == 29 && itsYourBirthday &&
          noPlans) {
    waitAnotherFourYears();
  }
  ```

  ****

  ###### 当给变量赋值时，第二行的位置应当和赋值运算符的位置保持对齐		

  ```javascript
  var result = something + antherThing + yetAnotherThing + someThingElse +
               anotherSomethingElse;
  ```



- #### 空行

  ###### 在一下场景中适当的添加空行：（空行不是必须的）

  1. 在方法之间；
  2. 在方法中的局部变量和第一条语句之间
  3. 在多行或单行注释之前
  4. 在方法内逻辑片段之间插入空行，提高可读性。

- #### 命名

  ###### 一般采用小驼峰命名。首字母小写，之后的每个单词首字母大写。

- #### 变量和函数

  ###### 变量名采用小驼峰命名法，命名前缀应该是名词；

  ###### 函数名前缀为动词;

  ```javascript
  var myName = 'marillo';
  function getName() {
    return myName;
  }
  ```

  **避免使用没有意义的命名，如foo,bar,tmp等**
    

- ####  常量

  ###### 采用大写字母和下划线来命名，下划线用以分割单词

  ```javascript
  var MAX_COUNT = 10;
  var URL = 'http://marillo.me';
  ```

- #### 构造函数

  ###### 命名采用大驼峰命名法，命名常常是名词

  ```javascript
  function Person(name) {
    this.name = name;
  }
  Person.prototype.sayName = function() {
    alert(this.name);
  };
  var me = new Person('marillo');
  ```

- #### 字符串

  ###### 字符串可用 *单引号／双引号* 扩起来；

  ###### 但字符串内含有外层一样的引号的时候需要转义

  ```javascript
  var name = 'Marillo says, \'hi\'';
  var name = 'Marillo says, "hi"';
  ```

  ###### 如果字符串是多行的，需要用 + 连接

  ```javascript
  var longString = "I took a pill in Ibiza" +
  			   "To show her I was cool";
  ```

- #### 数字

  ###### 如果是浮点数，则不能省略小数部分和整数部分。

  ###### 不推荐写八进制，已被弃用。