---
layout: post
title: 'String algorithm: Suffix Tries'
date: '2017-05-07T11:35:00.006-07:00'
author: mutux
tags:
modified_time: '2017-05-07T11:35:56.825-07:00'
blogger_id: tag:blogger.com,1999:blog-7424095106938843032.post-1209276707978389247
blogger_orig_url: http://mutux.blogspot.com/2017/05/string-algorithm-suffix-tries.html
comment_id: 4
---

# Trie
A trie is a tree representing a collection of strings with one node per common prefix. It is a natural tool to manage either a set or a map where keys are strings. In a trie tree:

+ Each edge is labeled with one and only one character;
+ Outgoing edges of a same node should be labeled differently from each other;
+ Each string (key) in the collection is “spelled out” along some path starting from the root.

Query time complexity is $$O(m)$$, where $$m$$ is the length of the query string.
Space complexity (number of nodes) is $$O(N)$$, where $$N$$ is the total length of all the strings in the collection.

**Data Structure**

{% highlight python linenos %}
class TrieMap(object):
  """ Trie implementation of a map. Associating Keys (strings or other
    sequence type) with values. Values can be any type. """
  def __init__(self, kvs):
    # A node is a dictionary storing the indiced character as the key,
    # and a dictionary as the value. The dictionary includes all
    #the nodes(dictionaries) that connected to it.
    self.root = {}
    # For each key (string)/value pair
    for (k,v) in kvs: self.add(k,v)

  def add(self, k, v):
    """ Add a key-value pair """
    cur = self.root
    for c in k: # for each character in the string
      if c not in cur:
        cur[c] = {} # add a new node (dictionary)
      cur = cur[c] # move cur forward to node with key c
    cur['value'] = v # add the value in the end

  def query(self, k):
    """Given key, return associated value or None """
    cur = self.root
    for c in k:
      if c not in cur:
        return None #key wasn't in the trie
      cur = cur[c]
    # get value, or None if there's no value associated with this node
    return cur.get('value')

if __name__ == "__main__":
  kvs = [('acdeast',1000),('bcde',2000),('acdb',3000),('bcas',4000),('case',5000),('ba',6000),('ae',7000)]

  m = TrieMap(kvs)
  print m.query('case')
{% endhighlight %}

# Suffix Trie

A suffix trie is a space-efficient data structure to store **one** string that allows many kinds of queries to be answered quickly. Every path from the root to a leaf node represents a suffix of the string. Each suffix of the string is represented by some path from the root to a leaf node. Special terminal character like '\$' has to be appended to the original string in order to discriminate all the suffixes of it.

**Processing Strings using Suffix Tries**

Given a suffix trie $$T$$, and a string $$q$$,  we can:
+ Determine whether $$q$$ is a substring of $$T$$?
  + Follow the path for $$q$$ starting from the root, if we exhaust $$q$$ without falling off, then $$q$$ is a substring of $$T$$, or it's not.
+ Check whether $$q$$ is a suffix of $$T$$?
  + Same procedure as checking substring, but additionally check whether the final node is has an outgoing edge '\$', i.e., next node is a leaf node.
+ Count how many times $$q$$ appears in $$T$$?
  + Follow path according to $$q$$, if we fall off the trie, then the answer is 0, or if we end up at node $$v$$, then the answer is the number of leaves of the subtree rooted at $$v$$.
+ Find the longest common substring of $$T$$ and $$q$$?
  + Follow path according to $$q$$, if we fall off after node $$v$$, then the answer is the substring from the root to $$v$$, or if we exhaust $$q$$, then the answer is $$q$$.
+ Find the longest repeated substring of $$T$$?
  + Find the deepest node with at least 2 children.

**Main Idea**

>Every substring of T is a prefix of some suffix of T.

**Data Structure**

{% highlight python linenos %}
class SuffixTrie(object):

  def __init__(self, t):
    t += '$' #add terminator
    self.root = {}
    for i in xrange(len(t)):
      cur = self.root
      for c in t[i:]:
        if c not in cur:
          cur[c] = {} # adding new outgoing edge
        cur = cur[c]

  def followPath(self,s):
    cur = self.root
    for c in s:
      if c not in cur:
        return None
      cur = cur[c]
    return cur

  def hasSubstring(self,s):
    return self.followPath(s) is not None

  def hasSuffix(self,s):
    node = self.followPath(s)
    return node is not None and '$' in node

