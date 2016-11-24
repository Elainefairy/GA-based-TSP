﻿## 基于遗传算法的旅行商最短路径规划问题 (GA-based TSP)

### Install 
    swig -c++ -python traveller.i
    python setup.py install


### Some Test:
> Python Test: 

    cd test
    python test.py
    
    
> C++ Test:

    gcc -std=c++11 -fPIC -lm -lstdc++ -pthread DPoint.cpp CityMap.cpp test.cpp -o test -g
    ./test city_num time_out

> Execute

* 设置城市数500，将随机生成 经度~(30.0, 40.0)，纬度~(110.0, 130.0)之间的城市
    * 城市距离没有使用球面模型计算，而是如下简化的计算方法：
        * 南北方向AM = 地球半径 \* 纬度差 \* Math.PI/180.0；
        * 东西方向BM = 地球半径 \* 经度差 \* Cos<当地纬度数 \* Math.PI/180.0>
        * 距离 = sqrt(AM\*AM + BM\*BM)
    * reference: http://tech.meituan.com/lucene-distance.html
* 遗传算法参数:
    * 种群数：种群数越大，遗传基因多样性越丰富
    * 子女数：每个种群遗传产生下一代的子女数
    * 最大转移数：变异时两个基因产生交叉的序列长度，该数值越大，变异性越大
    * ***灾变***：灾变初始值，灾变倍增值
        * 灾变的概念：这里设定当最优个体在连续多代遗传时都保持不变，就启动灾变倒计时。
        当倒计时为0时表示当前种群陷入***局部最优***，将种群基因重置，类似于物种灭绝并产生新物种
* 设置程序运行时间20秒，设置调试模式为true
* Result of Execution：

        [work@localhost test]$ python test.py 
        500个城市 第200代 最优产生于第199代 分数0.046215 里程137767986.669447 灾变倒计数0800 灾变计数000 群体平均分0.017644 用时210(ms)
        500个城市 第400代 最优产生于第398代 分数0.065158 里程97716073.922390 灾变倒计数0799 灾变计数000 群体平均分0.019714 用时370(ms)
        500个城市 第600代 最优产生于第599代 分数0.081307 里程78307826.891938 灾变倒计数0800 灾变计数000 群体平均分0.020653 用时520(ms)
        500个城市 第800代 最优产生于第798代 分数0.096583 里程65922802.285195 灾变倒计数0799 灾变计数000 群体平均分0.021502 用时670(ms)
        500个城市 第1000代 最优产生于第998代 分数0.109697 里程58041902.791668 灾变倒计数0799 灾变计数000 群体平均分0.023283 用时820(ms)
        500个城市 第1200代 最优产生于第1198代 分数0.121358 里程52464796.727875 灾变倒计数0799 灾变计数000 群体平均分0.024243 用时970(ms)
        ... 
        500个城市 第25800代 最优产生于第25674代 分数0.303750 里程20961321.872043 灾变倒计数3025 灾变计数000 群体平均分0.038376 用时19580(ms)
        500个城市 第26000代 最优产生于第25674代 分数0.303750 里程20961321.872043 灾变倒计数2825 灾变计数000 群体平均分0.037822 用时19730(ms)
        500个城市 第26200代 最优产生于第25674代 分数0.303750 里程20961321.872043 灾变倒计数2625 灾变计数000 群体平均分0.038433 用时19880(ms)
 
### Q&A:
* ERROR：traveller_wrap.cxx:2843:13: 错误：‘ptrdiff_t’不是一个类型名
                virtual ptrdiff_t distance(const SwigPyIterator &/*x*/) const\
    * Solution：类型ptrdiff_t在<stddef.h>中定义，在swig生成的traveller_wrap.cxx文件头部添加：#include<stddef.h>


