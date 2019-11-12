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



# 10.缓冲区问题（容量为80的缓冲区）

```
char Buffer[80];
semaphore empty = 80;
semaphore full = 0;
semaphore mutex = 1;
int count = 0;

Pi（） {
    while (true) {
        P(empty);
        str = getStr();    //生产一个字符
        P(mutex);
        Buffer[count++] = str;    //放一个字符
        if (count == 80) {
            V(full);
        }
        V(mutex);
    }
}

Q() {
    while(true) {
        P(full);
        P(mutex);
        for (int i=0; i<80; i++) {
            read(Buffer[i]);
            V(empty);
        }
        count = 0;
        V(mutex);
    }
}
```



# 11.缓冲区变式（三个生产者，三个消费者）

```
semaphore full = 1;
semaphore emptyi = 0;    //i=1,2,3

PSi(i=1,2,3) {
    while (true) {
        P(full);
        send(Mi);
        V(emptyi);
    }
}

PRi(i=1,2,3) {
    while (true) {
        P(emptyi);
        receive(Mi);
        V(full);
    }
}
```



# 12.哲学家就餐问题

```
semaphore chopstics[5] = {1,1,1,1,1}
semaphore mutex = 1;

Pi() {
    while (true) {
        P(mutex);    //当一个哲学家拿筷子时，其它的哲学家不能动
        P(chopstics[i]);        //拿起左边的筷子
        P(chopstics[(i+1)%5]);    //拿起右边的筷子
        进餐;
        V(mutex);
        V(chopstics[i]);
        V(chopstics[(i+1)%5]);
        思考;
    }
}
```



# 13.农夫猎人问题

```
semaphore cage = 1;
semaphore tiger = 0;
semaphore sheep = 0;

hunter() {
    while(true) {
        P(cage);
        放老虎;
        V(tiger);
    }
    
}

farmer() {
    while(true) {
        P(cage);
        放羊;
        V(sheep);
    }
}

zoo() {
    while(true) {
        P(tiger);
        取老虎;
        V(cage);
    }    
}

hotel() {
    while (true) {
        P(sheep);
        取羊;
        V(cage);
    }
}
```



# 14.银行业务问题

```
semaphore server_count = n;		//服务员数
semaphore customer_count = 0;	//顾客数
semaphore mutex = 1;	//对座位区的互斥访问量

Customer-i(i=1,2,3) {
    取号
    P(mutex);
    等待区找空沙发坐下;
    V(mutex);
    V(customer_count);
    P(server_count);
}

Server-j(i=1,2,3) {
    while(true) {
        P(customer_count);
        P(mutex);
        叫号;
        V(mutex);
        服务;
        客人离开;
        V(server_count);
    }
}
```



# 15.黑白棋子问题

这是一个交替访问问题

```
semaphore s1 = 1;	//假设先拣白子
semaphore s2 = 0;

P1() [
    while(true) {
        P(s1);
        拣白子;
        V(s2);
    }
]

P2() {
    while (true) {
        P(s2);
        拣黑子;
        V(s1);
    }
}
```



# 16.黑白棋子问题变式（50个棋子）

```
//下面两个信号量用于计数
semaphore white_count = 50;
semaphore black_count = 50;
//下面两个信号量用于互斥
semaphore s1 = 1;	//先黑后白
semaphore s2 = 0;

Zhang() {
	while (true) {
		P(s2);
		P(white_count);
		拣白子;
		V(s1);
	}
}

Li() {
	while (true) {
		P(s1);
		P(black_count);
		拣黑子;
		V(s2);
	}
}
```



# 17.独木桥问题

```
int east_count = 0;	//东边的汽车数量
int west_count = 0;	//西边的汽车数量
semaphore bridge = 1;	//互斥访问桥
semaphore east_mutex = 1;	//互斥访问east_count
semaphore west_mutex = 1;	//互斥访问west_count

east() {
    while(true) {
        P(east_mutex);	//请求东边的登记表
        if (east_count == 0) {
            P(bridge);
        }
        east_count++;	//东边汽车数量加1
        V(east_mutex);	//返还东边的登记表
        东边的汽车过桥;
        P(east_mutex);	//请求东边的登记表
        east_count--;
        if (east_count == 0) {
            V(bridge);
        }
        V(east_mutex);	//返还东边的登记表
    }
}

west() {	//和上边的代码完全一致
    while(true) {
        P(west_mutex);
        if (west_count == 0) {
            P(bridge);
        }
        west_count++;
        V(west_mutex);
        西边的汽车过桥;
        P(west_mutex);
        west_count--;
        if (west_count == 0) {
            V(bridge);
        }
        V(west_mutex);
    }
}
```



# 18.独木桥问题变式一（桥面上最多k辆车）

