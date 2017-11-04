---
layout: post
title: 'String Algorithms: KMP in python'
date: '2017-04-17T15:44:00.000-07:00'
author: mutux
tags:
modified_time: '2017-04-17T16:02:16.749-07:00'
blogger_id: tag:blogger.com,1999:blog-7424095106938843032.post-8692483318949907228
blogger_orig_url: http://mutux.blogspot.com/2017/04/string-agorothms-kmp-in-python.html
---

KMP is fantastic. There are some practical ideology in its design.

+ If you are traversing a list, try to make the step big enough.
+ If you are computing element one-by-one, and the computation of each element is related to others, try to think about a recursive formula.
+ If the sufficient condition or necessary condition are easy to acquire, then compare the condition with the problem definition, leverage the apparent conditions and focus on the difference between conditions and definition (sufficient and necessary conditions).

The problem of finding next start point when mismatch at some point i, can be solved in a recursive formula, using the necessary conditions of matching:
>If  $$(P_t, P_{t+1},…P_i)$$ is a suffix $$(P_1,…P_i)$$, then $$(P_t, P_{t+1},…,P_{i-1})$$ is the suffix of $$(P_1,…,P_{i-1})$$.

And it holds for the suffixes.

That means the later is the necessary condition of the former. It also means if we know the suffix of $$(P_1,…,P_{i-1})$$ is $$(P_t, P_{t+1},…, P_{i-1})$$, what we need to do is to find out if adding $$(P_i)$$ to suffix $$(P_t, P_{t+1},…, P_{i-1})$$ is a suffix of $$(P_1,…, P_i)$$, i.e., to find out if the character after prefix and character after suffix is the same. If yes, then its a suffix, if no, try the other suffixes.

Today, I finished the KMP algorithm:
{% highlight python linenos %}
def kmp(P,T):
    #Compute the start position (number of chars) of the longest suffix that matches a prefix,
    #and store them into list K, the first element of K is set to be -1, the second
    #
    K = [] #K[t] store the value that when mismatch happens at t, should move Pattern P K[t] characters ahead
    t = -1 #K's length is len(P) + 1, the first element is set to be -1, corresponding to no elements in P.
    K.append(t) # Add the first element, keep t = -1.
    for k in range(1,len(P)+1):
        # traverse all the elemtn in P, calculate the corresponding value for each element.
        while(t>=0 and P[t]!=P[k-1]):# if t=-1, then let t = 0, if t>=0 and current suffix doesn't match, then try a shorter suffix
            t = K[t]
        t = t+1 # If it matches, then the matching position should be one character ahead.
        K.append(t) #record the matching postion for k
    print(K)

    #Match the String T with P
    m = 0 # Record the current matching position in P when compared with T
    for i in range(0,len(T)): # traverse T one-by-one
        while (m>=0 and P[m]!=T[i]): # if mismatch happens at position m, move P forward with K[m] characters and restart comparison
            m = K[m]
        m = m + 1 # if position m matches, move P forward to next position
        if m == len(P): # if m is already the end of K (or P), the a fully match is found. Continue comparison by move P forward K[m] characters
            print (i-m+1,i)
            m = K[m]


if __name__ == "__main__":
    kmp('abcbabca','abcbabcabcbabcbabcbabcabcbabcbabca')

    kmp('abab','ababcabababc')

"""
Results:
========================================================
[-1, 0, 0, 0, 0, 1, 2, 3, 1]
0 7
15 22
26 33
[-1, 0, 0, 1, 2]
0 3
5 8
7 10
"""
{% endhighlight %}

# References

+ https://web.stanford.edu/class/cs97si/10-string-algorithms.pdf
+ https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm

