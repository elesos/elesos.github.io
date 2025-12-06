---
layout: post
title: 开源软件推荐系列16:youtube-dl
date: 2025-03-16 05:20:00 +0800
categories: [开源软件推荐系列]
tags: [开源软件推荐系列]
---
下载这个放在path里面
https://yt-dl.org/latest/youtube-dl.exe

youtube-dl [OPTIONS] URL [URL...]

选项
--abort-on-error 
--list-extractors                    List all supported extractors
--extractor-descriptions             Output descriptions of all supported extractors
--match-title REGEX 
-R, --retries RETRIES                Number of retries (default is 10), or
                                     "infinite".

-s, --simulate                       Do not download the video and do not
                                     write anything to disk


还有一个 https://github.com/yt-dlp/yt-dlp  a fork of youtube-dl 

youtube-dl.exe 跟 yt-dlp.exe 什么区别
yt-dlp更新频率高
可以用相同的命令（如 youtube-dl [URL]）运行 yt-dlp。

最好也安装好ffmpeg