```
int east_count = 0;	//东边的汽车数量
int west_count = 0;	//西边的汽车数量
semaphore bridge = 1;	//互斥访问桥
semaphore east_mutex = 1;	//互斥访问east_count
semaphore west_mutex = 1;	//互斥访问west_count
semaphore capacity = k;

east() {
    while(true) {
        P(east_mutex);	
        if (east_count == 0) {
            P(bridge);
        }
        east_count++;	
        V(east_mutex);	
        P(capacity);	//只需要在上一题的基础上，增加一个PV操作
        东边的汽车过桥;
        V(capacity);
        P(east_mutex);	
        east_count--;
        if (east_count == 0) {
            V(bridge);
        }
        V(east_mutex);	
    }
}

west() {	//和上边的代码完全一致
    while(true) {
        P(west_mutex);
        if (west_count == 0) {
            P(bridge);
        }
        west_count++;
        V(west_mutex);
        P(capacity);	//只需要在上一题的基础上，增加一个PV操作
        西边的汽车过桥;
        P(west_mutex);
        V(capacity);
        west_count--;
        if (west_count == 0) {
            V(bridge);
        }
        V(west_mutex);
    }
}
```



# 19.独木桥问题变式二（串行过桥，能够阻止对方后继车辆）

```
int east_count = 0;	//东边的汽车数量
int west_count = 0;	//西边的汽车数量
semaphore bridge = 1;	//互斥访问桥
semaphore east_mutex = 1;	//互斥访问east_count
semaphore west_mutex = 1;	//互斥访问west_count
semaphore pass = 1;	//只需要在原题的基础上，增加一个pass信号量

east() {
    while(true) {
    	P(pass);
        P(east_mutex);	
        if (east_count == 0) {
            P(bridge);
        }
        east_count++;	
        V(east_mutex);	
        V(pass);
        东边的汽车过桥;
        P(east_mutex);	
        east_count--;
        if (east_count == 0) {
            V(bridge);
        }
        V(east_mutex);	
    }
}

west() {	//和上边的代码完全一致
    while(true) {
    	P(pass);
        P(west_mutex);
        if (west_count == 0) {
            P(bridge);
        }
        west_count++;
        V(west_mutex);
        V(pass);
        西边的汽车过桥;
        P(west_mutex);
        west_count--;
        if (west_count == 0) {
            V(bridge);
        }
        V(west_mutex);
    }
}
```



# 20.独木桥问题变式三（以组为单位交替通过汽车）

```
int east_count = 0;	//东边的汽车数量
int west_count = 0;	//西边的汽车数量
semaphore bridge = 1;	//互斥访问桥
semaphore east_mutex = 1;	//互斥访问east_count
semaphore west_mutex = 1;	//互斥访问west_count

east() {
    while(true) {
    	P(s1);	//用于实现交替过桥
        P(east_mutex);	
        if (east_count == 0) {
            P(bridge);
        }
        east_count++;	
        V(east_mutex);	
        东边的汽车过桥;
        V(s2);	//用于实现交替过桥
        P(east_mutex);	
        east_count--;
        east_pass++;	//记录东面这次过桥的汽车数量
        if (east_count == 0 && east_pash == 3) {	
        	east_pass = 0;	//重新计数
            V(bridge);
        }
        V(east_mutex);	
    }
}

west() {	
    while(true) {
    	P(s2);
        P(west_mutex);
        if (west_count == 0) {
            P(bridge);
        }
        west_count++;
        V(west_mutex);
        西边的汽车过桥;
        V(s1);
        P(west_mutex);
        west_count--;
        west_pass++;
        if (west_count == 0 && west_pass == 3) {
        	west_pass = 0;
            V(bridge);
        }
        V(west_mutex);
    }
}
```



# 21.机房问题

```
semaphore computerCounter = 2m;	//计算机数量
semaphore mutex = 1;
semaphore finish = 0;
semaphore test = 0;
semaphore enter = 0;
int rc = 0;	//学生数，初始化为0

student(i=1,2,3...) {
	P(computerCounter);	//没有空机器就一直等待
	P(mutex);	//rc是公共变量，多个学生可以同时访问，故需要互斥访问
	rc++;
	if (rc == 1) {	//如果想上机的学生只有一个
		V(mutex);	//把互斥访问权让出去，使后来的学生可以访问这个变量
		P(enter);	//等待搭档来才可以上机
	} else {
		rc = 0;		//两个学生进去后，把计数器置0
		V(mutex);	//把互斥访问访问权让出去
		V(enter);	
	}
	学生上机;
	V(finish);
	P(test);	//等待老师检查
	V(computerCounter);
}

teacher() {
	while (true) {
		P(finish);	//同一组的第一个学生好了
		P(finish);	//同一组的第二个学生好了
		检查实习结果;
		V(test);	//检查完一号学生
		V(test);	//检查完二号学生
	}
}
```



# 22.阅览室问题

```
struct{
	char name[10];
	int number;
}A[100];
for (int i=0; i<100; i++) {
	A[i].number = i;
	A[i].name = null;
}

semaphore mutex = 1;
semaphore seatCount = 100;

reader() {
	int no;	//方便出馆时找到自己注销
	P(seatCount);
	P(mutex);	//登记表是公共变量，需要互斥访问
	for (int i=0; i<100; i++) {
		if (A[i].name == null) {
			A[i].name = readName;
			no = i;
		}
	}
	V(mutex);
	找到位置，坐下读书;
	P(mutex);
	A[no].name == null;
	V(mutex);
	V(seatCount);
	离开阅览室;
	
}
```

