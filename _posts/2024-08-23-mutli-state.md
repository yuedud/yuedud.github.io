# 如何优雅的去做多状态管理

## 背景
现在需要实现一个能力
有一个人物默认有一套装扮，在此基础上可以穿戴个性装扮
可以穿戴帽子、眼镜、衣服、裤子、鞋等等。

## 实现
如果按照旧有实现思路的话，可能我们会实现如下的对象
```ts
class Person {
    cap: string // 帽子资源地址
    glasses: string // 眼镜资源地址
    clothing: string // 衣服资源地址
    trousers: string //裤子资源地址
    shoes: string // 鞋资源地址
 
    constructor() {
        // 赋值默认的资源地址
        bootstrap()
    }
}
```
如果我们在此基础上去做个性装扮的话，我们可能会完成如下实现
```ts
class Person {
    cap: string // 帽子资源地址
    glasses: string // 眼镜资源地址
    clothing: string // 衣服资源地址
    trousers: string //裤子资源地址
    shoes: string // 鞋资源地址
 
    constructor() {
        // 赋值默认的资源地址
        bootstrap()
    }
 
    wearCap(cap: string) {
        this.cap = cap;
    }
 
    wearClothes(clothing: string) {
        this.clothing = clothing;
    }
 
    ...
}
```
这样实现的话，如果我们需要把个性装扮取消的话，就会发现之前的资源地址已经丢失，那我们就需要换一种实现
```
class Person {
    cap: string // 帽子资源地址
    glasses: string // 眼镜资源地址
    clothing: string // 衣服资源地址
    trousers: string //裤子资源地址
    shoes: string // 鞋资源地址
    
    isIndividualityCap: boolean // 是否穿戴了个性帽子
    individualityCap: string // 个性帽子地址
 
    constructor() {
        // 赋值默认的资源地址
        bootstrap()
    }
 
    wearCap(cap: string) {
        this.isIndividualityCap = true;
        this.individualityCap = cap;
    }
 
    ...
}
```

这样我们只需要判断isIndividualityCap是否是true来判断是否穿戴了个性装扮

但是如果这样的状态变多了之后，代码就变成了
```
class Person {
    cap: string // 帽子资源地址
    glasses: string // 眼镜资源地址
    clothing: string // 衣服资源地址
    trousers: string //裤子资源地址
    shoes: string // 鞋资源地址
    
    isIndividualityCap: boolean // 是否穿戴了个性帽子
    individualityCap: string // 个性帽子地址
 
    isIndividualityGlasses: boolean // 是否穿戴了个性眼镜
    individualityGlasses: string // 个性眼镜地址
 
    isIndividualityClothing: boolean // 是否穿戴了个性衣服
    individualityClothing: string // 个性衣服地址
 
    ...
}
```

状态就变得特别多，管理起来就非常复杂，那么有什么好的办法去管理这个状态呢？
