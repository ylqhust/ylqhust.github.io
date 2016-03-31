---
layout: post
title: ADB模拟用户输入
date: 2016-4-1
categories: blog
tags: [ADB,模拟用户输入,Android]
description: 如何使用ADB模拟用户输入
---
# Adb模拟用户输入  
## 1.目的  
通过代码模拟用户的行为，这里主要模拟用户的点击行为，长按行为，滑动行为  

## 2.实现方法  
### a.电脑上的实现方式  
使用adb的input命令向手机发送指令  
全部命令如下  

	  text <string> (Default: touchscreen)
      keyevent [--longpress] <key code number or name> ... (Default: keyboard)
      tap <x> <y> (Default: touchscreen)
      swipe <x1> <y1> <x2> <y2> [duration(ms)] (Default: touchscreen)
      press (Default: trackball)
      roll <dx> <dy> (Default: trackball)
      tmode <tmode>
1.输入文字  
input text "string"  
向android设备发送文本，文本是发送到当前获取焦点的EditText中，只能发送ASCII码，无法发送汉字  

2.按键  
input keyevent keyCode  
模拟按下一个值为keyCode的键，还可以直接使用这个键的名字  
input keyevent keyName  
加上--longpress代表长按一个键  
input keyevent --longpress kevCode

3.点击屏幕  
input tap x y  
模拟点击屏幕上坐标为(x,y)的地方，屏幕坐标可以通过打开手机中的开发者选项，来观察  

4.滑动  
input swipe x1 y1 x2 y2 millSecond  
从(x1,y1)滑到(x2,y2)，经过的时间为millSecond，以毫秒为单位,millSecond是一个可选的值，如果没有的话，滑动将会马上完成。  

5.长按屏幕上的某一点  
input swipe x1 y1 x1 y1 millSecond  
长按就是一种变相的滑动，只要将两个点的坐标设置为相同，将时间设置长点就可以实现长按  

