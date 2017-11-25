---
layout: post
title: Install TensorFlow in Virtualenv without sudo privilege
date: '2017-10-17T06:14:00.003-07:00'
author: mutux
tags:
modified_time: '2017-10-17T06:15:23.331-07:00'
blogger_id: tag:blogger.com,1999:blog-7424095106938843032.post-6919649362070342145
blogger_orig_url: http://mutux.blogspot.com/2017/10/install-tensorflow-in-virtualenv.html
comment_id: 5
---

## Install Virtualenv without sudo privilege
 - Download and unzip one of the latest pacages:
{% highlight python linenos %}
wget https://github.com/pypa/virtualenv/archive/15.1.0.tar.gz
tar -zxvf 15.1.0.tar.gz
{% endhighlight %}

 - Run command below to create an environment:
{% highlight python linenos %}
cd 15.1.0
python virtualenv.py ~/tfenv
{% endhighlight %}

 - Activate the virtualenv environment using source command:
{% highlight python linenos %}
source ~/tfenv/bin/activate
{% endhighlight %}
It will change you prompt to: `(tfenv)$`

 - Install pip in your the environment:
{% highlight python linenos %}
(tfenv)$ easy_install -U pip
{% endhighlight %}

 - Install tensorflow using pip:
{% highlight python linenos %}
(tfenv)$ pip install --upgrade tensorflow
{% endhighlight %}

## Uninstallation

 - Exit virtualenv:
{% highlight python linenos %}
(tfenv)$ deactivate
{% endhighlight %}

 - Remove the directory:
{% highlight python linenos %}
rm -rf ~/tfenv
{% endhighlight %}

## Enter & Quit the environment

 - Entering the virtualenv:
{% highlight python linenos %}
source ~/tfenv/bin/activate
{% endhighlight %}

 - Quitting the virtualenv:
{% highlight python linenos %}
(tfenv)$ deactivate
{% endhighlight %}

## Validate the installation

 - Invoke python
{% highlight python linenos %}
(tfenv)$ python
{% endhighlight %}

 - Enter the following program, the system should output "Hello, TensorFlow!":
{% highlight python linenos %}
>>> import tensorflow as tf:
>>> hello = tf.constant("Hello, TensorFlow!")
>>> sess = tf.Session()
>>> print sess.run(hello)
{% endhighlight %}


If anything goes wrong, please comment below, I'll response when I see it.
Have fun!
