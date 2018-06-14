插件开发语言: Python

1. 通过Preferences -> Browse Packages...打开Packages文件夹，在该文件夹下建立个子文件夹，名字为你想开发的插件名字，如：saveimage

2. 通过Tools -> New Plugin...来打开一个初始化的插件编辑文件
```
import sublime
import sublime_plugin
import sys
import os


# class ExampleCommand(sublime_plugin.TextCommand):
#   def run(self, edit):
#       self.view.insert(edit, 0, "Hello, World!")

# 1. create dir 'image' in current *.md file's path
# 2. use clipper tools clip picture and save *.png in image path
# 3. view.run_command('saveimage') in *.md file position    (shortcutkey: )
class saveimageCommand(sublime_plugin.TextCommand):
    flist=[]
    path = ""
    def on_done(self, id):
        if(id < 0):
            return
        print(self.flist[id])   

    def run(self, edit):
        # self.view.insert(edit, 0, "")
        full = self.view.file_name()
        print("full", full)
        [p,f] = os.path.split(full)
        print("p", p)
        print("f", f)
        # 处理剪贴板

        clipf = sublime.get_clipboard()
        print("clipf", clipf)
        for root,dirs,files in os.walk(os.path.join(p,"image")):
            print("root", root)
            print("dirs", dirs)
            print("files", files)
            mtime=0
            lastfile=""
            for f1 in files:
                (main,ext) = os.path.splitext(f1)
                print("ext",ext)
                if ext and ext.lower() not in [".jpg",".png",".gif",".bmp",".jpeg"]:
                    continue
                mtime1 = os.path.getmtime(os.path.join(root,f1))
                if mtime < mtime1:
                    lastfile = os.path.join("image", f1)
                    mtime=mtime1
            self.flist = files
            self.path = root
        for regoin in self.view.sel():
            self.view.insert(edit,regoin.begin(),"![](%s)"%lastfile)
        # self.view.show_popup_menu(self.flist, self.on_done)

```

3.  ctrl+s 保存这个文件，并放到你建立的子文件夹下，文件名如：SaveImage.py

4.  在插件文件夹 saveimage 下创建文件 Default (Windows).sublime-keymap,并编辑

```
[
    { "keys": ["alt+p"], "command": "saveimage"}
]
```