if __name__ == "__main__":
  st = SuffixTrie("abcefdgacematabce")
  a = "efdg"
  b = "efg"
  c = "atabce"
  d = "abc"
  print "hasSubstring - " + a + ": " + str(st.hasSubstring(a))
  print "hasSubstring - " + b + ": " + str(st.hasSubstring(b))
  print "hasSuffix - " + c + ": " + str(st.hasSuffix(c))
  print "hasSuffix - " + d + ": " + str(st.hasSuffix(d))
{% endhighlight %}

The worst case of space complexity is $$O(n^2)$$, and the best case is $$O(n)$$, the complexity is in between at most cases, where $$n$$ is the length of the string $$T$$.

# Suffix Tries with Suffix Links

Now we know a basic suffix trie of a string $$T$$, each suffix of $$T$$ can be found by starting from the root node. Can we also easily find out the suffixes of $$t$$ in the suffix trie ($$t$$ is a substring of $$T$$)? That's where suffix links can play their role.

**Suffix Links** connect node representing "$$x\alpha$$" to a node representing "$$\alpha$$". Most important suffix links are the ones connecting suffixes of the full string (substring). Every node has a suffix link. Each node represent the prefix of some suffix. *The node's suffix link should link to the prefix of the suffix s that is 1 character shorter.*

**Application of Suffix Link**: find the longest common substring of $$T$$ and $$q$$

+ Walk down the tree following q.
  + If you hit a dead end, save the current depth,
  + Follow the suffix link from the current node.
  + When you exhaust q, return the longest substring found.

**Constructing Suffix Tries**

We may construct a suffix trie in an incremental way. If we have construct a suffix trie for $$s[0,...,i-1]$$, then we my get suffix trie of $$s[0,...,i]$$ by adding character $$s[i]$$ to all the suffixes of $$s[0,...,i-1]$$. This can be accomplished by adding s[i] to the trie following the suffix links from the current deepest node the root node. To build $$SuffixTrie(s[0...i])$$ from $$SuffixTrie(s[0...i-1])$$, the algorithm could be:

{% highlight python linenos %}
cur = deepest()
repeat:
    Add child edge s[i] to cur.
    Follow suffix link to set cur to the next suffix
    Add suffix links connecting node you just added in the order in which they were added.
{% endhighlight %}

# Algorithm in Python

The main idea is to construct the suffix trie incrementally, the incremental step is as demonstrated above.
{% highlight python linenos %}
class SuffixNode:
  def __init__(self, suffix_link = None):
    self.children = {}
    if suffix_link is not None:
      self.suffix_link = suffix_link
    else:
      self.suffix_link = self

  def add_edge(self, edge, node): # (key,value): key is the edge label, value is the node to be appended
    self.children[edge] = node

# Construct suffix tries in an incremental way.
def suffix_tries(s):
  # Construct the first edge and the nodes.
  root = SuffixNode()
  deepest = SuffixNode(root)
  root.add_edge(s[0],deepest)
  # Adding each character in an incremental way: from suffix_tries(s[0...i-1]), construct suffix_trie(s[0...i])
  # start from the current deepest node to the root.
  for c in s[1:len(s)]:
    cur = deepest
    prev = None
    #for char c, travel start from the deepest node, along the suffix_links
    while c not in cur.children:
      tmp = SuffixNode()
      cur.add_edge(c,tmp)

      if prev is not None:
        prev.suffix_link = tmp

      prev = tmp
      cur = cur.suffix_link

    if cur is root:
      prev.suffix_link = root
    else:
      prev.suffix_link = cur.children[c]

    #update current deepest node, prepare for the next char
    deepest = deepest.children[c]

if __name__ == "__main__":
  suffix_tries("abacbade")
{% endhighlight %}

# Suffix Trees

For a string $$a^nb^n$$ like "aaabbb", the number of node could be $$O(n^2)$$, this is not space efficient. In order to make it more space-efficient, we need Suffix Trees to substitute Suffix Tries.
The algorithm is called [Ukkonen's Algorithm](https://github.com/mutux/Ukkonen-s-Suffix-Tree-Algorithm).

# Reference

+ www.cs.jhu.edu/~langmea/resources/lecture_notes/tries_and_suffix_tries.pdf
+ https://www.cs.cmu.edu/~ckingsf/bioinfo-lectures/suffixtrees.pdf
