---
title: "Squaring the square"
layout: post
tags:
  - puzzle
---
We prove that every square can be partitioned into $n$ squares for every $n \geq 6$.

This problem belongs to a family of problems known as [*squaring the square*](https://en.wikipedia.org/wiki/Squaring_the_square). What I particularly like about this problem is that the problem statement can be understood by anyone of almost any age, and that this problem admits some elegant solutions.

First, note that "partitioning a square" means to tile the square with other squares that are not necessarily of the same size. For example, Fig. 1 shows how to partition a square into $n = 1, 4, 9,$ and $16$ squares.

{% include figure.html
   filename="squaring-the-square/squaring-the-square-simple.svg"
   caption="Partitioning a square into $n = 1, 4, 9,$ and $16$ squares."
   fignum=1
   scale=70
%}

We see from Fig. 1 that a square can always be partitioned into $n$ squares of the same size when $n$ is a perfect square (i.e. its positive or negative square root is an integer). Hence, we have solved the problem for the case when $n \geq 6$ and $n$ is a perfect square. What remains is the case when $n \geq 6$ but $n$ is not a perfect square.

Looking closely at Fig. 1, we see that to go from $n = 1$ squares to $n = 4$ squares, we applied a "split" operation. That is, we partitioned a square into $4$ squares of equal size. This increases the total number of squares in the partition by $3$.

So, we can partition a square into $n = 4, 7, 10, 13, \dots$ by repeatedly applying this "split" operation to one of the squares in the $n = 4$ square, as shown in Fig. 2.

{% include figure.html
   filename="squaring-the-square/squaring-the-square-4-7-10.svg"
   caption='Partitioning a square into $n = 4,7,10,\dots$ squares using the "split" operation.'
   fignum=2
   scale=90
%}

Similarly, looking again at Fig. 1, we see that to go from $n = 4$ squares to $n = 1$ square, we applied a "merge" operation, which is the inverse of the "split" operation. That is, we replcaed $4$ neighboring squares with a single square. This decreased the total number of squares in the partition by $3$.

We can apply this "merge" operation to the case of $n=9$ squares to obtain a partition that consists of $n = 6$ squares, as shown in Fig. 3.

{% include figure.html
   filename="squaring-the-square/squaring-the-square-merge.svg"
   caption='Merging a $9$-partition square into a $6$-partition square.'
   fignum=3
   scale=50
%}

We can then repeatedly apply the "split" operation on one of the squares in the $6$-partition square to obtain the sequence $n = 6, 9, 12, 15,\dots$. To summarize, we have found the following sequences of partitions:

$$
\begin{align*}
n &= 6, 9, 12, 15, 18, \dots \\
n &= 7, 10, 13, 16, 19, \dots
\end{align*}
$$

The only sequence remaining is $n = 8, 11, 14, 17, \dots$. This sequence can be obtained by paritioning a square into $8$ squares and then repeatedly applying the "split" operation. We can partition a square into $8$ squares by applying the "merge" operation to one of the $3 \times 3$ sub-squares in the $n = 16$ partition, as shown in Fig. 4.

{% include figure.html
   filename="squaring-the-square/squaring-the-square-merge-16-8.svg"
   caption='Merging a $16$-partition square into an $8$-partition square.'
   fignum=4
   scale=50
%}

We can then repeatedly apply the "split" operation to one of the squares in this $8$-partition to obtain the desired sequence, which concludes the solution to this problem.
