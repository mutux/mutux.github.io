---
title: Sublime Text 3 As a Simple Python IDE
layout: post
---

**Install Package Control**
- Copy the Python code for Sublime Text 3 from [here](https://packagecontrol.io/installation#st3), click ``View->Show Console`` to open the ST3 console, Paste the code into the console, press Enter. Reboot ST3.
{% highlight python linenos %}
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
{% endhighlight %}
- Press ``CMD + Shift + P``, and typing ``Install Package``, and Press Enter.
- Click ``Sublime Text -> Preferences -> Settings-User``, to add

>{% highlight json %}
{
    "auto_complete": false,
    "auto_complete_commit_on_tab": true,
    "auto_match_enabled": true,
    "bold_folder_labels": true,
    "caret_style": "solid",
    "color_scheme": "Packages/Theme - Flatland/Flatland Dark.tmTheme",
    "detect_indentation": true,
    "draw_indent_guides": true,
    "ensure_newline_at_eof_on_save": true,
    "file_exclude_patterns":
    [
        "*.DS_Store",
        "*.pyc",
        "*.git"
    ],
    "find_selected_text": true,
    "fold_buttons": false,
    "folder_exclude_patterns":
    [
    ],
    "font_face": "Menlo",
    "font_options":
    [
        "no_round"
    ],
    "font_size": 13,
    "highlight_line": true,
    "highlight_modified_tabs": true,
    "ignored_packages":
    [
        "Vintage"
    ],
    "indent_to_bracket": true,
    "line_padding_bottom": 0,
    "line_padding_top": 0,
    "match_brackets": true,
    "match_brackets_angle": false,
    "match_brackets_braces": true,
    "match_brackets_content": true,
    "match_brackets_square": true,
    "new_window_settings":
    {
        "hide_open_files": true,
        "show_tabs": true,
        "side_bar_visible": true,
        "status_bar_visible": true
    },
    "remember_open_files": true,
    "remember_open_folders": true,
    "save_on_focus_lost": true,
    "scroll_past_end": false,
    "show_full_path": true,
    "show_minimap": false,
    "tab_size": 2,
    "theme": "Flatland Dark.sublime-theme",
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true,
    "use_simple_full_screen": true,
    "vintage_start_in_command_mode": false,
    "wide_caret": true,
    "word_wrap": true
}
{% endhighlight %}
- Click ``Sublime Text -> Preferences -> Settings -> Syntax Specific -User``, then add the following
{% highlight json linenos %}
{
    // editor options
    "draw_white_space": "all",

    // tabs and whitespace
    "auto_indent": true,
    "rulers": [79],
    "smart_indent": true,
    "tab_size": 4,
    "trim_automatic_white_space": true,
    "use_tab_stops": true,
    "word_wrap": true,
    "wrap_width": 80
}
{% endhighlight %}
Then save the file as ``Python.sublime-settings``
- Install Package ``Soda Dark Theme`` and ``Flatland``
- Update the settings through ``Sublime Text -> Preferences -> Settings User``:
{% highlight json linenos %}
{
  "theme": "Flatland Dark.sublime-theme",
  "color_scheme": "Packages/Theme - Flatland/Flatland Dark.tmTheme"
}
{% endhighlight %}
- Install SideBarEnhancements using Package Control
- Install Anacoda, the IDE-like features for Python, and set Anacoda.sublime-settings ``Sublime Text -> Preferences -> Package Settings -> Anaconda --> Settings-User`` as:
{% highlight json linenos %}
{
  "anaconda_linting": false
}
{% endhighlight %}
- Install Python environment with MiniConda
  - Download [Miniconda](https://repo.anaconda.com/miniconda/Miniconda2-latest-MacOSX-x86_64.sh)
  - Install ``sh Miniconda2-latest-MacOSX-x86_64.sh``
  - Create & User python Environment version 2.7
    - ``conda create --name python27 python=2.7``
    - ``source activate python27``
    - ``python --version``
    - ``(deactivate)``
  - Install Syntax Checker ``pyflakes`` and ``pycodestyle``
    -  ``pip install pyflakes``
    -  ``pip install pycodestyle``
  - Use ``which python`` to get the path of python for future use.
- Install Syntax Checker ``SublimeLinter-pyflakes`` and ``SublimeLinter-pycodestyle`` and ``SublimeLinter-json`` using Package Control
- Customize the linter settings ``Sublime Text -> Preferences -> Package Settings -> SublimeLinter -> Settings-User``, add the follow code:
{% highlight json linenos %}
// SublimeLinter Settings - User
{
	"debug": false,
	"linters":{
		"pycodestyle": {
	  	"disable": false,
	  	"args": [],
	  	"excludes": [],
	  	"ignore": "E501,C0301,W0142,W0402,R0201,E1101,E1102,C0103,R0901,R0903,R0904,C1001,W0223,W0232,W0201,E1103,R0801,C0111",
	  	"max-line-length": 80,
	  	"select": "" 
		}
	},
	"paths": {
        "linux": [],
        "osx": ["~/miniconda2/envs/python27/bin"],
        "windows": []
  },
}
{% endhighlight %}
- Update the settings for Anacoda Plugin to use Python27 as:
{% highlight json linenos %}
{
	"anaconda_linting": false,
	"python_interpreter": "~/miniconda2/envs/python27/bin/python"
}
{% endhighlight %}
- Install ``TerminalView`` with Package Control
  - Set the key binding:
  {% highlight json linenos %}
  [
	{ "keys": ["ctrl+shift+t"], "command": "terminal_view_open" }
  ]
  {% endhighlight %}
- Install ``SFTP`` with Package Control
  - Right Click a folder name on the sidebar, select ``SFTP/FTP -> Add Remote Mapping...``, to add a mapping file named ``sftp-config.json`` in the folder.
  - Edit fields accordingly, e.g.:
  {% highlight json linenos %}
  {
    // sftp, ftp or ftps
    "type": "sftp",

    "save_before_upload": true,
    "upload_on_save": true,
    "sync_down_on_open": true,
    "sync_skip_deletes": false,
    "sync_same_age": true,
    "confirm_downloads": false,
    "confirm_sync": true,
    "confirm_overwrite_newer": false,
    
    "host": "aaaaaaaaa",
    "user": "xxxxx",
    "password": "yyyyyy",
    "port": "22",
    "remote_path": "/u/pandu/pyspace/health/analyzer/"
  }
  {% endhighlight %}
  Once the SFTP is set, one can edit locally and upload to remote server automatically, and debug on the remote server.
- Modify the font size of ST3 by:
{% highlight json linenos %}
{
	"auto_complete": false,
	"color_scheme": "Packages/Theme - Flatland/Flatland Dark.tmTheme",
	"font_size": 11,
	"ignored_packages":
	[
		"Vintage"
	],
	"sublimelinter": false,
	"tab_size": 2,
	"theme": "Flatland Dark.sublime-theme",
	"ui_scale": 1.3,
	"word_wrap": true
}
{% endhighlight %}
**References**
- https://realpython.com/setting-up-sublime-text-3-for-full-stack-python-development/
- https://conda.io/docs/user-guide/getting-started.html#managing-python
- https://conda.io/miniconda.html
