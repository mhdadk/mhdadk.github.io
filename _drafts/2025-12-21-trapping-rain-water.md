---
title: "42. Trapping Rain Water"
layout: post
tags:
  - two-pointers
  - leetcode-hard
---
## [Problem statement](https://leetcode.com/problems/trapping-rain-water/description/)

We show how to solve this problem using an example input. We first describe a solution
that requires $O(n)$ time and space. Then, we explain how to further optimize this
solution to reduce the space required to $O(1)$.

## Preliminaries

Consider the input array `height = [0,2,0,3,1,0,1,3,2,1]`. The maximum area of water
trapped for this `height` array is shown in Fig. 1.

{% include figure.html
   filename="trapping-rain-water/trapping_rain_water_orig.jpg"
   caption='Maximum area of rain water trapped for `height = [0,2,0,3,1,0,1,3,2,1]`.'
   fignum=1
   scale=90
%}

We then overlay the amount of water trapped at the $i$th position, as shown in Fig. 2.

{% include figure.html
   filename="trapping-rain-water/trapping_rain_water_per_pos.jpg"
   caption='The amount of water trapped at the $i$th position based on Fig. 1.'
   fignum=2
   scale=90
%}

The amount of water that can be trapped at the $i$th position is shown in orange. The
orange line also corresponds to a plot of the function
$f : \{0,1,\dots,N-1\} \to \mathbb N \cup \{0\}$, where $N$ is the length of the `height`
array, that returns the amount of water that can be trapped at the $i$th position in the
`height` array.

The total amount of water trapped is equivalent to the area under the orange line. That
is, we compute the total area of water trapped as $\sum_{k=0}^{N-1} f(k)$.

## Decomposing $f$

The function $f$ can be decomposed as the difference between the "envelope" of the bars
in Fig. 2 and the height of the bars, as shown in Fig. 3.

{% include figure.html
   filename="trapping-rain-water/trapping_rain_water_difference.jpg"
   caption='The difference between the envelope of the bars and the height of the bars. The envelope of the bars is shown in green while the height of the bars is shown in red'
   fignum=3
   scale=90
%}

In Fig. 3, the function $f$ shown in orange is obtained by subtracting the red line
(bar heights) from the green line (envelope). Our goal then is to construct the green
and red lines and then subtract them to obtain the function $f$.

We already know how to construct the red line, since it is equivalent to the values in the `height` array. To construct the green line, we decompose it as the minimum of two lines, as shown in Fig. 4.

{% include figure.html
   filename="trapping-rain-water/trapping_rain_water_envelope.jpg"
   caption='How to construct the envelope of the bars. The envelope is shown in green.'
   fignum=4
   scale=90
%}

In Fig. 4, the envelope can be decomposed as the minimum of a "left envelope" shown in
blue in Fig. 5a and a "right envelope" shown in purple in Fig. 5b.

{% include figure.html
   filename="trapping-rain-water/trapping_rain_water_left_envelope.jpg"
   caption='Left envelope.'
   fignum=5a
   scale=90
%}

{% include figure.html
   filename="trapping-rain-water/trapping_rain_water_right_envelope.jpg"
   caption='Right envelope.'
   fignum=5b
   scale=90
%}

The envelope in Fig. 4 is the result of computing the element-wise minimum of the left
and right envelopes shown in Fig. 5a and 5b, respectively.

To summarize, we compute the total area of trapped water as follows:

1. Decompose $f$ as the difference between the green and red lines shown in Fig. 3.
2. Decompose the green line into the blue and purple lines shown in Fig. 5a and 5b, respectively.
3. Compute the heights of the blue and purple lines.
4. Compute $f$ at each position.
5. Sum all the outputs of $f$ to obtain the total area of trapped water.

We now describe how to compute the blue and purple lines (i.e. the left and right envelopes).

## Computing the left and right envelopes

For convenience, let $\texttt{height}[i] = h_i$.

The left and right envelopes correspond to the cumulative maximum of the bar heights
starting from the left and right, respectively. That is, the height of the blue and
purple lines in Fig. 5a and Fig. 5b, respectively, at the $i$th position are
$\ell_i = \max(h_0, h_1, \dots, h_i)$ and $r_i = \max(h_{N-(i+1)}, \dots, h_{N-2}, h_{N-1})$, respectively.

Because

$$
\begin{align}
\ell_i &= \max(h_0, h_1, \dots h_i) \\
&= \max(\max(\max(\max(h_0, h_1),h_2),h_3), ..., h_i) \\
&= \max(\max(\max(\max(\max(h_0, h_0), h_1),h_2),h_3), ..., h_i) \\
\r_i &= \max(h_{N-(i+1)}, \dots, h_{N-2}, h_{N-1}) \\
&= \max(h_{N-(i+1)}, \dots, \max(h_{N-4},\max(h_{N-3}, \max(h_{N-2}, h_{N-1})))) \\
&= \max(h_{N-(i+1)}, \dots, \max(h_{N-4},\max(h_{N-3}, \max(h_{N-2}, \max(h_{N-1}, h_{N-1})))))
\end{align}
$$