### b.手机上使用代码实现  

	public class RootShellCmd {

	private OutputStream os;

	/**
	 * 执行shell指令
	 * 
	 * @param cmd
	 *            指令
	 */
	public final void exec(String cmd) {
		try {
			if (os == null) {
				os = Runtime.getRuntime().exec("su").getOutputStream();
			}
			os.write(cmd.getBytes());
			os.flush();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	/**
	 * 后台模拟全局按键
	 * 
	 * @param keyCode
	 *            键值
	 */
	public final void simulateKey(int keyCode) {
		exec("input keyevent " + keyCode + "\n");
	}
	}

<p><span STYLE="FonT-FAMiLY: Microsoft YaHei; FonT-siZe: 14px"><br />
每个数字与keycode对应表如下：<br />
0 --&gt; "KEYCODE_UNKNOWN"<br />
1 --&gt; "KEYCODE_MENU"<br />
2 --&gt; "KEYCODE_SOFT_RIGHT"<br />
3 --&gt; "KEYCODE_HOME"<br />
4 --&gt; "KEYCODE_BACK"<br />
5 --&gt; "KEYCODE_CALL"<br />
6 --&gt; "KEYCODE_ENDCALL"<br />
7 --&gt; "KEYCODE_0"<br />
8 --&gt; "KEYCODE_1"<br />
9 --&gt; "KEYCODE_2"<br />
10 --&gt; "KEYCODE_3"<br />
11 --&gt; "KEYCODE_4"<br />
12 --&gt; "KEYCODE_5"<br />
13 --&gt; "KEYCODE_6"<br />
14 --&gt; "KEYCODE_7"<br />
15 --&gt; "KEYCODE_8"<br />
16 --&gt; "KEYCODE_9"<br />
17 --&gt; "KEYCODE_STAR"<br />
18 --&gt; "KEYCODE_POUND"<br />
19 --&gt; "KEYCODE_DPAD_UP"<br />
20 --&gt; "KEYCODE_DPAD_DOWN"<br />
21 --&gt; "KEYCODE_DPAD_LEFT"<br />
22 --&gt; "KEYCODE_DPAD_RIGHT"<br />
23 --&gt; "KEYCODE_DPAD_CENTER"<br />
24 --&gt; "KEYCODE_VOLUME_UP"<br />
25 --&gt; "KEYCODE_VOLUME_DOWN"<br />
26 --&gt; "KEYCODE_POWER"<br />
27 --&gt; "KEYCODE_CAMERA"<br />
28 --&gt; "KEYCODE_CLEAR"<br />
29 --&gt; "KEYCODE_A"<br />
30 --&gt; "KEYCODE_B"<br />
31 --&gt; "KEYCODE_C"<br />
32 --&gt; "KEYCODE_D"<br />
33 --&gt; "KEYCODE_E"<br />
34 --&gt; "KEYCODE_F"<br />
35 --&gt; "KEYCODE_G"<br />
36 --&gt; "KEYCODE_H"<br />
37 --&gt; "KEYCODE_I"<br />
38 --&gt; "KEYCODE_J"<br />
39 --&gt; "KEYCODE_K"<br />
40 --&gt; "KEYCODE_L"<br />
41 --&gt; "KEYCODE_M"<br />
42 --&gt; "KEYCODE_N"<br />
43 --&gt; "KEYCODE_O"<br />
44 --&gt; "KEYCODE_P"<br />
45 --&gt; "KEYCODE_Q"<br />
46 --&gt; "KEYCODE_R"<br />
47 --&gt; "KEYCODE_S"<br />
48 --&gt; "KEYCODE_T"<br />
49 --&gt; "KEYCODE_U"<br />
50 --&gt; "KEYCODE_V"<br />
51 --&gt; "KEYCODE_W"<br />
52 --&gt; "KEYCODE_X"<br />
53 --&gt; "KEYCODE_Y"<br />
54 --&gt; "KEYCODE_Z"<br />
55 --&gt; "KEYCODE_COMMA"<br />
56 --&gt; "KEYCODE_PERIOD"<br />
57 --&gt; "KEYCODE_ALT_LEFT"<br />
58 --&gt; "KEYCODE_ALT_RIGHT"<br />
59 --&gt; "KEYCODE_SHIFT_LEFT"<br />
60 --&gt; "KEYCODE_SHIFT_RIGHT"<br />
61 --&gt; "KEYCODE_TAB"<br />
62 --&gt; "KEYCODE_SPACE"<br />
63 --&gt; "KEYCODE_SYM"<br />
64 --&gt; "KEYCODE_EXPLORER"<br />
65 --&gt; "KEYCODE_ENVELOPE"<br />
66 --&gt; "KEYCODE_ENTER"<br />
67 --&gt; "KEYCODE_DEL"<br />
68 --&gt; "KEYCODE_GRAVE"<br />
69 --&gt; "KEYCODE_MINUS"<br />
70 --&gt; "KEYCODE_EQUALS"<br />
71 --&gt; "KEYCODE_LEFT_BRACKET"<br />
72 --&gt; "KEYCODE_RIGHT_BRACKET"<br />
73 --&gt; "KEYCODE_BACKSLASH"<br />
74 --&gt; "KEYCODE_SEMICOLON"<br />
75 --&gt; "KEYCODE_APOSTROPHE"<br />
76 --&gt; "KEYCODE_SLASH"<br />
77 --&gt; "KEYCODE_AT"<br />
78 --&gt; "KEYCODE_NUM"<br />
79 --&gt; "KEYCODE_HEADSETHOOK"<br />
80 --&gt; "KEYCODE_FOCUS"<br />
81 --&gt; "KEYCODE_PLUS"<br />
82 --&gt; "KEYCODE_MENU"<br />
83 --&gt; "KEYCODE_NOTIFICATION"<br />
84 --&gt; "KEYCODE_SEARCH"<br />
85 --&gt; "TAG_LAST_KEYCODE"</SPAN></P>
<p>&nbsp;<wbr></P>
<p><span STYLE="FonT-FAMiLY: Microsoft YaHei; FonT-siZe: 14px"><span STYLE="FonT-FAMiLY: Microsoft YaHei; FonT-siZe: 14px"><br /></SPAN></SPAN></P>
<h1 STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px" ALIGN="center"><a NAME="t0"></A><span STYLE="Line-HeiGHT: 36px; WorD-WrAp: normal; WorD-BreAK: normal"><font FACE="Arial">KEYCODE列表</FONT></SPAN></H1>
<h1 STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t1"></A><a NAME="t2" TARGET="_blank"></A><a NAME="t0" TARGET="_blank"></A>电话键</H1>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="708" HEIGHT="344">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_CALL</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
拨号键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
5</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_ENDCALL</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
挂机键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
6</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_HOME</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键Home</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
3</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MENU</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<a TITLE="菜单" HREF="http://www.haogongju.net/tag/%E8%8F%9C%E5%8D%95" TARGET="_blank">菜单</A>键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
82</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BACK</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
返回键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
4</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_S<a TITLE="EA" HREF="http://www.haogongju.net/tag/EA" TARGET="_blank">EA</A>RCH</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<a TITLE="搜索" HREF="http://www.haogongju.net/tag/%E6%90%9C%E7%B4%A2" TARGET="_blank">搜索</A>键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
84</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_CAMERA</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
拍照键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
27</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_FOCUS</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
拍照对焦键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
80</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_POWER</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
电源键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
26</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NOTIFICATION</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通知键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
83</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MUTE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
话筒静音键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
91</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_VOLUME_MUTE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
扬声器静音键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
164</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_VOLUME_UP</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
音量增加键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
24</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_VOLUME_DOWN</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
音量减小键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>25</P>
<p>&nbsp;<wbr></P>
</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<h1 STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t2"></A><a NAME="t3" TARGET="_blank"></A><a NAME="t1" TARGET="_blank"></A></H1>
<table STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px; pADDinG-Top: 0px" WIDTH="762" HEIGHT="862">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_NUM</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Number modifier</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_INFO</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Info</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_<a TITLE="APP" HREF="http://www.haogongju.net/tag/App" TARGET="_blank">APP</A>_SW<a TITLE="IT" HREF="http://www.haogongju.net/tag/IT" TARGET="_blank">IT</A>CH</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键App switch</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_BOOKMARK</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Bookmark</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_AVR_INPUT</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键A/V Receiver input</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_AVR_POWER</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键A/V Receiver power</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_CAPTIONS</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Toggle captions</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_CHANNEL_DOWN</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Channel down</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_CHANNEL_UP</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Channel up</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_CLEAR</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Clear</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_DVR</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键DVR</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_ENVELOPE</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Envelope special function</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_E<a TITLE="XP" HREF="http://www.haogongju.net/tag/XP" TARGET="_blank">XP</A>LORER</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Explorer special function</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_FORWARD</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Forward</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_FORWARD_DEL</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Forward Delete</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_FUNCTION</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Function modifier</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_G<a TITLE="UI" HREF="http://www.haogongju.net/tag/UI" TARGET="_blank">UI</A>DE</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Guide</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_HEADSETHOOK</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Headset Hook</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_META_LEFT</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Left Meta modifier</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_META_RIGHT</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Right Meta modifier</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_PICTSYMBOLS</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Picture Symbols modifier</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_PROG_BL<a TITLE="UE" HREF="http://www.haogongju.net/tag/UE" TARGET="_blank">UE</A></P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Blue “programmable”</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_PROG_GREEN</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Green “programmable”</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_PROG_RED</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Red “programmable”</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_PROG_YELLOW</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Yellow “programmable”</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_SETTINGS</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Settings</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_SOFT_LEFT</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Soft Left</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_SOFT_RIGHT</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Soft Right</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_STB_INPUT</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Set-top-box input</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_STB_POWER</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Set-top-box power</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_SWITCH_CHARSET</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Switch Charset modifier</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_SYM</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Symbol modifier</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_SYSRQ</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键System Request / Print Screen</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_TV</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键TV</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_TV_INPUT</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键TV input</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_TV_POWER</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键TV power</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_WINDOW</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>按键Window</P>
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>KEYCODE_UNKNOWN</P>
</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<p>未知按键</P>
</TD>
</TR>
</TBODY>
</TABLE>
<p>&nbsp;<wbr></P>
<p>&nbsp;<wbr></P>
<p><a NAME="t4" TARGET="_blank"></A><a NAME="t2" TARGET="_blank"></A>控制键</P>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="711" HEIGHT="491">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_ENTER</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
回车键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
66</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_ESCAPE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
ESC键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
111</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_DPAD_CENTER</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
<a TITLE="导航" HREF="http://www.haogongju.net/tag/%E5%AF%BC%E8%88%AA" TARGET="_blank">导航</A>键 确定键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
23</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_DPAD_UP</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
导航键 向上</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
19</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_DPAD_DOWN</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
导航键 向下</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
20</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_DPAD_LEFT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
导航键 向左</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
21</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_DPAD_RIGHT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
导航键 向右</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
22</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MOVE_HOME</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
光标<a TITLE="移动" HREF="http://www.haogongju.net/tag/%E7%A7%BB%E5%8A%A8" TARGET="_blank">移动</A>到开始键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
122</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MOVE_END</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
光标移动到末尾键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
123</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_PAGE_UP</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
向上翻页键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
92</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_PAGE_DOWN</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
向下翻页键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
93</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_DEL</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
退格键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
67</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_FORWARD_DEL</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
删除键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
112</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_<a TITLE="IN" HREF="http://www.haogongju.net/tag/in" TARGET="_blank">IN</A>SERT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
插入键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
124</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_TAB</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Tab键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
61</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUM_LOCK</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘锁</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
143</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_CAPS_LOCK</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
大写锁定键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
115</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BREAK</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Break/Pause键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
121</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_SCROLL_LOCK</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
滚动锁定键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
116</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_ZOOM_IN</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
放大键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
168</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_ZOOM_OUT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
缩小键</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
169</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<p>&nbsp;<wbr></P>
<h1 STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t5"></A><a NAME="t6" TARGET="_blank"></A><a NAME="t4" TARGET="_blank"></A>组合键</H1>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="714" HEIGHT="152">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_ALT_LEFT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Alt+Left</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_ALT_RIGHT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Alt+Right</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_CTRL_LEFT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Control+Left</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_CTRL_RIGHT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Control+Right</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_SHIFT_LEFT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Shift+Left</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_SHIFT_RIGHT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Shift+Right</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<h1 STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t6"></A><a NAME="t7" TARGET="_blank"></A>&nbsp;<wbr></H1>
<p STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t8" TARGET="_blank"></A><a NAME="t6" TARGET="_blank"></A>基本</P>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="721" HEIGHT="806">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_0</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'0'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
7</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_1</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'1'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
8</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_2</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'2'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
9</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_3</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'3'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
10</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_4</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'4'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
11</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_5</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'5'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
12</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_6</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'6'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
13</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_7</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'7'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
14</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_8</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'8'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
15</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_9</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'9'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
16</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_A</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'A'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
29</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_B</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'B'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
30</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_C</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'C'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
31</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_D</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'D'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
32</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_E</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'E'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
33</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'F'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
34</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_G</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'G'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
35</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_H</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'H'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
36</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_I</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'I'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
37</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_J</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'J'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
38</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_K</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'K'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
39</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_L</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'L'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
40</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_M</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'M'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
41</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_N</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'N'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
42</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_O</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'O'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
43</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_P</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'P'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
44</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_Q</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'Q'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
45</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_R</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'R'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
46</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_S</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'S'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
47</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_T</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'T'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
48</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_U</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'U'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
49</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_V</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'V'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
50</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_W</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'W'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
51</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_X</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'X'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
52</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_Y</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'Y'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
53</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_Z</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'Z'</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
54</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<h1 STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t8"></A><a NAME="t9" TARGET="_blank"></A>&nbsp;<wbr></H1>
<p STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t10" TARGET="_blank"></A><a NAME="t8" TARGET="_blank"></A>符号</P>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="729" HEIGHT="370">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_PLUS</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'+'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MINUS</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'-'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_STAR</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'*'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_SLASH</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'/'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_EQUALS</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'='</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_AT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'@'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_POUND</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'#'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_AP<a TITLE="OS" HREF="http://www.haogongju.net/tag/OS" TARGET="_blank">OS</A>TROPHE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键''' (单引号)</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BACKSLASH</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'\'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_COMMA</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键','</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_PERIOD</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'.'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_LEFT_BRACKET</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'['</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_RIGHT_BRACKET</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键']'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_<a TITLE="SEM" HREF="http://www.haogongju.net/tag/SEM" TARGET="_blank">SEM</A>ICOLON</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键';'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_GRAVE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键'`'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_SPACE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
空格键</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<p>&nbsp;<wbr></P>
<p><a NAME="t12" TARGET="_blank"></A><a NAME="t10" TARGET="_blank"></A>小键盘</P>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="731" HEIGHT="474">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_0</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'0'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_1</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'1'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_2</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'2'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_3</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'3'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_4</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'4'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_5</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'5'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_6</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'6'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_7</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'7'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_8</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'8'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_9</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'9'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_ADD</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'+'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_SUBTRACT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'-'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_MULT<a TITLE="IP" HREF="http://www.haogongju.net/tag/IP" TARGET="_blank">IP</A>LY</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'*'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_DIV<a TITLE="IDE" HREF="http://www.haogongju.net/tag/IDE" TARGET="_blank">IDE</A></TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'/'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_EQUALS</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'='</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_COMMA</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键','</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_DOT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'.'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_LEFT_PAREN</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键'('</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_RIGHT_PAREN</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键')'</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_NUMPAD_ENTER</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
小键盘按键回车</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<p>&nbsp;<wbr></P>
<p><a NAME="t14" TARGET="_blank"></A><a NAME="t12" TARGET="_blank"></A>功能键</P>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="735" HEIGHT="308">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F1</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F1</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F2</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F2</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F3</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F3</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F4</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F4</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F5</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F5</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F6</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F6</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F7</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F7</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F8</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F8</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F9</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F9</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F10</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F10</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F11</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F11</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_F12</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
按键F12</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<h2 STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t14"></A><a NAME="t15" TARGET="_blank"></A>&nbsp;<wbr></H2>
<p STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 26px; LisT-sTYLe-TYpe: none; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); pADDinG-Top: 0px">
<a NAME="t16" TARGET="_blank"></A><a NAME="t14" TARGET="_blank"></A>多媒体键</P>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="745" HEIGHT="253">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_<a TITLE="MED" HREF="http://www.haogongju.net/tag/MED" TARGET="_blank">MED</A>IA_PLAY</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 播放</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_STOP</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 停止</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_PAUSE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 暂停</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_PLAY_PAUSE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 播放/暂停</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_FAST_FORWARD</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 快进</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_REWIND</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 快退</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_NEXT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 下一首</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_<a TITLE="PR" HREF="http://www.haogongju.net/tag/PR" TARGET="_blank">PR</A>EVIOUS</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 上一首</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_CLOSE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 关闭</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_EJECT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 弹出</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_MEDIA_RECORD</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
多媒体键 录音</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<p>&nbsp;<wbr></P>
<p><a NAME="t18" TARGET="_blank"></A><a NAME="t16" TARGET="_blank"></A>手柄按键</P>
<div STYLE="Line-HeiGHT: 26px; FonT-FAMiLY: Arial; CoLor: rgb(51,51,51); FonT-siZe: 14px">
<br />
<table STYLE="pADDinG-BoTToM: 0px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; pADDinG-Top: 0px" WIDTH="750" HEIGHT="705">
<tbody>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_1</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用<a TITLE="游戏" HREF="http://www.haogongju.net/tag/%E6%B8%B8%E6%88%8F" TARGET="_blank">游戏</A>手柄按钮#1</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_2</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #2</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_3</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #3</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_4</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #4</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_5</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #5</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_6</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #6</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_7</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #7</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_8</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #8</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_9</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #9</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_10</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #10</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_11</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #11</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_12</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #12</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_13</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #13</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_14</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #14</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_15</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #15</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_16</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
通用游戏手柄按钮 #16</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_A</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 A</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_B</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 B</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_C</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 C</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_X</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 X</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_Y</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 Y</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_Z</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 Z</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_L1</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 L1</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_L2</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 L2</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_R1</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 R1</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_R2</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 R2</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_MODE</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 Mode</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_SELECT</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 Select</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_START</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
游戏手柄按钮 Start</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_THUMBL</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Left Thumb Button</TD>
</TR>
<tr>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
KEYCODE_BUTTON_THUMBR</TD>
<td STYLE="pADDinG-BoTToM: 0px; Line-HeiGHT: 18px; MArGin: 0px; pADDinG-LeFT: 0px; pADDinG-riGHT: 0px; FonT-FAMiLY: Verdana, 宋体, sans-serif; pADDinG-Top: 0px">
Right Thumb Button</TD>
</TR>
</TBODY>
</TABLE>
</DIV>
<p>&nbsp;<wbr></P>							


参考链接  
[安卓使用Root权限实现后台模拟全局按键、触屏事件方法（类似按键精灵）](http://m.blog.csdn.net/article/details?id=39158865)  
[adb 命令模拟按键事件](http://blog.sina.com.cn/s/blog_68f262210102vc1b.html)  


