#!/usr/bin/python3
# -*- coding: utf-8 -*-

'''
主题：集成搜索功能的txt小说下载器
     基于小说网站：http://www.jingcaiyuedu.com/
作者：Pychenn
版本：V2.0
时间：2018.03.07
'''

import requests
import re

#查找所输入的小说，返回小说名和url
def search_book():
    url = 'http://www.jingcaiyuedu.com/book/search.html'
    name = input("请输入小说名字（例如：斗破苍穹）：")
    params = {"q":name}
    response = requests.get(url,params = params)    #传入参数构造请求
    response.encoding = 'utf-8'
    if response.status_code == 200:
        html = response.text
        book_list = re.findall(r'''<div class="media-left.*?href="(.*?)" title='(.*?)全文阅读'>''',html,re.S)
    else:
        print("网络有误，请重新输入")
        return search_book()
    #判断搜索中结果有没有输入的小说
    for xiaoshuo in book_list:
        if name == xiaoshuo[1]:
            flag = 1
            name0 = xiaoshuo
            break
        else:
            flag = 0
    if flag ==1:
        print("找到小说：%s,正在下载..."%name)
        return name0
    else:
        print("找不到小说，请重新输入")
        return search_book()

# 获取小说章节信息：url和标题
def get_chapter_infos(novel_url):
    response = requests.get(novel_url)
    response.encoding = 'utf-8'
    html = response.text
    #提取章节目录
    chapter_infos = re.findall(r'<dd><a.*?href="(.*?)">(.*?)<',html,re.S)
    return chapter_infos        #返回章节url和标题

#获取一个章节的内容
def get_chapter_data(chapter_url):
    response = requests.get(chapter_url)        # 发送get请求
    response.encoding = "utf-8"
    html = response.text                # html源码
    #提取章节内容
    chapter_data = re.findall(r'<script>a1\(\);</script>(.*?)<script>a2\(\);</script>',html,re.S)[0]
    #清洗数据
    chapter_data = chapter_data.strip()     #去掉首尾的空格
    chapter_data = chapter_data.replace('&nbsp;&nbsp;&nbsp;&nbsp;','    ')        #换掉空格字符
    chapter_data = chapter_data.replace('<br>','\n')     #换掉换行字符
    chapter_data = chapter_data.replace('<br />','\n')   #换掉换行字符
    return chapter_data     #返回章节内容

#main()函数,执行下载流程
def main():
    data = search_book()
    url = "http://www.jingcaiyuedu.com"+ data[0]
    chapter_info = get_chapter_infos(url)
    #用open()函数写进txt文本
    fb = open('%s.txt'%data[1],'w',encoding='utf-8')
    # 分别写进每个章节的内容
    for chapter in chapter_info:
        chapter_data = get_chapter_data('http://www.jingcaiyuedu.com%s'% chapter[0])
        fb.write(chapter[1])
        fb.write('\n')
        fb.write(chapter_data)
        print("%s  下载完成"%chapter[1])
    fb.close()
    print("%s  下载完成"%data[1])

if __name__ == '__main__':
    main()
