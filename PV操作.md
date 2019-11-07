# 1.基本操作

找出所有进程→找出所有事件→把事件放入对应的进程→判断同步互斥关系→添加PV操作

P操作：我提供什么

V操作：我想要什么

事件一定被P操作和V操作夹着，有P操作就一定有一个对应的V操作

```
1.同步
semaphore signal = 0;

Process-A {
    我说完，B才能说
    V(signal);
}

Process-B {
    P(signal);
    A说完，我才能说
}
    
2.交替访问    
semaphore s1 = 1;
semaphore s2 = 0;

Process-A {
    while (true) {
        P(s1);
        我说一句就停下来，B说一句
        V(s2);
    }
}

Process-B {
    while (true) {
        P(s2);
        我说一句就停下来，A说一句
        V(s1);
    }
}

3.互斥
semaphore mutex = 1;

Process-A {
    while (true) {
        P(mutex);
        互斥访问
        V(mutex);
    }
}

Process-B {
    while (true) {
        P(mutex);
        互斥访问
        V(mutex);
    }
}
```

# 2.读写者问题

```
//读者优先
```

```
//写着优先
```



# 3.吸烟者问题

问题描述：假设一个系统有三个抽烟者进程和一个供应者进程。每个抽烟者不停地卷烟 并抽掉它，但是要卷起并抽掉一支烟，抽烟者需要有三种材料：烟草、纸和胶水。三个抽烟 者中，第一个拥有烟草、第二个拥有纸，第三个拥有胶水。供应者进程无限地提供三种材料， 供应者每次将两种材料放到桌子上，拥有剩下那种材料的抽烟者卷一根烟并抽掉它，并给供 应者一个信号告诉完成了，供应者就会放另外两种材料在桌上，这种过程一直重复（让三个 抽烟者轮流地抽烟)。

```
semaphore offer1 = 0;    //提供卷纸和火柴
semaphore offer2 = 0;    //提供火柴和烟草
semaphore offer3 = 0;    //提供卷纸和烟草
semaphore finish = 1;    //通知供应者提供材料

provider() {
    int rand;	//随机数
    while(true) {
        P(finish);
        rand = getRandom() % 3;
        if (rand == 0) {
            V(offer0);
        } else if (rand == 1) {
            V(offer2);
        } else {
            V(offer3);
        }
    }
}

smoker1() {
    while (true) {
        P(offer1);
        制作香烟并吸烟;        //事件
        V(finish);
    }
}

smoker2() {
    while (true) {
        P(offer2);
        制作香烟并吸烟;        //事件
        V(finish);
    }
}

smoker3() {
    while (true) {
        P(offer3);
        制作香烟并吸烟;        //事件
        V(finish);
    }
}
```



# 4.售票问题

问题描述：汽车司机与售票员之间必须协同工作，一方面只有售票员把车门关好了司机才能开车，因此，售票员关好门应通知司机开车，然后售票员进行售票。另 一方面，只有当汽车已经停下，售票员才能开门上下客，故司机停车后应该通知售票员。假定某辆公共汽车上有一名司机与两名售票员，汽车当前正在始发站停车上客，试用信号量与P、V操作写出他们的同步算法。

这是一个同步问题。

```
semaphore close1;
semaphore close2;
semaphore open1;
semaphore open2;

driver() {
    P(close1);
    P(close2);
    开车;        //事件
    V(open1);
    V(open2);
}

conductor1() {
    售票;
    关门
    V(close1);
    ...客车运行中
    P(open1);
    开门;
}

conductor2() {
    售票;
    关门
    V(close2);
    ...客车运行中
    P(open2);
    开门;
}
```



# 5.橘子-苹果问题（一儿一女，盘子只能放一个水果）

问题描述：

这是一个同步问题。

```
semaphore plate = 1;    //空盘子
semaphore apple = 0;    //有苹果的盘子
semaphore orange = 0;   //有橘子的盘子

father() {
    while(true) {
        P(plate);
        放苹果;
        V(apple);
    }
}

mother() {
    while(true) {
        P(plate);
        放橘子;
        V(orange);
    }
}

son() {
    while (true) {
        P(orange);
        取橘子;
        V(plate);
    }
}

doughter() {
    while (true) {
        P(apple);
        取苹果;
        V(apple);
    }
}
```



# 6.橘子-苹果问题变式一（一儿一女，能放N个水果）

