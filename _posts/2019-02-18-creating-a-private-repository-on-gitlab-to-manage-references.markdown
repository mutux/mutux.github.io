---
title: creating a private repository on gitlab to manage references
layout: post
---

- Create the repository on Gitlab
- Set your SSH keys in your Profile
	- Generate SSH key or use an existing key to 
	  - ``ssh-keygen -o -t rsa -b 4096``
	- Copy your public key into clipboard 
	  - ``cat ~/.ssh/id_rsa.pub | clip`` or ``pbcopy ~/.ssh/id_rsa.pub``
	- Paste your public key into your Gitlab account
	  - clik your avatar, and select **Settings**
	  - navigate to **SSH Keys**, and paste your public keys in the **Key** section
	  - comment your key like 'my lab machine'
	  - click **Add key**
- Clone the repository to your local directory
  - ``cd */path/to/your/local/repository/*``
  - ``git clone git@gitlab.com:YourName/YourRepository.git``
- Config locally your repository (without **\-\-global** option)
  - ``git config user.email "user@email.com"``
  - ``git config user.name "username"``
- Always pull your repository before you manipulate it
	- ``git pull``
- Use ``.gitignore`` file to ignore some local file in submission
  {% highlight python %}
	*.log
	.DS_Store
	.gitignore
	*.so
	*.$py.class
	{% endhighlight %}
- Manipulate your local repository, add, commit, and push.
  - ``git add .``
  - ``git commit -m 'message or comment'``
  - ``git push [origin master]``
- Enjoy!
