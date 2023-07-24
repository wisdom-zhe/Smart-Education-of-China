>前言：最近又要看2023年暑期教师研修高等教育教师专业发展，抓包发现开启倍数无效了，要一个一个点击看视频，岂不是累死人，于是想个办法解放双手。【该网站观看视频时，客户端间隔20~50s向服务端发送一个POST请求，服务器每秒返回ts响应。】



# 1、抓包更换相应值
`注意：下面代码，需要更换自己的cookie、orgId和sourceId`

【点击任意一个视频，开始学习】->【按F12或右键--检查】->【网络（等待15秒），查看Post请求】->【找到对应值更换即可】

![Alt Text](https://raw.githubusercontent.com/wisdom-zhe/MyPic/img/img/small-eduction1.png)
![Alt Text](https://raw.githubusercontent.com/wisdom-zhe/MyPic/img/img/small-eduction2.png)




# 2、替换值后，运行下面代码

```python

# -*- coding:utf-8 -*-
import requests
import json
import time

# 本脚本只刷每个模块的第一个视频，注意运行完脚本后，都需要自己去对应视频答题，即可获得对应学时
"""
1.orgId和sourceId是永久不变的，一次获取后一直可用
2.segId、itemId、courseId是随着不同课程变化的，同一个课程内所有视频都是相同的
3.videoId是每个视频都不一样
"""

orgId=替换自己的
sourceId=替换自己的

courseName=''
courseId=0
itemId=0
segId=0
videoId=0
videoList=[]
item=set()

# 暑期教师研修内容
content=[
    {
        'courseName':"思想铸魂",
        'courseId':726987629091446784,
        'itemId':726641870617784320,
        'segId':726641816054083584,
        'videoList':[728849882907701248]
    },
    {
        'courseName':"固本强基",
        'courseId':726987629116612608,
        'itemId':726641973097213952,
        'segId':726641920563556352,
        'videoList':[728929480488022016]
    },
    {
        'courseName':"以案促学",
        'courseId':726987629129195520,
        'itemId':726642071551070208,
        'segId':726642024438136832,
        'videoList':[734265050282094592]
    },
    {
        'courseName':"综合育人能力提升",
        'courseId':742239030691524608,
        'itemId':742246596178419712,
        'segId':742246596170031104,
        'videoList':[742712347525410816,742712347525410819,742712347525410822]
    },
    {
        'courseName':"数字素养提升",
        'courseId':742239030662164480,
        'itemId':742246596144865280,
        'segId':742246596136476672,
        'videoList':[742504165599907840]
    },
    {
        'courseName':"科学素养提升",
        'courseId':742239030716690432,
        'itemId':742246596228751360,
        'segId':742246596216168448,
        'videoList':[742707008659296256,742707046128746496]
    },
     {
        'courseName':"培养高校创新型教师队伍",
        'courseId':742239030733467648,
        'itemId':742246596270694400,
        'segId':742246596258111488,
        'videoList':[742632994143928320]
    }
]



totalSize=len(content)

# 需要自己抓包获取cookie;
cookie='替换自己的cookie'

"""50s刷新"""
for index in range(totalSize):
# 给每个模块的第一个视频重新赋值
    item=content[index]

    courseId=item.get('courseId')
    itemId=item.get('itemId')
    segId=item.get('segId')
    videoList=item.get('videoList')
    videoId=videoList[0]
    courseName=item.get('courseName')

    print(f'============开始学习{courseName}课程============')

    # 每隔50s发一次post请求
    while True:
        res = requests.post(
            url=f"https://core.teacher.vocational.smartedu.cn/p/course/services/member/study/progress?orgId={orgId}",
            headers={
                    "User-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36 Edg/111.0.1661.62",
                    'Content-Length':'202',
                    'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8',
                    'Host':'core.teacher.vocational.smartedu.cn',
                    'Cookie':cookie
                    },
            data={
                'courseId': courseId,
                'itemId': itemId,
                'videoId': videoId,
                'playProgress': 666,  #这个随便写都不影响结果
                'segId': segId,
                'type': 1,
                'tjzj': 1,
                'clockInDot': 666,  #这个随便写都不影响结果
                'sourceId': sourceId,
                'timeLimit': '-1',
                'originP': 2,
                })

        if json.loads(res.content)['success']:

            if eval(json.loads(res.content)["data"]["progress"])==1:
                print(f'{courseName}学习完成了')
                time.sleep(51)
                break  # 结束当前while循环
               


            print(time.asctime())
            time.sleep(51)

        else:
            print(json.loads(res.content)['success'])
            print("2■■■■■■■■■■■■■■■■■■■■■■■■学习失败■■■■■■■■■■■■■■■■■■■■■■■■")
            print(time.asctime())
            break


```