问题描述：

```
semaphore mutex = 1;
semaphore empty = N;	//n个空位
semaphore appleCount = 0;	//苹果数目
semaphore orangeCount = 0;	//橘子数目

father() {
    while(true) {
        P(empty);    //只要盘子中有空位，就可以放苹果
        P(mutex);	 //我用盘子时，你们都不能用
        放苹果;
        V(appleCount);
        V(mutex);
    }
}

mother() {
    while(true) {
        P(empty);   
        P(mutex);
        放橘子;
        V(orangeCount);
        V(mutex);
    }
}

son() {
    while (true) {
        P(orangeCount);
        P(mutex);
        取橘子;
        V(empty);
        V(mutex);
    }
}

doughter() {
    while (true) {
        P(appleCount);
        P(mutex);
        取橘子;
        V(empty);
        V(mutex);
    }
}
```



# 7.橘子-苹果问题（二儿二女，可以放二个水果）

问题描述：

在变式一的基础上增加了交替访问。

```
semaphore signal = 0;    //用于实现交替放入水果
semaphore mutex = 1;    //用于实现互斥访问盘子
semaphore apple = 0;    //盘子中是否有苹果
semaphore orange = 0;    //盘子中是否有橘子
semaphore appleEmpty = 1;    //盘子中无苹果时，爸爸才能放苹果
semaphore orangeEmpty = 1;    //盘子中无橘子时，爸爸才能放橘子

father() {
    while(true) {
        P(appleEmpty);    //盘子中没有苹果，可以准备将苹果放入盘子
        P(mutex);    //互斥使用盘子
        放苹果
        V(mutex);
        V(apple);    //通知女儿有苹果了
        V(signal);    //通知妈妈可以放橘子了
    }
}

mother() {
    while(true) {
        P(orangeEmpty);    //盘子中没有橘子，可以准备将橘子放入盘子
        P(signal);    //等爸爸放完苹果，妈妈才能放橘子
        P(mutex);
        放橘子
        V(mutex);
        V(orange);    //通知儿子有橘子了
    }
}

son() {
    while (true) {
        P(orange);
        P(mutex);
        取橘子;
        V(mutex);
        V(orangeEmpty);
    }
}

doughter() {
    while (true) {
        P(apple);
        P(mutex);
        取橘苹果;
        V(mutex);
        V(appleEmpty);
    }
} 
```



# 8.理发师问题

问题描述：

```
int CHAIRS = n;    //理发椅的数目
semaphore customers = 0;
semaphore barbers = 0;
semaphore mutex = 1;    //对椅子的互斥访问


barber() {
    while(true) {
        P(customers);    //有没有顾客？没有就休息
        P(mutex);        //如果有顾客，理发师被叫醒，修改空椅子的数量
        CHAIRS++;    //空椅子多出了一个
        V(barbers);        //有理发师准备为顾客理发，V(barbers)，退出临界区
        理发
        )
    }
}


customer() {
    P(mutex);    //进入临界区
    if (CHAIRS > 0) {    //有空椅子
        CHAIRS--;
        V(customers);    //唤醒理发师
        V(mutex);    //退出临界区;
        P(barbers);    //一直等待理发师    
        坐下理发;
    } else {
        V(mutex);    //人满了，走了
    }
}
```



# 9.生产与装配车间问题

问题描述：

```
semaphore empty1 = 10;	//货架F1上的空闲空间
semaphore full1 = 0;	//F1上的A产品数目
semaphore empty2 = 10;	//货架F2上的空闲空间
semaphore full2 = 0;	//F2上的B产品数目
semaphore mutex1 = 1;	//对F1互斥访问
semaphore mutex2 = 1;	//对F2互斥访问

A车间 {
    while(true) {
        P(empty1);
        生产一个产品A
        P(mutex1);
        将产品A放到货架F1上
        V(mutex1);
        V(full1);
    }
}

B车间 {
    while(true) {
        P(empty2);
        生产一个产品B
        P(mutex2);
        将产品B放到货架F1上
        V(mutex2);
        V(full2);
    }
}

装配工 {
    while(true) {
        P(full1);
        P(mutex1);
        从货架上取一个A产品
        V(mutex1);
        V(empty1);
        P(full2);
        P(mutex2);
        从货架上取一个B产品
        V(mutex2);
        V(empty2);
        组装...
    }
}
```

