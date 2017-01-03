---
layout: post
title: linux下串口读写 
category: Tech
tags: linux_serial
keywords: serial
description: linux.
---

## 目的：
最近由于项目需求需要从串口读取数据并做一个简单的展示，在这里上网查了一些资料，写了一下，能够完成linux下串口的监听和读入，并将读入的数据流转换为十六进制的ascii表示存储在文本文件中。

## 使用方法：
1：通过串口转USB转接线，使得串口连接到Linux操作系统上   
2: 安装pyserial模块  [pip install pyserial]    
3：启动read.py       [python /path/of/read.py]    

## 一些说明：
+ 可以通过 [tailf /tmp/mSerial.log] 查看实时的串口读数据字节数的日志记录
+ 可以通过 [tailf /tmp/mSerialdata] 查看实时串口读取的数据(该数据为字节流的十六进制的ascii码表示)。
+ /tmp/mSerialdata 为串口读取到的数据存储的位置
+ /tmp/mSerial.log 为串口读的日志记录，记录已经读取的数据字节个数

## 测试说明

+ 测试时候的链接方式如下图
![connect.jpg](/public/img/pic/connect.jpg)
+ 数据读取的结果展示如下图
![connect.jpg](/public/img/pic/display.png)

## CODE
pyserial的文档见[这里http://pyserial.readthedocs.io/en/latest/pyserial.html](http://pyserial.readthedocs.io/en/latest/pyserial.html)

``` python
# -*- coding: utf-8 -*-
# @Author: gxn
# @Date:   2016-12-30 22:41:04
# @Last Modified by:   gxn
# @Last Modified time: 2017-01-02 19:29:25
#!/usr/bin/env python

import serial
import time
import thread

class MSerialPort:
	message=''
	def __init__(self,port,buand):
		self.port=serial.Serial(
			port,
			buand
			)
		self.port.flushInput()
		self.port.flushOutput()
		self.counter=0

		if not self.port.is_open:
			self.port.open()
	def port_open(self):
		if not self.port.is_open:
			self.port.open()
	def port_close(self):
		self.port.close()
	def send_data(self,data):
		number=self.write(data)
		return number
	def read_data(self):
		while True:
			data=''
			while self.port.inWaiting()>0:
				data+=self.port.read(1)
				self.counter+=1
			if data !='':
				timestamp=str( time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(time.time())) )+ " "
				savedata('/tmp/mySerialdata',timestamp+data_handle(data))
				data =''
def savedata(path,data):
	savefile=open(path,'a+')
	savefile.write(data)
	savefile.close()

def data_handle(data):
	result=''
	for x in xrange(len(data)):
		result+= (hex(ord(data[x]))[2:]+' ')
	return result+'\n'
if __name__=='__main__':
	mSerial=MSerialPort('/dev/ttyUSB0',9600)
	thread.start_new_thread(mSerial.read_data,())
	print "Process Runing...."
	mSerial_log_file="/tmp/mSerial.log"
	while True:
		time.sleep(5)
		templog_data = str( time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(time.time())) )+": "+ str(mSerial.counter)+'\n'
		savedata(mSerial_log_file,templog_data)
```

