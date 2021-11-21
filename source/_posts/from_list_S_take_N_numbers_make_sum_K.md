---
title: 集合S取出n个元素其和为K个数统计
toc: true
date: 2018-11-17 08:17:17
tags:
- 笛卡尔积
- 斗地主
categories:
- 逻辑思维
---
# 序
最近csrediscore的创作者在制作一个斗地主的机器人,他探讨性地给我出了一个问题——**怎么样去统计手牌组合的可能性？**  
该问题算是比较复杂的,在不考虑癞子的情况下有**火箭、炸弹、单牌、对牌、连对、三张牌、三带一、单顺、双顺、三顺、飞机带翅膀、四带二**等等牌型。    
以地主20张手牌为例：
* 20张手牌中能打出火箭次数在[0,1]中取值。
* 20张手牌中能打出炸弹次数在[0,5]中取值。
* 20张手牌中能打出单牌次数在[0,20]中取值——在不考虑其他玩家的情况下,最多出20次单牌。
* 依次类推…………  
所以问题转换为**每种牌型中选取任意次数构成N张手牌的情况有多少种？**

# 问题分解一
为了更加简单一点描述问题,我继续对问题进行了简化。  
**从集合{1,2,3,4}中,取出一个元素和为10的个数统计**  
比如集合为：{1,2,3,4}，和值为10；其中取法1,2,3,4和4,3,2,1等价。  
因和值固定，且都为正数，所以每个元素的取出次数有限，可以得出结论如下图  
![1,2,3,4可以取出次数分析](1234_10.png)  

# 问题分解二  
```
从集合
{0,1,2,3,4,5,6,7,8,9,10},
{0,2,4,6,8,10},
{0,3,6,9},
{0,4,8}
中各取出一个元素组成新集合S1,S1求和为10的个数统计。
```
由图可知,可以取出的元素组合情况为:  11 \*  6 \* 4 \* 3=792种。

最终只需要在这792种方案中选取出和为10的记录。
罗列792种方案的可行性的过程叫做求笛卡尔积,以下给出代码实现C#版代码实现过程。

# 代码实现
求笛卡尔积扩展类
```
    public static class EnumerableEx
    {
        /// <summary>
        /// 求集合的笛卡尔积
        /// </summary>
        public static IEnumerable<IEnumerable<T>> Cartesian<T>(this IEnumerable<IEnumerable<T>> sequences)
        {
            IEnumerable<IEnumerable<T>> tempProduct = new[] {Enumerable.Empty<T>()};
            return sequences.Aggregate(tempProduct,
                (current, sequence) =>
                    (from accseq in current from item in sequence select accseq.Concat(new[] {item})));
        }
    }
```
创建数字集合类
```
      /// <summary>
        /// 数字集合
        /// </summary>
        public class DigitGroup
        {
            /// <summary>
            /// 元素
            /// </summary>
            public int Value;

            /// <summary>
            /// 次数
            /// </summary>
            public int Times;

            /// <summary>
            /// 和
            /// </summary>
            public int Sum;

            public DigitGroup(int value, int times)
            {
                this.Value = value;
                this.Times = times;
                this.Sum = value * times;
            }

            public override string ToString()
            {
                return string.Format("{0}个{1},和为{2}", Times, Value, Sum);
            }
        }
```

控制台Program类
```
  class Program
    {
        static void Main(string[] args)
        {
            List<int> numbers = new List<int> {1, 2, 3, 4};
            int Sum = 10;
            var digitGroupList = GetChooseList(numbers, Sum);

            var result = digitGroupList.Cartesian();
            result = result.Where(chooses => chooses.Sum(choose => choose.Sum) == Sum);
            PrintResult(result);

            Console.ReadKey();
        }

        private static IEnumerable<IEnumerable<DigitGroup>> GetChooseList(List<int> intList, int target)
        {
            List<List<DigitGroup>> newList = new List<List<DigitGroup>>();
            foreach (var beichushu in intList)
            {
                List<DigitGroup> temp = new List<DigitGroup>();
                var count = target / beichushu;
                for (int i = 0; i <= count; i++)
                {
                    temp.Add(new DigitGroup(beichushu, i));
                }

                newList.Add(temp);
            }

            return newList;
        }

        private static void PrintResult(IEnumerable<IEnumerable<DigitGroup>> result)
        {
            int index = 0;
            foreach (var list in result)
            {
                index += 1;
                Console.Write(index + ": ");
                foreach (var choose in list)
                {
                    if (choose.Sum != 0)
                    {
                        Console.Write(" " + choose + "   ");
                    }
                }

                Console.WriteLine();
            }
        }
    }
```
最终运行结果如下
```
1:  2个3,和为6    1个4,和为4
2:  1个2,和为2    2个4,和为8
3:  2个2,和为4    2个3,和为6
4:  3个2,和为6    1个4,和为4
5:  5个2,和为10
6:  1个1,和为1    3个3,和为9
7:  1个1,和为1    1个2,和为2    1个3,和为3    1个4,和为4
8:  1个1,和为1    3个2,和为6    1个3,和为3
9:  2个1,和为2    2个4,和为8
10:  2个1,和为2    1个2,和为2    2个3,和为6
11:  2个1,和为2    2个2,和为4    1个4,和为4
12:  2个1,和为2    4个2,和为8
13:  3个1,和为3    1个3,和为3    1个4,和为4
14:  3个1,和为3    2个2,和为4    1个3,和为3
15:  4个1,和为4    2个3,和为6
16:  4个1,和为4    1个2,和为2    1个4,和为4
17:  4个1,和为4    3个2,和为6
18:  5个1,和为5    1个2,和为2    1个3,和为3
19:  6个1,和为6    1个4,和为4
20:  6个1,和为6    2个2,和为4
21:  7个1,和为7    1个3,和为3
22:  8个1,和为8    1个2,和为2
23:  10个1,和为10
```
# 总结
  我现在依旧不知道20张手牌,各种牌型组合的可能性有多少种,但是将问题转化成{1,2,3,4}求和为10的这种方式已经将问题做了一个很好的分解。
  只需要将DigitGroup再做一下相应的替换就可以计算出可能的牌型有多少种，该问题的求解已经不在该文的范畴类，有兴趣的读者可以尝试解决一下。

# 参考资料
* [C# 用Linq的方式实现组合和笛卡尔积（支持泛型T）](https://www.cnblogs.com/localhost2016/p/8668355.html)
* [欢乐斗地主出牌方式统计](https://ddabb.github.io/Fight-the-Landlord-Card-Type-Aanalysis/)
