---
title: ubuntu下sublime中文输入问题
categories: Note
---
# ubuntu下安装的sublime text中文不能输入问题：

## a.保存下面的代码到文件sublime_imfix.c(位于~目录)

\#include"gtk/gtkimcontext.h"
void gtk_im_context_set_client_window (GtkIMContext *context, GdkWindow  *window)
{
GtkIMContextClass *klass;
g_return_if_fail (GTK_IS_IM_CONTEXT (context));
klass = GTK_IM_CONTEXT_GET_CLASS (context);
if (klass->set_client_window)
klass->set_client_window (context, window);
g_object_set_data(G_OBJECT(context),"window",window);
if(!GDK_IS_WINDOW (window))
return;
int width = gdk_window_get_width(window);
int height = gdk_window_get_height(window);
if(width != 0 && height !=0)
gtk_im_context_focus_in(context);
}

## b.将上一步的代码编译成共享库libsublime-imfix.so，命令
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
    注意：如果提示 gtk/gtkimcontext.h：没有那个文件或目录，那就是没有相关的依赖软件，安装命令：

sudo apt-get install build-essential libgtk2.0-dev

## c.将libsublime-imfix.so拷贝到sublime_text所在文件夹
sudo mv libsublime-imfix.so /opt/sublime_text/
## d.修改文件/usr/bin/subl的内容
sudo gedit /usr/bin/subl
将

\#!/bin/sh

exec /opt/sublime_text/sublime_text "$@"

修改为

\#!/bin/sh

LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text "$@"

原文链接：http://www.jianshu.com/p/1f3a3e4f4e92
