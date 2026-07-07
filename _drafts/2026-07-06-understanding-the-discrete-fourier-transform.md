---
title: "Understanding the discrete Fourier transform"
layout: post
excerpt: "We explore the quirks of the discrete Fourier transform"
tags:
  - signal-processing
---

Consider the continuous-time signal
$x(t) = 5 \sin(10 \pi t) + 15 \sin(30 \pi t)$ for $t \geq 0$. Let's sample this
signal at $100$ Hz such that $t = n / 100$ for $n \in \mathbb N \cup \{0\}$ and
$x(n) = 5 \sin(0.1\pi n) + 15 \sin(0.3\pi n)$. This means that $x(n)$ contains
the normalized angular frequencies of $0.1\pi$ and $0.3\pi$ radians/sample.

We should then expect that a plot of the magnitude spectrum of this signal to
show peaks of 5 and 15 at $0.1\pi$ and $0.3\pi$ respectively. Let's plot this
magnitude spectrum in MATLAB using the code below.

```matlab
% Frequencies (cycles/second)
F1 = 5;
F2 = 20;

% Amplitudes
A1 = F1;
A2 = F2;

% Sampling frequency (samples/second)
Fs = 500;

% Normalized frequencies (cycles/sample)
f1 = F1/Fs;
f2 = F2/Fs;

% Normalized angular frequencies (radians/sample)
w1 = 2*pi*f1;
w2 = 2*pi*f2;

% Sampling indices
N = 100;
n = 0:1:N-1;

% Signal
x = A1 * sin(w1 * n) + A2 * sin(w2 * n);

% dft
X = fft(x);

% plot
figure;
tiledlayout(1,2);
nexttile;
k = 0:1:N-1; % cycle number
wk = 2*pi*k/Fs; % radians/sample
stem(wk,abs(X));

nexttile;
stem(wk,2/N * abs(X));
```
