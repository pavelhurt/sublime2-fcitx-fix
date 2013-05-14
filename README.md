通过启动sublime text 2时指定LD_PRELOAD来修复ubuntu 13.04中sublime text 2不支持fcitx中文输入法的问题。

# 1.安装C/C++的编译环境和gtk libgtk2.0-dev

    sudo apt-get install build-essential
    sudo apt-get install libgtk2.0-dev

# 2.编译共享内库

    gcc -shared -o libsublime-imfix.so sublime-imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC

# 3.将libsublime-imfix.so移动到/usr/lib/

    sudo mv libsublime-imfix.so /usr/lib/

# 4.启动 Sublime Text 2

    LD_PRELOAD=/usr/lib/libsublime-imfix.so sublime-text

但是这样的话，我们每次都要在终端里面使用命令启动sublime text 2，这样很不方便，接下来我们还要通过修改sublime-text-2命令达到使用subl、sublime-text、sublime-text-2等命令启动以及修改sublime-text-2.desktop达到点击图标启动。

# 1.打开sublime-text-2

    sudo gedit /usr/bin/sublime-text-2

# 2.修改sublime-text-2为以下内容

    #!/bin/bash

    LD_PRELOAD=/usr/lib/libsublime-imfix.so /usr/lib/sublime-text-2/sublime_text --class=sublime-text-2 "$@"

这样就能使用subl、sublime-text、sublime-text-2等命令启动

# 3.打开sublime-text-2.desktop

    sudo gedit /usr/share/applications/sublime-text-2.desktop

# 4.修改sublime-text-2.desktop

将

    Exec=/usr/bin/sublime-text-2 %F

修改为

    Exec=sublime-text-2 %F

还有将

    Exec=/usr/bin/sublime-text-2 --new-window

修改为

    Exec=sublime-text-2 --new-window

好了，接下来你在dash中点击打开sublime text2吧，开始你的代码之旅吧。

[参考文章 作者：Wuu]: http://my.oschina.net/wugaoxing/blog/121281  "Ubuntu系统下Sublime Text 2中fcitx中文输入法的解决方法"