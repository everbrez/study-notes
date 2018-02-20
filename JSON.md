# JSON（JavaScript Object Notation，JavaScript对象表示法），利用JavaScript中的一些模式来表示结构化数据。(JSON不支持变量、函数、对象实例，它就是一种结构化数据的格式)
## JSON的语法可以表示为一下三种类型：
- 简单值：使用JavaScript相同的语法，可以在JSON中表示字符串、数值、布尔值和null，但是不支持JavaScript中的undefined。
- 对象：表示一组无序的键值对。
- 数组：表示一组有序的值的列表。
### 简单值
如：
- ```5```
- ```“Hello World”```
### 对象
如：
```
{
    "name": "Nicholas",
    "age": 29,
    "school": {
        "name": "sun yat-son University",
        "location": "sourth of china"
    }
}
```
- 与JavaScript对象字面量相比，没有声明变量和分号（:）
- 对象的属性必须加双引号（单引号也不可以），这个是必需的
- 属性值可以是简单值，也可以是复杂的类型值
### 数组
如：
```
[23,true,null,{
    "name": "Nicholas",
    "age": 29,
    "school": {
        "name": "sun yat-son University",
        "location": "sourth of china"
    }
}
]
```
## 解析与序列化
### JSON对象
早期使用JavaScript的```eval()```函数来解析JSON，ECMAScript5定义了全局对象JSON。
JSON对象有两种方法：
- ```stringify()```将JavaScript对象序列化成JSON字符串
- ```parse()```将JSON字符串解析为原生JavaScript值
如：
```
const book = {
    title: "JavaScript",
    author: ["Nicholas","zakas"],
    year: 2018
};
const jsonText = JSON.stringify(book);
```
一般输出内容不包括空格和缩进：
```
{"title":"JavaScript","author":["Nicholas","zakas"],"year":2018}
```
在序列化JavaScript对象时，一般函数及原型对象都会被忽略，值为undefined的任何属性也会被忽略
#### 使用parse()函数可以得到相应的JavaScript值
如：
```
const bookCopy = JSON.parse(jsonText);
```
输出与book没有关系的对象
##  过滤及缩进
### ```JSON.stringify()```还接受另外两个参数，第一个参数是过滤器（可以为数组或者函数），第二个参数是一个选项（用于表示JSON字符串中保留缩进）
- 如果过滤器是数组，那么```JSON.stringify()```的结果中只包含数组中列出的属性
如:
```
const filter = [
    ["title","year"],
    function(key,value){
        switch(key){
            case "year":
                return 5000;
            case "edition":
                return undefined;
            case "author":
                return value.join(",");
            default:
                return value;
        }
    }
];
const jsonText1 = JSON.stringify(book,filter[0]); 
console.log(jsonText1);
const jsonText2 = JSON.stringify(book,filter[1]); 
console.log(jsonText2);
```
输出：
```
{"title":"JavaScript","year":2018}
{"title":"JavaScript","author":"Nicholas,zakas","year":5000}
```
缩进：
上文中的改成
```
const jsonText1 = JSON.stringify(book,filter[0],4); 
console.log(jsonText1);
```
输出
```
{
app.js:24
    "title": "JavaScript",
    "year": 2018
}
```
**最大的缩进为10，任何大于10的数字都会自动转换为10**
Date对象有一个toJSON()方法，除此之外可以为任何对象添加toJSON()方法
#### JSON.stringify()序列化顺序：
- 如果存在toJSON方法，并且可以取得有效值，则调用该方法，否则返回对象本身
- 如果提供了第二参数，则应用这个过滤器
- 对上步的值进行序列化
- 如果提供了第三参数，执行相应的格式化
## 解析选项
JSON.parse()方法接受另一个参数（函数），成为还原函数
```
const book = {
    year: new Date()
}
const jsonText = JSON.stringify(book);
console.log(jsonText);
console.log(JSON.parse(jsonText,function(key,value){
    if(key === "year"){
        return new Date(value);
    } else{
        return value;
    }
}));
```
