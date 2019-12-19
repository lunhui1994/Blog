---
layout: '[layout, category]'
title: QQ音乐api(持续更新)
date: 2019-12-5 15:10:03
type: mylist
categories: api
tags: api
keywords: QQ音乐api
---
# 说明

> 接口仅供交流学习使用

[github 项目地址](https://github.com/lunhui1994/node-music-api) 

因为之前使用网络上别人封装的音乐api现在无法使用，或者一些需要付费使用，当然这也无可厚非。

但对我而言，只需要简单的一些音乐api做一些东西。 感觉给钱有点亏。 就自己搞一个简单的符合我的需求的吧。

# 文档

<!-- more -->

## 所支持的Api

1. 音乐搜索
2. 音乐top100列表
3. 音乐播放地址

很简单的三个基本的功能。

所有方法都是GET

## 音乐列表

### /list

发送参数：

| 参数   |  类型  |  描述 |
| ---    |  ---   | ---   |
| p | string  | 页码| 
| n | string  | 数目| 
| w | string  | 关键词 |


返回参数：

| 参数   |  类型  |  描述 |
| ---   |  ---  | ---   |
| curpage | int  | 页码| 
| curnum | int  | 数目| 
| list |  ---  | 音乐列表 |

eg：

```
    http://api.zsfmyz.top/list?p=1&n=30&w=下山
```

返回参数举例

```
    {
    "code": "0",
    "data": {
        "curnum": 30, //数目
        "curpage": 1, //页数
        "list": [
            {
                "songname": "下山", //歌曲名称
                "singer": { // 歌手信息
                    "id": 4185269,
                    "mid": "001adk3q0FE3XP",
                    "name": "要不要买菜",
                    "name_hilight": "要不要买菜"
                },
                "albumname": "下山", //专辑名称
                "songmid": "001GAAVj1GjW8F", //很重要用来获取vkey 拼接播放地址
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/57300_albumpic_9327357_0.jpg" //封面地址
            },
            {
                "songname": "下山 (Cover: 要不要买菜)",
                "singer": {
                    "id": 162021,
                    "mid": "003QznRR2ry01V",
                    "name": "叶洛洛",
                    "name_hilight": "叶洛洛"
                },
                "albumname": "叶洛洛の音乐电台",
                "songmid": "000elQIm3uD9kX",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/99300_albumpic_6938999_0.jpg"
            },
			// ...
        ]
    }
}
```

## 音乐top100列表

### /top

参数

无


返回参数

| 参数   |  类型  |  描述 |
| ---   |  ---  | ---   |
| date | string  | 日期| 
| curpage | int  | 页码| 
| curnum | int  | 数目| 
| list |  ---  | 音乐列表 |
| topinfo |  ---  | 音乐top100信息 |

list中歌曲信息比普通列表多了排名: cur_count



eg：

```
    http://api.zsfmyz.top/top
```

返回参数举例

```
    {
    "code": "0",
    "data": {
        "code": 0,
        "date": "2019-12-05",
        "curnum": 100,
        "curpage": 1,
        "list": [
            {
                "cur_count": "1",
                "songname": "像极了",
                "singer": {
                    "id": 1441799,
                    "mid": "0023dQD40to8NP",
                    "name": "永彬Ryan.B"
                },
                "albumname": "像极了",
                "songmid": "000V8En93R3Dvd",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/36300_albumpic_9218636_0.jpg"
            },
            {
                "cur_count": "2",
                "songname": "拱手相让",
                "singer": {
                    "id": 22529,
                    "mid": "001z6uGh1j5qBh",
                    "name": "胜屿"
                },
                "albumname": "拱手相让",
                "songmid": "002DIlMZ48qB1F",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/66300_albumpic_9414066_0.jpg"
            },
            {
                "cur_count": "3",
                "songname": "余年",
                "singer": {
                    "id": 1060985,
                    "mid": "0022eAG537I1bg",
                    "name": "肖战"
                },
                "albumname": "余年",
                "songmid": "000bFWrY2VrdVp",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/92300_albumpic_9423892_0.jpg"
            },
            {
                "cur_count": "4",
                "songname": "触不可及",
                "singer": {
                    "id": 199509,
                    "mid": "003fA5G40k6hKc",
                    "name": "周深"
                },
                "albumname": "触不可及",
                "songmid": "002EFRnf3ekI9S",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/4300_albumpic_9320604_0.jpg"
            },
            {
                "cur_count": "5",
                "songname": "冷静和热情之间",
                "singer": {
                    "id": 198135,
                    "mid": "001IoTZp19YMDG",
                    "name": "易烊千玺"
                },
                "albumname": "冷静和热情之间",
                "songmid": "0014YYnw3vadJJ",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/59300_albumpic_9415259_0.jpg"
            },
            {
                "cur_count": "6",
                "songname": "美丽谎言",
                "singer": {
                    "id": 71976,
                    "mid": "001gthIA2JeIV1",
                    "name": "都智文"
                },
                "albumname": "美丽谎言",
                "songmid": "003sJCeZ1iK9mZ",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/88300_albumpic_9353488_0.jpg"
            },
            {
                "cur_count": "7",
                "songname": "那男孩还好吗",
                "singer": {
                    "id": 3298773,
                    "mid": "003yGiqM2qF7Gm",
                    "name": "Uu"
                },
                "albumname": "那男孩还好吗",
                "songmid": "002COmzJ0SPZMl",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/36300_albumpic_9132036_0.jpg"
            },
            {
                "cur_count": "8",
                "songname": "星辰大海",
                "singer": {
                    "id": 25724,
                    "mid": "0044vhyY2lfSB8",
                    "name": "周冬雨"
                },
                "albumname": "星辰大海",
                "songmid": "003enTsq4M1J59",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/63300_albumpic_9305663_0.jpg"
            },
            {
                "cur_count": "9",
                "songname": "Lover (Remix)",
                "singer": {
                    "id": 11921,
                    "mid": "000qrPik2w6lDr",
                    "name": "Taylor Swift"
                },
                "albumname": "Lover (Remix)",
                "songmid": "000H6p9p0V4MXi",
                "albumimg": "http://imgcache.qq.com/music/photo/album_300/58300_albumpic_9207358_0.jpg"
            },
        ],
        "topinfo": {
            "ListName": "巅峰榜·新歌",
            "MacDetailPicUrl": "http://y.gtimg.cn/music/common/upload/iphone_order_channel/20150820172435.jpg",
            "MacListPicUrl": "http://y.gtimg.cn/music/common/upload/iphone_order_channel/20150820172427.jpg",
            "UpdateType": "1",
            "albuminfo": "",
            "headPic_v12": "http://y.gtimg.cn/music/common/upload/iphone_order_channel/20150820174934.jpg",
            "info": "集结30天内发行的优质歌曲，鼓励原创、着眼未来的乐坛风向标。根据每日综合数据进行排序，体现QQ音乐用户追新潮流，致力于打造最权威最有公信力的专业健康的新歌排行榜。<br><br>歌曲数量：100首<br>综合数据：登录用户在QQ音乐收听/分享/下载数据",
            "listennum": 1497166,
            "pic": "http://y.gtimg.cn/music/common/upload/iphone_order_channel/20150820172421.jpg",
            "picDetail": "http://y.gtimg.cn/music/common/upload/iphone_order_channel/20150820172414.jpg",
            "pic_album": "http://imgcache.qq.com/music/photo_new/T002R300x300M000000tSk703NJAVD.jpg",
            "pic_h5": "http://y.gtimg.cn/music/common/upload/iphone_order_channel/20150820172242.jpg",
            "pic_v11": "http://y.gtimg.cn/music/common/upload/iphone_order_channel/20150820172421.jpg",
            "pic_v12": "http://y.gtimg.cn/music/photo_new/T003R300x300M000003zALCN1hkB6y.jpg",
            "topID": "27",
            "type": "0"
        }
    }
}
```

## 音乐播放地址

### /music


| 参数   |  类型  |  描述 |
| ---    |  ---   | ---   |
| songmid | string  | 用于获取token | 
| guid | string  | 用于获取token，暂固定用126548448| 

其他参数固定

返回参数

| 参数   |  类型  |  描述 |
| ---    |  ---   | ---   |
| musicUrl | string  | 音乐播放地址| 


eg:

```
    http://api.zsfmyz.top/music?songmid=003lghpv0jfFXG&guid=126548448
```

返回参数举例

```
    {
    "code": "0",
    "data": {
        "musicUrl": "http://ws.stream.qqmusic.qq.com/C400003lghpv0jfFXG.m4a?fromtag=0&guid=126548448&vkey=7888A32FC10168AAD914CA484401762D7F060E7337C0B9187D8B907681BB177669ADB3DFBF398E0FC4D6ED1E0EC7574716872D7B5FE14322"
    }
}

```

over 暂时只有这三个，不过做一个音乐demo足够了，有兴趣的话可以试试。

http://api.zsfmyz.top/ 是目前开放的api接口地址，可直接食用。


# 原接口说明

### 搜索
 - https://c.y.qq.com/soso/fcgi-bin/client_search_cp?aggr=1&cr=1&flag_qc=0&p=1&n=30&w=简单爱

### 封面 
 - http://imgcache.qq.com/music/photo/album_300/[albumid%100]/300_albumpic_[albumid]_0.jpg, albumid%100, albumid
 - 比如albumid=8217，封面地址就是
 - http://imgcache.qq.com/music/photo/album_300/17/300_albumpic_8217_0.jpg。

### 歌曲token 
 - https://c.y.qq.com/base/fcgi-bin/fcg_music_express_mobile3.fcg?format=json205361747&platform=yqq&cid=205361747&songmid=003lghpv0jfFXG&filename=C400003lghpv0jfFXG.m4a&guid=126548448

1. songmid可以从歌曲信息中取到，filename根据songmid生成。
2. 比如，songmid是003lghpv0jfFXG，则filename就是前缀加上C400，后缀加上.m4a，即C400003lghpv0jfFXG.m4a。
3. 其他字段format、platform、cid、guid可以写死，但都是必须的。

### 拼接播放地址
 - http://ws.stream.qqmusic.qq.com/C400003lghpv0jfFXG.m4a?fromtag=0&guid=126548448&vkey=D661E5DF19B8FEB2FBFC554276746AC608AE98B0F30595B3B3BAD5C1C89ECCDD7BE599E306F786621856D22D6BD6B96F5DD344CF3814DB71

[原文依据](https://www.jianshu.com/p/67e4bd47d981)

