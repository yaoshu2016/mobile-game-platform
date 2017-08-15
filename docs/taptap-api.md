# TapTap API

通过抓包分析TapTap安卓客户端来获取TapTap的API。

## 总览

https://api.taptapdata.com/

params:
- X-UA: 需要encodeURIComponent
  - V: 1（固定）
  - PN: TapTap（固定）
  - VN_CODE: 如306
  - LANG: 语言，如zh_CN
  - CH: 渠道，如Tencent
  - UID: 用户唯一标识符

headers: 
- Accept-Encoding: gzip
- User-Agent: okhttp/3.6.0

URL示例：

https://api.taptapdada.com/app-tag/v1/hot?from=0&limit=5&X-UA=V%3D1%26PN%3DTapTap%26VN_CODE%3D300%26LANG%3Dzh_CN%26CH%3Dtencent%26UID%3D8634100398XXXXX

response: 
```JavaScript
// success
{
  success: true,
  data: {}
}

// error
{
  success: false,
  data: {
    code: -1,
    msg: '',
    error: '',
    error_description: ''
  }
}
```

## 获取热门标签

URL: /app-tag/v1/hot

params:
- from: 0, 5, 10...
- limit: 5

response data:
```JavaScript
{
  list: [
    {
      data: [...],
      label: '冒险', // 标签名
      params: {
        tag: '冒险' // 标签名
      },
      style: 0
    },
    ...
  ],
  total: 10
}
```

## 根据标签名获取游戏

URL: /app-tag/v1/by-tag

params:
- from: 0, 5, 10...
- limit: 10
- tag: 标签名
- sort: hits, score, updated // 下载量，评分，更新时间

response data:
```JavaScript
{
  list: [
    {
      id: 51131,
      area_label: '',
      area: 'us',
      identifier: 'zombie.survival.craft.z', // 包名
      title: '地球末日',
      icon: {
        url: 'https://img.taptapdada.com/market/lcs/4f9dd3e3c37034055f57e4cb15e2427e_360.png?imageMogr2/auto-orient/strip',
        original_url: 'https://img.taptapdada.com/market/lcs/4f9dd3e3c37034055f57e4cb15e2427e_360.png',
        original_format: 'png',
        ...
      },
      category_key: 'action',
      category: '动作',
      author: 'K-MOBILE', // 厂商
      developer_id: 12094,
      developer_website: null,
      price: {
        google: null,
        apple: ''
      },
      update_date: '2017-08-13',
      sell_price: 0,
      uri: {
        google: 'https://play.google.com/store/apps/details?id=zombie.survival.craft.z',
        apple: 'https://itunes.apple.com/cn/app/id1241932094'
      },
      download: {
        apk_id: 136188,
        apk: {
          name: 'zombie.survival.craft.z-162.apk',
          size: 86946166, // 单位：B
          md5: '41c4e79fa568b3e06b39c79bdc7ac924',
          version_code: 162,
          version_name: '1.5.4' // 当前版本
        }
      }
      banner: { ... }, // 同icon
      additional: [
        {
          key: 'lang',
          value: 'en_US',
          icon: 'https://assets.tapimg.com/img/additional/lang_en.png',
          label: '英文'
        },
        ...
      ]
      stat: {
        hits_total: 2422764, // 下载数
        play_total: 92162,
        rating: {
          score: '8.9',
          max: 10
        },
        review_count: 19659, // 评价数
        reserve_count: 0, // 预约数
        bought_count: 0, // 购买数
        reserve_total: 0, // 同reserve_count
        vote_info: {
          5: 10377,
          4: 3158,
          3: 1263,
          2: 317,
          1: 434
        }
      },
      tags: [
        {
          id: 1,
          value: '独立游戏'
        },
        ...
      ],
      trailer: { // 头部视频
        video_id: '11601',
        label: '预告片',
        tumbnail: { ... }, // 同icon
        blur_url: 'https://img.taptapdada.com/market/images/e7d8ba143341b38975cf0950b9883a2c.jpg?imageMogr2/auto-orient/thumbnail/750x422/format/jpg/quality/80/blur/50x20/interlace/1'
      }
      ...
    },
    ...
  ],
  total: 10
}
```

## 根据ID获取游戏详情

URL: /app/v1/detail

params:
- id: 游戏ID
- referer: 来源，如 tag_list|角色扮演
- time: unix时间戳
- nonce: 随机生成的五位字母数字组合
- sign: 根据算法生成的值（通过抓包和逆向得到算法）

response data:
```JavaScript
{
  // 大部分字段和上一个接口中的游戏数据一致，下面是补充字段
  description: {
    text: '地球末日是一个僵尸生存游戏。...' // 简介
  },
  whatsnew: {
    text: '...' // 更新内容
  },
  screenshots: [
    { ... }, // 同icon
    ...
  ]
  editor_reason {
    text: '' // 编辑的话
  },
  chatting: { // 交流群
    label: '地球末日玩家交流7群：',
    number: '203689623'
  },
  developers: [
    {
      id: 12094,
      label: '厂商', // 还有开发商
      name: 'K-MOBILE',
      website: null
    },
    ...
  ],
  ...
}
```