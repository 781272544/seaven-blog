---
title: 如何利用Typescript封装本地存储
date: 2022-04-07 09:48:12
tags: ["typescript"]
categories: ["TypeScript"]
---

## 前言
> 本地存储是前端开发过程中经常会用到的技术，但是官方api在使用上多有不便，且有些功能并没有提供给我们相应的api，比如设置过期时间等。本文无意于介绍关于本地存储概念相关的知识，旨在使用typescript封装一个好用的本地存储类。

## 本地存储使用场景
+ 用户登录后token的存储
+ 用户信息的存储
+ 不同页面之间的通信
+ 项目状态管理的持久化，如redux的持久化、vuex的持久化等
+ 性能优化等
+ ...
## 使用中存在的问题
+ 官方api不是很友好（过于冗长），且都是以字符串的形式存储，存取都要进行数据类型转换
    + localStorage.setItem(key, value)
    + ...
+ 无法设置过期时间
+ 以明文的形式存储，一些相对隐私的信息用户都能很轻松的在浏览器中查看到
+ 同源项目共享本地存储空间，可能会引起数据错乱
## 解决方案
将上述问题的解决方法封装在一个类中，通过简单接口的形式暴露给用户直接调用。 类中将会封装以下功能：

+ 数据类型的转换
+ 过期时间
+ 数据加密
+ 统一的命名规范

