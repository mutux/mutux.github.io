---
layout: post
title: 'String Algorithm: Bloom Filter'
date: '2017-04-26T12:50:00.002-07:00'
author: mutux
tags:
modified_time: '2017-04-26T12:53:12.600-07:00'
blogger_id: tag:blogger.com,1999:blog-7424095106938843032.post-6297431923901521855
blogger_orig_url: http://mutux.blogspot.com/2017/04/string-algorithm-bloom-filter.html
---

Just like hash table with multiple $$(k)$$ hash functions. The searching time complexity of bloom filter is $$O(k)$$, just as that of hash table $$O(1)$$.
For hash table, in order to keep the the false positive rate sufficiently low, the array must be very large to make sure that only a small fraction of bits are set to 1. Whereas for Bloom filter, the fraction could be relatively big (~50%) if the number $$k$$ (the number of hash functions) and $$m$$ (the size of bit array) are properly set.

If $$m$$ is the number of bits in a the array, the probability that a certain bit is not set to 1 by all the k hash functions during the insertion of an element is

\begin{equation}
\left(1-\frac{1}{m}\right)^k
\end{equation}

If we have inserted $$n$$ elements, the probability that a certain bit is still 0 is
\begin{equation}
\left(1-\frac{1}{m}\right)^{kn}
\end{equation}

The Probability that it is 1 therefore
\begin{equation}
1-\left(1-\frac{1}{m}\right)^{kn} \approx 1-e^{-kn/m}
\end{equation}

# False Positive Probability

+ Suppose we are trying to insert the $$(n+1)_{th}$$ $$x$$, which is different from all the previous n keys, into the Bloom filter. The probability that the search result indicates that x is already in the Bloom filter (which means the positions to which the hash functions map x are all set to 1) is:
\begin{equation}
P(k) = \left(1-\left[1-\frac{1}{m}\right]^{kn}\right)^k \approx \left(1-e^{-kn/m}\right)^k
\end{equation}
+ That's right the **false positive probability** of a Bloom filter with $$m$$ bits, $$k$$ hash functions, and $$n$$ keys or elements.

# Optimal Number of K

Suppose $$m$$ and $$n$$ are fixed, and $$\alpha = n/m$$ is fixed, the false positive probability is then written as:

$$
P(k) = \left(1-e^{-\alpha k}\right)^k
$$


Generally we get the optimal number of $$K$$ by minimizing $$P$$, which involves the derivative of $$P$$ with respect to $$k$$, and $$P$$ is a composite exponential function of $$k$$, the trick for inferring the derivative form of a composite exponential function is as follow:

$$
\begin{split}
\ln(p) &= k \ln(1-e^{-\alpha k})\\
z &= 1-q\\
q &= e^t\\
t &= -\alpha k\\
\ln(p) &= k \ln(z(q(t(k))))\\
z_k^\prime &= z_q^\prime \cdot q_t^\prime \cdot t_k^\prime \\
&= -1 \cdot e^t \cdot -\alpha \\
&= \alpha \cdot e^{-\alpha k}\\
\frac{p^\prime}{p} &= \frac{k}{1-e^{-\alpha k}}z_k^\prime + \ln(1-e^{-\alpha k})\\
&= \alpha k \frac{e^{-\alpha k}}{1-e^{-\alpha k}} + \ln(1-e^{-\alpha k})\\
\frac{dP(k)}{dk} &= (1-e^{-\alpha k})^k \cdot \left[ \alpha k \frac{e^{-\alpha k}}{1-e^{-\alpha k}} + \ln(1-e^{-\alpha k}) \right]
\end{split}
$$

let $$d_k^\prime = 0$$, denote $$e^{-\alpha k} = x$$, then $$\alpha k = -\ln x$$, we get either the first factor equals 0, or the second equals 0. The first factor equals 0 will lead to $$k = 0$$, which is a trivial solution, hence we mainly focus on the second factor:

$$
\begin{split}
0 &= \left[ \alpha k \frac{e^{-\alpha k}}{1-e^{-\alpha k}} + \ln(1-e^{-\alpha k}) \right]\\
&= -\ln x \frac{x}{1-x} + \ln(1-x)\\
x\ln x &= (1-x)\ln(1-x)\\
x &= 1-x\\
x &= \frac{1}{2} = e^{-\alpha k}\\
k &= \alpha^{-1}\ln2
\end{split}
$$

