---
title: JS常用函数
categories: 'js'
tags: 
    - "函数"
    - "基础"
---



### split

用于把一个字符串分割成字符串数据

```JavaScript
Object.split(',')
```

### Array.form

获取数组中某一项值

```JavaScript
const list = [{id:1,user:'员工1'},{id:2,user:'员工2'}]
conlose.log(Array.from(list,i=>i.user))

//
['员工1','员工2']
```

### filter

过滤

从对象中获取匹配数组的值

```JavaScript
const words = [{id: 1, name: 'a'}, {id: 2, name: 'b'}, {id: 3, name: 'c'}];
const arr = [1,3]
const result = words.filter(word => arr.indexOf(word.id) > -1);

console.log(result.map(i => i.name));

// Array ["a", "c"]
```

### 获取两个数组的差值

```JavaScript
let list1 = [1,2,3]
let list2 = [2,3,6]
const differList = differenceWith([...list1, ...list2], intersection(list1, list2));

console.log(differList);
// Array [1,6]
```

### 获取文件夹下的所有文件，并加载

当前index.js 同级加入modules文件夹。并自动加载

```JavaScript
const modules = {};
const files = require.context('./modules', false, /\.js$/);
files.keys().forEach(key => {
  const fileName = key.replace(/(\.\/|\.js)/g, '');
  const keyName = fileName.replace(/-(\w)/g, (all, letter) => letter.toUpperCase());
  modules[keyName] = files(key).default;
});
export default modules;
```

### 判断UTF-8字符数

```JavaScript
function lengthUTF8(str) {
  var i = 0, code, len = 0;
  for (; i < str.length; i++) {
    code = str.charCodeAt(i);
      if(code == 10){//回车换行问题
      len += 2;
    }else if (code < 0x007f) {
      len += 1;
    } else if (code >= 0x0080 && code <= 0x07ff) {
      len += 2;
    } else if (code >= 0x0800 && code <= 0xffff) {
      len += 3;
    }
  }
  return len;
}
```