### 功能实现
```typescript
// storage.ts
 
enum StorageType {
  l = 'localStorage',
  s = 'sessionStorage'
}
 
class MyStorage {
  storage: Storage
 
  constructor(type: StorageType) {
    this.storage = type === StorageType.l ? window.localStorage : window.sessionStorage
  }
 
  set(
    key: string,
    value: any
  ) {
    const data = JSON.stringify(value)
    this.storage.setItem(key, data)
  }
 
  get(key: string) {
    const value = this.storage.getItem(key)
    if (value) {
      return JSON.parse(value)
  }
 
  delete(key: string) {
    this.storage.removeItem(key)
  }
 
  clear() {
    this.storage.clear()
  }
}
 
const LStorage = new MyStorage(StorageType.l)
const SStorage = new MyStorage(StorageType.s)
 
export { LStorage, SStorage }
```
以上代码简单的实现了本地存储的基本功能，内部完成了存取时的数据类型转换操作，使用方式如下：
```typescript
import { LStorage, SStorage } from './storage'
 
...
 
LStorage.set('data', { name: 'zhangsan' })
LStorage.get('data') // { name: 'zhangsan' }
```
### 加入过期时间
设置过期时间的思路为：在set的时候在数据中加入expires的字段，记录数据存储的时间，get的时候将取出的expires与当前时间进行比较，如果当前时间大于expires，则表示已经过期，此时清除该数据记录，并返回null，expires类型可以是boolean类型和number类型，默认为false，即不设置过期时间，当用户设置为true时，默认过期时间为1年，当用户设置为具体的数值时，则过期时间为用户设置的数值，代码实现如下：
```typescript
interface IStoredItem {
  value: any
  expires?: number
}
...
set(
    key: string,
    value: any,
    expires: boolean | number = false,
  ) {
    const source: IStoredItem = { value: null }
    if (expires) {
    // 默认设置过期时间为1年，这个可以根据实际情况进行调整
      source.expires =
        new Date().getTime() +
        (expires === true ? 1000 * 60 * 60 * 24 * 365 : expires)
    }
    source.value = value
    const data = JSON.stringify(source)
    this.storage.setItem(key, data)
  }
   
  get(key: string) {
    const value = this.storage.getItem(key)
    if (value) {
      const source: IStoredItem = JSON.parse(value)
      const expires = source.expires
      const now = new Date().getTime()
      if (expires && now > expires) {
        this.delete(key)
        return null
      }
 
      return source.value
    }
  }
```
### 加入数据加密
加密用到了crypto-js包，在类中封装encrypt,decrypt两个私有方法来处理数据的加密和解密，当然，用户也可以通过encryption字段设置是否对数据进行加密，默认为true，即默认是有加密的。另外可通过process.env.NODE_ENV获取当前的环境，如果是开发环境则不予加密，以方便开发调试，代码实现如下：
```typescript
import CryptoJS from 'crypto-js'
 
const SECRET_KEY = 'nkldsx@#45#VDss9'
const IS_DEV = process.env.NODE_ENV === 'development'
...
class MyStorage {
  ...
   
  private encrypt(data: string) {
    return CryptoJS.AES.encrypt(data, SECRET_KEY).toString()
  }
 
  private decrypt(data: string) {
    const bytes = CryptoJS.AES.decrypt(data, SECRET_KEY)
    return bytes.toString(CryptoJS.enc.Utf8)
  }
   
  set(
    key: string,
    value: any,
    expires: boolean | number = false,
    encryption = true
  ) {
    const source: IStoredItem = { value: null }
    if (expires) {
      source.expires =
        new Date().getTime() +
        (expires === true ? 1000 * 60 * 60 * 24 * 365 : expires)
    }
    source.value = value
    const data = JSON.stringify(source)
    this.storage.setItem(key, IS_DEV ? data : encryption ? this.encrypt(data) : data
    )
  }
   
  get(key: string, encryption = true) {
    const value = this.storage.getItem(key)
    if (value) {
      const source: IStoredItem = JSON.parse(value)
      const expires = source.expires
      const now = new Date().getTime()
      if (expires && now > expires) {
        this.delete(key)
        return null
      }
 
      return IS_DEV
        ? source.value
        : encryption
        ? this.decrypt(source.value)
        : source.value
    }
  }
   
}
```
### 加入命名规范
可以通过在key前面加上一个前缀来规范命名，如项目名_版本号_key类型的合成key，这个命名规范可自由设定，可以通过一个常量设置，也可以通过获取package.json中的name和version进行拼接，代码实现如下：
```typescript
const config = require('../../package.json')
 
const PREFIX = config.name + '_' + config.version + '_'
 
...
class MyStorage {
 
  // 合成key
  private synthesisKey(key: string) {
    return PREFIX + key
  }
   
  ...
   
 set(
    key: string,
    value: any,
    expires: boolean | number = false,
    encryption = true
  ) {
    ...
    this.storage.setItem(
      this.synthesisKey(key),
      IS_DEV ? data : encryption ? this.encrypt(data) : data
    )
  }
   
  get(key: string, encryption = true) {
    const value = this.storage.getItem(this.synthesisKey(key))
    ...
  }
 
}
```
### 完整代码
```typescript
import CryptoJS from 'crypto-js'
const config = require('../../package.json')
 
enum StorageType {
  l = 'localStorage',
  s = 'sessionStorage'
}
 
interface IStoredItem {
  value: any
  expires?: number
}
 
const SECRET_KEY = 'nkldsx@#45#VDss9'
const PREFIX = config.name + '_' + config.version + '_'
const IS_DEV = process.env.NODE_ENV === 'development'
 
class MyStorage {
  storage: Storage
 
  constructor(type: StorageType) {
    this.storage =
      type === StorageType.l ? window.localStorage : window.sessionStorage
  }
 
  private encrypt(data: string) {
    return CryptoJS.AES.encrypt(data, SECRET_KEY).toString()
  }
 
  private decrypt(data: string) {
    const bytes = CryptoJS.AES.decrypt(data, SECRET_KEY)
    return bytes.toString(CryptoJS.enc.Utf8)
  }
 
  private synthesisKey(key: string) {
    return PREFIX + key
  }
 
  set(
    key: string,
    value: any,
    expires: boolean | number = false,
    encryption = true
  ) {
    const source: IStoredItem = { value: null }
    if (expires) {
      source.expires =
        new Date().getTime() +
        (expires === true ? 1000 * 60 * 60 * 24 * 365 : expires)
    }
    source.value = value
    const data = JSON.stringify(source)
    this.storage.setItem(
      this.synthesisKey(key),
      IS_DEV ? data : encryption ? this.encrypt(data) : data
    )
  }
 
  get(key: string, encryption = true) {
    const value = this.storage.getItem(this.synthesisKey(key))
    if (value) {
      const source: IStoredItem = JSON.parse(value)
      const expires = source.expires
      const now = new Date().getTime()
      if (expires && now > expires) {
        this.delete(key)
        return null
      }
 
      return IS_DEV
        ? source.value
        : encryption
        ? this.decrypt(source.value)
        : source.value
    }
  }
 
  delete(key: string) {
    this.storage.removeItem(this.synthesisKey(key))
  }
 
  clear() {
    this.storage.clear()
  }
}
 
const LStorage = new MyStorage(StorageType.l)
const SStorage = new MyStorage(StorageType.s)
 
export { LStorage, SStorage }
```
## 总结
到此这篇关于如何利用Typescript封装本地存储的文章就介绍到这了,更多相关Typescript封装本地存储内容请搜索68HTML以前的文章或继续浏览下面的相关文章希望大家以后多多支持68HTML！



