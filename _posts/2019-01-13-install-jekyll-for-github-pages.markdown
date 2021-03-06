---
title: Install Jekyll for github pages
layout: post
comment_id: 7
---

**Install Ruby Environment**
1. ``brew install rbenv``
2. Add ``"eval $(rbenv init -)"`` to the end of ``~/.bash_profile``
3. Install a ruby version ``rbenv install 2.3.0``
4. Select a ruby version by rbenv `rbenv global 2.3.0`
5. Open a new terminal window
6. Verify that the right gem folder is being used with ``gem env home`` (should report something your user folder not system wide)

**Install Bundler**
1. Select a ruby version locally for the folder you are located in e.g. ``rbenv local 2.3.0``
2. Verify that the right gem folder is being used with ``gem env home``
3. Run ``gem insall bundler``
4. Run ``bunlde install``

**Install Jekyll**
1. Run ``gem install jekyll``

**Create a new Jekyll site at ``./myblog``**
1. Run ``jekyll new myblog``
2. Change to the folder ``cd myblog``
3. Run ``bundle exec jekyll serve``
4. Browse to [http://localhost:4000](http://localhost:4000)

**Connect Github**
1. ``git init``
2. ``git remote add origin https://github.com/username-or-organization-name/your-remote-repository-name``
3. ``git add .``
4. ``git commit -m "update site"``
5. ``git push -u origin master``

**Use an old site folder ``./oldblog``**
1. Change to ``cd oldblog``
2. Build the site and make it available on a local server ``bundle exec jekyll serve``
3. Browse to [http://localhost:4000](http://localhost:4000)

**Install Sublime Text 3 Jekyll Editor Plugin**
1. Press ``CMD + Shift + P``
2. Select ``Install Package``
3. Search ``Jekyll`` and install the latest version
4. Change settings ``Preferences->Package Settings->Jekyll->Settings-User`` to add ``"jekyll_posts_path": "/xxx/newblog/_posts"``

**Write a New Post**
1. Select the Folder ``_posts``
2. Press ``CMD + Shift + P``
3. Search ``Jekyll New Post``, and press Enter
4. Input the File Name, e.g., "The First Blog", and press Enter
5. The Plugin will rename the file following the rules automatically.
6. Edit the file, and use ``git`` command to submit or update.

**Add a Comment Section**
1. Create an issue under your github page project, remember the issue id.
2. Add ``comment_id: ##`` in the head area.

**Add support of Mathjax via secure https connections**
- ``gem install kramdown``
- Change the markdown line in _config.yml to ``markdown: kramdown``
- Create a local js file named ``mathjax-local.js`` with script like this:
{% highlight Javascript %}
window.MathJax = {
     displayAlign: "center",
     displayIndent: "0em",
     "HTML-CSS": { scale: 100,
                   linebreaks: { automatic: "false" },
                   webFont: "TeX"
     },
     SVG: {scale: 100,
           linebreaks: { automatic: "false" },
           font: "TeX"},
     NativeMML: {scale: 100},
     TeX: { equationNumbers: {autoNumber: "AMS"},
            MultLineWidth: "85%",
            TagSide: "right",
            TagIndent: ".8em"
     }
 };
{% endhighlight %}
- Add the following script to ``_layouts/default.html``, or to ``_includes/header.html``:
{% highlight python %}
<script type="text/javascript" src="/mathjax-local.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML"></script>
{% endhighlight %}
- Mark any mathematics in your posts within a pair of ``$$``s.
- A test: $$x^2$$.

**References:**
- https://github.com/rbenv/rbenv/issues/938
- https://jekyllrb.com/docs/installation/macos/
- https://jekyllrb.com/docs/
- https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/
- https://jekyllrb.com/docs/posts/

