---
title: 详解TypeScript的基础类型
date: 2022-04-07 09:41:46
tags: ["typescript"]
categories: ["TypeScript"]
---

> 这篇文章主要为大家介绍了TypeScript的基础类型，具有一定的参考价值，感兴趣的小伙伴们可以参考一下，希望能够给你带来帮助

## 布尔类型
```typescript
// 布尔类型--->boolean
// let 变量名：数据类型 = 值
let flag: boolean = true;
console.log(flag)
```
## 数字类型
```typescript
// 数字类型--->number
let a1: number = 10 // 十进制
let a2: number = 0b1010 // 二进制
let a3: number = 0o12// 八进制
let a4: number = 0xa // 十六进制
console.log(a1 + a2 + a3 + a4)
```
## 字符串类型
```typescript
// 字符串类型--->string
let str1: string = '床前明月光';
let str2: string = '地上鞋两双';
console.log(str1 + ',' + str2)
```
## 字符串和数字进行拼接
```typescript
let str3: string = '我现在的岁数：'
let a5: number = 24
console.log(`${str3}${a5}`)
```
总结:ts中变量一开始是什么类型,那么后期赋值的时候,只能用这个类型的数据,是不允许用其他类型的数据赋值给当前的这个变量中
## undefined和 null
```typescript
// undefined和 null都可以作为其他类型的子类璧,把undefined和nu1l赋值给其他类型的变量的,如: number类型的变量
let und: undefined = undefined
let n1l: null = null
console.log(und)
console.log(n1l)
```
## 数组类型
```typescript
// 方式一：let变量名:数据类型[]=[值1,值2,值3,...]
let arr1: number[] = [10, 20, 30, 40, 50]
console.log(arr1);

// 方式二：泛型的写法
// 语法: let变量名: Array<数据类型>=[值1,值2,值3]
let arr2: Array<number> = [100, 200, 300]
console.log(arr2);
```
注意问题:数组定义后,里面的数据的类型必须和定义数组的时候的类型是一致的,否则有错误提示信息,也不会编译通过的
## 元组类型
```typescript
// 元组类型:在定义数组的时候,类型和数据的个数一开始就已经限定了
let arr3: [string, number, boolean] = ['小甜甜', 100, true];
console.log(arr3)
// 注意问题:元组类型在使用的时候,数据的类型的位置和数据的个数应该和在定义元组的时候的数据类型及位置应该是一致的
console.log(arr3[0].split(''));
console.log(arr3[1].toFixed(2));
```
## 枚举类型
```typescript
enum Color {
       red,
       green,
       blue
}
// 定义一个Color的枚举类型的变量来接收枚举的值
let color: Color = Color.red
console.log(color);
console.log(Color[2])
```
## any类型
```typescript
let str5: any = 100;
str5 = '宇智波带土'
console.log(str5);
// 当一个数组中要存储多个数据,个数不确定,类型不确定,此时也可以使用any类型来定义数组
let arr6: any = [100, '宇智波带土', true];
console.log(arr6)
// 这种情况下也没有错误的提示信息, any类型有优点,也有缺点
console.log(arr6[1].split(''));
```
## void类型
```typescript
function getobj(obj: object): object {
       console.log(obj);
       return {
           name: '卡卡西',
           age: 27
       }
}
console.log(getobj({ name: '佐助', age: 20 }))
```
## 联合类型
```typescript
// 需求1:定义一个函数得到一个数字或字符串值的字符串形式值
function getString(str: number | string): string {
      return str.toString();
}
console.log(getString('萨斯给'))
  
// 需求2:定义一个一个函数得到一个数字或字符串值的长度
function getString1(str: number | string): number {
      return str.toString().length
      if ((<string>str).length) {
          return (str as string).length
      } else {
          return str.toString().length
      }
}
console.log(getString1(12345))
console.log(getString1('12345'))
```
## 总结
本篇文章就到这里了，希望能够给你带来帮助，也希望您能够多多关注68HTML的更多内容! 

