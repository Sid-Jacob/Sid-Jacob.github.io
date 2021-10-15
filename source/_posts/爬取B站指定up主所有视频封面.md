---
title: 爬取B站指定up主所有视频封面
date: 2021-10-15 14:09:45
tags:
- 爬虫
categories:
- [编程, 杂, 爬虫] 
lang: zh
---
```python
'''
爬取B站指定up主所有视频封面
'''
import requests, math, time, random, re, os

start = time.time()

mid =   # 修改1，B站UP主的mid，从网址复制
save_dir = './tem/'  # 修改2，图片保存路径

URL = 'https://api.bilibili.com/x/space/arc/search?mid={}&ps=30&tid=0&pn={}&keyword=&order=pubdate&jsonp=jsonp'

if not os.path.exists(save_dir):
    os.makedirs(save_dir)
ps = int(re.findall('ps=(.*?)&', URL)[0])
res = requests.get(url=URL.format(mid, 1)).json()
total_video_counts = res['data']['page']['count']
total_pn_counts = math.ceil(total_video_counts / ps)
time.sleep(0.05)

video_list = []
for i in range(total_pn_counts):
    res = requests.get(url=URL.format(mid, i + 1)).json()
    for item in res['data']['list']['vlist']:
        tem_dict = {}
        tem_dict['title'] = item['title']
        tem_dict['pic'] = item['pic']
        tem_dict['bvid'] = item['bvid']
        video_list.append(tem_dict)
    time.sleep(random.uniform(0.05, 0.2))

download_counts = 0
skip_counts = 0
for item in video_list:
    # print(item['pic'])
    image = requests.get(item['pic']).content
    if '《' in item['title']:
        title = re.findall('《(.*?)》', item['title'])[0]
    else:
        title = item['title']
    if '/' in title:
        title = title.replace('/', ',')
    if '互动' in item['title']:
        skip_counts += 1
        continue
    with open(save_dir + '{}_防{}重.jpg'.format(title, random.randint(1, 1000)),
              'wb') as f:
        f.write(image)
        download_counts += 1
    time.sleep(random.uniform(0.05, 0.2))

end = time.time()
print('{}张封面下载完成，任务计时{}秒，跳过{}个互动视频'.format(download_counts,
                                           round(end - start, 2), skip_counts))
```

————————————————

参考链接：https://blog.csdn.net/Rookie_Max/article/details/112313636