we can compute $\ell_i$ and $r_i$ recursively as

$$
\begin{align}
\ell_i &= \begin{cases} h_0, &i = 0 \\ \max(\ell_{i-1},h_i), &i = 1,\dots,N-1\end{cases} \\
\r_i &= \begin{cases} h_{N-1}, &i = 0 \\ \max(\r_{i-1},h_{N-(i+1)}), &i = 1,\dots,N-1\end{cases}
\end{align}
$$

## Computing the envelope, $f$, and the area of water trapped

Once we have computed the left and right envelopes $\ell_i$ and $r_i$ for $i = 0,\dots,N-1$,
the envelope shown in Fig. 4 is $e_i = \min(\ell_i,r_i)$ for $i = 0,\dots,N-1$.

The function $f$ is then be computed as $f(k) = e_k - h_k$ for $k = 0,\dots,N-1$ and
the total area of water trapped is equal to $\sum_{k=0}^{N-1} f(k)$.

## Implementation

Because this solution required the use of the `left_max` and `right_max` arrays, then it requires $O(n)$ space. Additionally, only single passes are made over the `height` array, so this solution requires $O(n)$ time.
# $O(1)$ space
We can simplify the solution given above to only require $O(1)$ space. The key insight needed to do this is to notice that `left_max` is a monotonically increasing sequence from the left and `right_max` is a monotonically increasing sequence from the right. This means that
$$
\begin{align}
\texttt{left\_max}[0] &\leq \texttt{left\_max}[1] \leq \cdots \leq \texttt{left\_max}[k] \\
\texttt{right\_max}[N-1-k] &\geq \cdots \geq \texttt{right\_max}[N-2] \geq \texttt{right\_max}[N-1]
\end{align}
$$
for $k = 0,\dots,N-1$.

Hence, we can infer the following. Let $i < j$. If $\texttt{left\_max}[i] \leq \texttt{right\_max}[j]$, then
$$
\begin{align}
\texttt{left\_max}[i] &\leq \texttt{right\_max}[j] \\
&\leq \texttt{right\_max}[j-1] \\
&\leq \texttt{right\_max}[j-2] \\
&\leq \cdots \\
&\leq \texttt{right\_max}[0]
\end{align}
$$
Alternatively, if $\texttt{right\_max}[j] < \texttt{left\_max}[i]$, then
$$
\begin{align}
\texttt{right\_max}[j] &< \texttt{left\_max}[i] \\
&< \texttt{left\_max}[i + 1] \\
&< \texttt{left\_max}[i + 2] \\
&< \cdots \\
&< \texttt{left\_max}[N-1]
\end{align}
$$
Using these relationships, consider $\texttt{left\_max}[0]$ and $\texttt{right\_max}[N-1]$. If $\texttt{left\_max}[0] \leq \texttt{right\_max}[N-1]$, then
$$
\begin{align}
\texttt{left\_max}[0] &\leq \texttt{right\_max}[N-1] \\
&\leq \texttt{right\_max}[N-2] \\
&\leq \cdots \\
&\leq \texttt{right\_max}[0]
\end{align}
$$
and so
$$
\begin{align}
\texttt{envelope}[0] &= \min(\texttt{left\_max}[0],\texttt{right\_max}[0]) \\
&= \texttt{left\_max}[0]
\end{align}
$$
Alternatively, if $\texttt{right\_max}[N-1] < \texttt{left\_max}[0]$, then
$$
\begin{align}
\texttt{right\_max}[N-1] &< \texttt{left\_max}[0] \\
&< \texttt{left\_max}[1] \\
&< \cdots \\
&< \texttt{left\_max}[N-1]
\end{align}
$$
and so
$$
\begin{align}
\texttt{envelope}[N-1] &= \min(\texttt{left\_max}[N-1],\texttt{right\_max}[N-1]) \\
&= \texttt{right\_max}[N-1]
\end{align}
$$
For the sake of explanation, suppose that $\texttt{left\_max}[0] \leq \texttt{right\_max}[N-1]$ such that $\texttt{envelope}[0] = \texttt{left\_max}[0]$. This means that we were able to compute `envelope[0]`, but we want to also compute `envelope[N-1]`.

We can try to compare `left_max[0]` and `right_max[N-2]`, but as we have already established that $\texttt{left\_max}[0] \leq \texttt{right\_max}[N-1]$, then $\texttt{left\_max}[0] \leq \texttt{right\_max}[N-2]$. This does not give us any new information. So, we instead compare `left_max[1]` with `right_max[N-1]`.

