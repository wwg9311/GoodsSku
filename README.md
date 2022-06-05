# 商品规格创建组件

## 项目介绍

该组件是基于Vue开发的，涉及到工作中一部分的功能，目前基本上满足了电商项目创建商品规格的功能，包含了商品规格的新增、修改和删除和商品表格的页面渲染，目前设置了可新增的规格和规格值为3

## 项目难点剖析

做这个项目的时候，前期做的时候还是比较顺利的，比较有挑战性的就是表格数据的生成，即根据设置的商品规格来生成表格内容，做这块儿的时候，也请教了一些朋友，也去查了相关的算法，无奈自己的算法底子薄弱，对于大佬的算法无从下手，后来就自己去想解决方案。

这块儿主要的难点就是数据的排列组合，类似`笛卡尔积`，感兴趣的可以去了解相关的算法。在这儿我简单说一下我的思路吧。、

假设目前有如下的商品规格，解题思路就是顺序递归每一级规格

```javascript
// 商品规格数组
const skuArr = [['白色', '黑色'], ['大','中','小']]

function formateData(skuList, result = []) {
    let newResult = [];
    
    skuList.forEach(sku => {
        if(result.length){
            // 用来处理其余级规格的数据
        	result.forEach(item => {
                newResult.push([...item, sku])
            })    
        } else {
            // 用来处理第一级规格的数据
            newResult.push([sku])
        }  
    })
    
    return newResult;
}

// 存放结果数组
let result = [];

// 遍历每一级规格数据
for(let i = 0; i < skuArr.length; i++) {
    result = formateData(skuArr[i], result)
}

console.log(result); // [["白色","大"], ["黑色","大"], ["白色","中"], ["黑色","中"], ["白色","小"], ["黑色","小"]]
```

后来在处理表格展示的时候，发现当前的遍历结果在展示数据时有问题，`el-table`更希望的数据是这样的

```javascript
console.log(result); // [["白色","大"], ["白色","中"], ["白色","小"], ["黑色","大"], ["黑色","中"], ["黑色","小"]]
```

所以在遍历数据的时候需要调整一下数据遍历的先后关系，如下

```javascript
function formateData(skuList, result = []) {
    let newResult = [];
    
    result.forEach(item => {
        skuList.forEach(sku => newResult.push([...item, sku]))
    })
    
  	// result数据为空，说明是遍历第一级规格
    if(!result.length) skuList.forEach(sku => newResult.push([sku]))
    
    return newResult;
}
```

至此，该数据就能在页面中正常渲染展示了

![示例图](./src/assets/demo.png)
