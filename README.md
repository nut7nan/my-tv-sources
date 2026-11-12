# 我的电视直播源

每天北京时间06:23自动运行（UTC前一天22:23，cron: 23 22 * * *）。仅提取两个公开M3U中的频道名和HTTP(S)播放地址，去重、分类成自己的M3U，不搬运整个仓库、不保存视频、不转播视频。

## 订阅
把以下地址复制到支持M3U的播放器。能否播放还取决于你所在网络、地域限制和上游状态。

- [全部 all.m3u](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/all.m3u)
- [央视 cctv.m3u](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/cctv.m3u)
- [卫视 satellite.m3u](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/satellite.m3u)
- [地方 local.m3u](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/local.m3u)
- [少儿动漫 kids.m3u](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/kids.m3u)
- [国际 international.m3u](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/international.m3u)

## 来源与使用范围
按用户要求启用：[jiandantv/IPTV2026](https://github.com/jiandantv/IPTV2026) 的 live.m3u，以及 [iptv-org/iptv](https://github.com/iptv-org/iptv) 的官方 index.m3u。保留来源归属，仅限个人非商业链接整理。jiandantv README 声称MIT同时明确禁止商业用途；iptv-org README的CC0标识与实际Unlicense许可证不一致。软件或列表许可不等于节目版权授权，本项目不声称逐条流已获内容授权。尊重权利方条款，不绕过登录、付费或访问限制；收到有效权利通知应及时移除对应链接。

## 配置与手动运行
config/sources.json 中每项有 name、url、enabled 和 rights_status。enabled 改为 false 可停用来源；initialized 保持 true，避免重新自动初始化。rights_status 是来源情况记录，不代表节目授权证明。
在 Actions → Daily IPTV → Run workflow 可手动运行。reports/latest.json 记录每个来源的解析数量、摘要和结果：published 为已发布；unchanged 为来源未变；retained 为出现异常并保留旧列表。红色运行记录需查看错误原因。

## 处理及局限
M3U要求EXTM3U头和EXTINF/HTTP(S)地址对。相同频道名与相同URL去重，同频道不同URL保留多线路。名称启发式分为央视、卫视、地方、少儿动漫、国际，可能误分，国际不表示准确国别；暂不保留台标、EPG和特殊播放请求头。
每个来源下载15秒超时、12MB上限。比较来源SHA256，有变化才重建列表。每次最多抽查64条、4路并发、5秒超时，只读取512字节；未抽查及不确定线路不删除。收到数据不保证画面音轨或持续播放。抽查全部失败、任一来源失败或为空时保留前次列表，并记录报告；首次失败则尚无列表。六个列表和来源状态在单个Git提交中发布。分类为空的文件仍是合法空M3U。

仅 schedule 与 workflow_dispatch 触发，无push触发，不会提交循环。GITHUB_TOKEN仅 contents: write，无个人令牌；串行运行，最多20分钟。当前运行权限已验证可写回，无需额外放宽。GitHub排队可能延迟，公开仓库长期无活动可能停用定时任务，需到Actions恢复。