Suppose instead that $\texttt{left\_max}[0] > \texttt{right\_max}[N-1]$ such that $\texttt{envelope}[N-1] = \texttt{left\_max}[N-1]$. This means that we were able to compute `envelope[N-1]`, but we want to also compute `envelope[0]`.

We can try to compare `left_max[1]` and `right_max[N-1]`, but as we have already established that $\texttt{left\_max}[0] > \texttt{right\_max}[N-1]$, then $\texttt{left\_max}[1] > \texttt{right\_max}[N-1]$. This does not give us any new information. So, we instead compare `left_max[0]` with `right_max[N-2]`.

For convenience, let us replace "$0$" and "$N-1$" in the analysis above with the left and right pointers $\ell = 0$ and $r = N-1$ respectively. Then, the logic above simplifies to: "If $\texttt{left\_max}[\ell] \leq \texttt{right\_max}[r]$, then $\ell \gets \ell + 1$. Otherwise, $r \gets r - 1$".

Furthermore, if $\texttt{left\_max}[\ell] \leq \texttt{right\_max}[r]$, we will need to compute $\texttt{left\_max}[\ell + 1]$ for the next iteration. To do so, recall from $(1)$ above that
$$
\texttt{left\_max}[\ell + 1] = \max(\texttt{left\_max}[\ell],\texttt{height}[\ell + 1])
$$
Alternatively, if $\texttt{left\_max}[\ell] > \texttt{right\_max}[r]$, we will need to compute $\texttt{right\_max}[r - 1]$ for the next iteration. To do so, recall from $(2)$ above that
$$
\texttt{right\_max}[r - 1] = \max(\texttt{right\_max}[r], \texttt{height}[r-1])
$$
To summarize, if $\texttt{left\_max}[\ell] \leq \texttt{right\_max}[r]$, we will need to do the following:
1. Compute $\texttt{envelope}[\ell]$.
2. Compute $f[\ell]$.
3. Accumulate the area of water.
4. Compute $\texttt{left\_max}[\ell + 1]$ for the next iteration.
5. Move the left pointer $\ell$.

Otherwise, if $\texttt{left\_max}[\ell] > \texttt{right\_max}[r]$, we will need to do the following instead:
1. Compute $\texttt{envelope}[f]$.
2. Compute $f[r]$.
3. Accumulate the area of water.
4. Compute $\texttt{right\_max}[r - 1]$ for the next iteration.
5. Move the right pointer $r$.

Here is a complete version of this algorithm for computing the total area of water:
1. Start with two pointers, each at opposite ends of the array `height`. Let the left end have index `l` (or $\ell$) and the right end have index `r` (or $r$).
2. For the first iteration, let $\texttt{left\_max} = \texttt{height}[\ell]$ and $\texttt{right\_max} = \texttt{height}[r]$. We do not need to use arrays for $\texttt{left\_max}$ and $\texttt{right\_max}$.
3. Let the total water area be $A = 0$.
4. While $\ell < r$,
	1. If $\texttt{left\_max} \leq \texttt{right\_max}$,
		1. Compute $\texttt{envelope}[\ell]$: $\texttt{envelope} \gets \texttt{left\_max}$
		2. Compute $f[\ell]$: $f \gets \texttt{envelope} - \texttt{height}[\ell]$
		3. Accumulate the area of water: $A \gets A + f$
		4. Compute $\texttt{left\_max}[\ell + 1]$ for the next iteration: $\texttt{left\_max} \gets \max(\texttt{left\_max}, \texttt{height}[\ell + 1])$
		5. Move the left pointer: $\ell \gets \ell + 1$
	2. Otherwise,
		1. Compute $\texttt{envelope}[r]$: $\texttt{envelope} \gets \texttt{right\_max}$
		2. Compute $f[r]$: $f \gets \texttt{envelope} - \texttt{height}[r]$
		3. Accumulate the area of water: $A \gets A + f$
		4. Compute $\texttt{right\_max}[r - 1]$ for the next iteration: $\texttt{right\_max} \gets \max(\texttt{right\_max}, \texttt{height}[r])$
		5. Move the right pointer: $r \gets r - 1$
5. Return $A$.

Additionally, here is the Python code used to implement this algorithm:
```python
def trap(self, height: List[int]) -> int:
    l, r = 0, len(height) - 1
    left_max = height[l]
    right_max = height[r]
    A = 0
    while l < r:
        if left_max <= right_max:
            # compute envelope[l]
            envelope = left_max
            # compute f[l]
            f = envelope - height[l]
            # accumulate the area of water
            A += f
            # compute left_max[l + 1] for the next iteration
            left_max = max(left_max, height[l+1])
            # move the left pointer
            l += 1
        else:
            # compute envelope[r]
            envelope = right_max
            # compute f[r]
            f = envelope - height[r]
            # accumulate the area of water
            A += f
            # compute right_max[r - 1] for the next iteration
            right_max = max(right_max, height[r - 1])
            # move the right pointer
            r -= 1
    return A
```
