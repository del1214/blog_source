---
title: ffmpeg
date: 2019-07-03 13:36:09
tags:
 - ffmpeg
---

## mp4 生成 m3u8

在线视频直接播放mp4太大，网络跟不上

m3u8介君愁，切成一堆小文件，读取速度会很快

```bash
ffmpeg -i input.mp4 -codec: copy -bsf:v h264_mp4toannexb -start_number 0 -hls_time 10 -hls_list_size 0 -f hls output.m3u8
```

页面中如下

```html
<source src="/output.m3u8" type="application/x-mpegURL">
```
