---
title: "42. Trapping Rain Water"
layout: post
tags:
  - two-pointers
  - leetcode-hard
---
## [Problem statement](https://leetcode.com/problems/trapping-rain-water/description/)

TODO: change structure so that they start with showing how to compute maximum area
of rain water using example, then provide more general two-step process.

We first solve this problem using an example input and then provide a more general
solution.

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

<figcaption>
<img src="figures/trapping_rain_water/trapping_rain_water_envelope.jpg">
<caption>Fig. 4</caption>
</figcaption>

In Fig. 4, the envelope is shown in green, which can be decomposed as the minimum of a "left envelope" shown in blue in Fig. 5a and a "right envelope" shown in purple in Fig. 5b.

<figcaption>
<img src="figures/trapping_rain_water/trapping_rain_water_left_envelope.jpg">
<caption>Fig. 5a</caption>
</figcaption>

<figcaption>
<img src="figures/trapping_rain_water/trapping_rain_water_right_envelope.jpg">
<caption>Fig. 5b</caption>
</figcaption>

Note that when the element-wise minimum of the arrays corresponding to the left and right envelopes is computed, we obtain an array whose values correspond to the envelope shown in green in Fig. 4.

To summarize, to compute the total area of trapped water, which is equivalent to the sum of all the outputs of the function $f$, we first decompose $f$ as the difference between the green and red lines shown in Fig. 3. We then decompose the green line into the blue and purple lines shown in Fig. 5a and Fig. 5b.

What remains to be done is to compute the arrays corresponding to the left and right envelopes shown in Fig. 5a and Fig. 5b.

We do so by noting that the left envelope corresponds to the "maximum-so-far", "running maximum", or "cumulative maximum" function of the bar heights starting from the left. That is, the height of the blue line at position `i` in Fig. 5a is equivalent to `max(height[0], height[1], ..., height[i])`.

To compute this cumulative maximum array `left_max` for a simpler array $[a_0,a_1,a_2,a_3]$, note that
$$
\begin{align}
\texttt{left\_max}[0] &= a_0 \\
\texttt{left\_max}[1] &= \max(a_0, a_1) \\
&= \max(\texttt{left\_max}[0], a_1) \\
\texttt{left\_max}[2] &= \max(a_0, a_1,a_2) \\
&= \max(\max(a_0, a_1),a_2) \\
&= \max(\texttt{left\_max}[1],a_2) \\
\texttt{left\_max}[3] &= \max(a_0, a_1, a_2, a_3) \\
&= \max(\max(\max(a_0, a_1),a_2),a_3) \\
&= \max(\texttt{left\_max}[2],a_3) \\
\end{align}
$$
More generally, for an array of length $N$,
$$
\texttt{left\_max}[k] =
\begin{cases}
a_0, &k = 0 \\
\max(\texttt{left\_max}[k-1],a_{k}), &k = 1,\dots,N-1
\end{cases}
$$
Applying this to the `height` array, we get
$$
\texttt{left\_max}[k] =
\begin{cases}
\texttt{height}[0], &k = 0 \\
\max(\texttt{left\_max}[k-1],\texttt{height}[k]), &k = 1,\dots,N-1
\end{cases} \tag{1}
$$
Similarly, the right envelope corresponds to the "maximum-so-far", "running maximum", or "cumulative maximum" function of the bar heights starting from the right. That is, the height of the purple line at position `i` in Fig. 5b is equivalent to `max(height[i], height[i+1], ..., height[-1])`.

To compute this cumulative maximum array `right_max` for a simpler array $[a_0,a_1,a_2,a_3]$, note that
$$
\begin{align}
\texttt{right\_max}[3] &= a_3 \\
\texttt{right\_max}[2] &= \max(a_2, a_3) \\
&= \max(a_2,\texttt{right\_max}[3]) \\
\texttt{\texttt{right\_max}}[1] &= \max(a_1, a_2,a_3) \\
&= \max(a_1,\max(a_2,a_3)) \\
&= \max(a_1,\texttt{right\_max}[2]) \\
\texttt{right\_max}[0] &= \max(a_0, a_1, a_2, a_3) \\
&= \max(a_0,\max(a_1,\max(a_2, a_3))) \\
&= \max(a_0,\texttt{right\_max}[1]) \\
\end{align}
$$
More generally, for an array of length $N$,
$$
\texttt{right\_max}[k] =
\begin{cases}
a_{N-1}, &k = N-1 \\
\max(a_k,\texttt{right\_max}[k+1]), &k = N-2,\dots,0
\end{cases}
$$
Applying this to the `height` array, we get
$$
\texttt{right\_max}[k] =
\begin{cases}
\texttt{height}[N-1], &k = N-1 \\
\max(\texttt{height}[k],\texttt{right\_max}[k+1]), &k = N-2,\dots,0
\end{cases} \tag{2}
$$
Moreover, the envelope array `envelope` shown in Fig. 4 is
$$
\texttt{envelope}[k] = \min(\texttt{left\_max}[k],\texttt{right\_max}[k])
$$
for $k = 0,\dots,N-1$.

Once the `envelope` array is computed, the function $f$ can be computed as
$$
f(k) = \texttt{envelope}[k] - \texttt{height}[k]
$$
for $k = 0,\dots,N-1$. Finally, the total area of water trapped is equal to
$$
\sum_{k=0}^{N-1} f(k)
$$
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