Then the probability will be $$P(k=\alpha^{-1}\ln 2) = 0.619^{\alpha^{-1}}$$. The number of $$\alpha^{-1}=\frac{m}{n}$$ can be thought of as the number of bits allocated per element in the Bloom filter on average, e.g, 32bits, which is greater than $$k=\ln2 \alpha^{-1} = 0.693*\alpha^{-1} < \alpha^{-1}$$, e.g. 22, the number of bits set to 1 by k hash functions.


Now we know the optimal number $$k$$ of hash functions should be $$k=\ln2 \alpha^{-1} = \ln 2 \frac{m}{n}$$. Given a preset false positive probability $$p$$, , what's the optimal value for $$m$$, $$n$$, respectively?

$$
\begin{split}
\ln p &= \ln((1-e^{-\alpha k})^k)\\
&= \ln((1-e^{-\alpha \alpha^{-1} \ln2})^{\ln2 \alpha^{-1}})\\
&= \ln(2^{-\ln2 \alpha^{-1}})\\
&= -\alpha^{-1}(\ln 2)^2\\
&= -\frac{m}{n}(\ln 2)^2\\
k &= \alpha^{-1}\ln2 = \frac{m}{n}\ln2 = -\frac{\ln p}{\ln 2} = -1.4427\ln p\\<
m &= -\frac{n\ln p}{(\ln 2)^2} = -2.0814n\ln p\\
n &= -\frac{m(\ln 2)^2}{\ln p}
\end{split}
$$

# Deletions

Deletion in Bloom filter might cause False Negatives, because 1s in Bloom filter might be shared by different elements, deleting a 1 for one element will affect other elements who also set the position as 1. A partial solution for this is
+ counting Bloom filter.
+ In counting Bloom filter, each position is not just a bit, but a small counter. All the small counters are set 0 at the beginning, when insert an element, the k hash functions find the positions, increment the corresponding counters by 1. When deleting an element, query this element at first, if all positions are greater than 0, means "probably yes", then decrement the corresponding positions by 1. The counter should be very small, otherwise it would undermine the space saving advantage that Bloom filters are designed to deliver. Too few bits for each counter might also cause problems, such as overflow.

# Bloom Filter Demo Algorithm in Python

Suppose we are going to store $$n=100,000$$ elements in this Bloom Filter, and requiring the False Positive Probability $$p <= 0.000001$$, then the number $k$ of hash functions should be $$k = -\frac{\ln p}{\ln 2} = -1.4427\ln p = 20$$, the size of the bit array should be $$m = -2.0814n\ln p = 2875560$$ bits, approximately $$351KB$$ in memory.

{% highlight python linenos %}
from bitarray import bitarray
import mmh3

class BloomFilter:
  def __init__(self, size, hash_funcs_count):
    self.m = size
    self.k = hash_funcs_count
    self.store = bitarray(size)
    self.store.setall(0)

  def insert(self, text):
    for seed in xrange(self.k):
      result = mmh3.hash(text,seed) % self.m
      self.store[result] = 1

  def query(self, text):
    for seed in xrange(self.k):
      result = mmh3.hash(text, seed) % self.m
      if self.store[result] == 0:
        return "Definitely Not!"
    return "Maybe!"

"""
    Suppose we are going to store n=100000 elements in BloomFilter
    Hoping False Positive Probability be p<10^-5
    Then $m=-\\frac{n \\ln p}{(\\ln 2)^2}= -2.0814 n \\ln p = 2875560 bits = 351 KB$
    function number $k = -1.4427 \\ln p = 20 $
"""

bf = BloomFilter(2876000,20)
bf.insert("test")
bf.insert("insert")
bf.insert("is")
bf.insert("there")
bf.insert("any")
bf.insert("false")
bf.insert("positive")
bf.insert("in")
bf.insert("this")
bf.insert("Bloom")
bf.insert("filter")
bf.insert("Notes")

print "bloom: " + bf.query("bloom")
print "Filter: " + bf.query("Filter")
print "Mutu: " + bf.query("Mutu")
print "X: " + bf.query("X")
print "1: " + bf.query("1")
print "Bloom: " + bf.query("Bloom")
print "filter: " + bf.query("filter")
print "Notes: " + bf.query("Notes")
{% endhighlight %}

# Reference
+ https://en.wikipedia.org/wiki/Differentiation_rules
+ http://www.cs.toronto.edu/~vassos/teaching/b63/handouts/bloom.pdf
+ https://en.wikipedia.org/wiki/Bloom_filter
+ http://www.maxburstein.com/blog/creating-a-simple-bloom-filter/
