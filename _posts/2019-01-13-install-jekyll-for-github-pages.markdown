---
title: Install Jekyll for github pages
layout: post
---

# Installation Guide

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
5. Open the folder ``/xxx/newblog/_posts/`` and right click to add a new file by inputing the file name.

**References:**
- https://github.com/rbenv/rbenv/issues/938
- https://jekyllrb.com/docs/installation/macos/
- https://jekyllrb.com/docs/
- https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/
- https://jekyllrb.com/docs/posts/

