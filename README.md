# vue-ruler
> 基于vue 2.x的 刻度尺，只支持移动端
## 实例代码：
```
    体重
    <DLRuler :value="50" :min="30" :max="300" :onChange="changeWeight"></DLRuler>
    身高
    <DLRuler :value="60" :min="10" :max="100"></DLRuler>
    测试
    <DLRuler :value="35" :min="10" :max="120"></DLRuler>
```    
参数描述：

value 当前值

min 最小值

max 最大值

onChange：数值发生改变后调用， 可选参数

## 效果：
![image](https://github.com/alvin198761/vue-ruler/blob/master/ruler.gif)
## 问题：
最小刻度还有bug,

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run start
```

### Compiles and minifies for production
```
npm run build
```
