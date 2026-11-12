# 我的电视直播源自动整理

每天北京时间06:23运行（UTC前一天22:23，cron: 23 22 * * *），也可 Actions → Daily IPTV → Run workflow 手动运行。GitHub 排队可能延迟，长期无活动可能停用定时任务。

## 当前状态
2026年11月12日手动运行及写回报告已验证：https://github.com/nut7nan/my-tv-sources/actions/runs/43183247500
当前没有确认授权的来源，未生成可播放列表。绿色仅代表程序与报告发布成功；reports/latest.json 的 status=published 才表示发布频道，retained 表示保留旧版。

## 来源与配置
jiandantv/IPTV2026 声称 MIT 同时限制商业用途；iptv-org/iptv 的 README 使用 CC0 标识但 LICENSE 为 Unlicense。两者都未证明逐流节目获准再分发，默认不采集。公开链接及代码许可不等于节目版权授权，不绕过付费、登录或地域限制。
编辑 config/sources.json，在 sources 数组内加入对象，仅在取得列表与所含流再分发许可后启用。例如：
{"name":"授权来源","url":"https://example.com/list.m3u","enabled":true,"redistribution_confirmed":true,"rights_evidence":"权利方可核查许可网址"}
程序仅检查许可记录存在，不能判定授权真伪。授权撤回需及时移除已发布链接。

## 订阅
配置有效授权来源并成功发布后，把以下网址添加到播放器。目前文件未生成，不能播放。
- [全部](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/all.m3u)
- [央视](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/cctv.m3u)
- [卫视](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/satellite.m3u)
- [地方](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/local.m3u)
- [少儿动漫](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/kids.m3u)
- [国际](https://raw.githubusercontent.com/nut7nan/my-tv-sources/main/playlists/international.m3u)

## 处理与局限
要求 EXTM3U 文件头及 EXTINF/HTTP(S) 地址对；同名同URL去重，不同URL保留多线路。按名称分类，可能误分，不能可靠识别国别。列表下载8秒超时、4MB上限；探测4路并发、5秒超时、只读512字节。收到数据不保证画面、声音或持续播放。部分探测失败保留不确定线路；全部不确定、任一来源下载失败或空列表保留旧版。六个列表与报告在单个Git提交发布。任务被终止或写入失败需看Actions日志。

仅 schedule 和 workflow_dispatch 触发，不监听push，不会自动提交循环。权限仅 contents: write，无需个人令牌；串行运行，最多20分钟。手动运行已经验证现有权限可写回，无需放宽设置。以后若拒绝写入，检查 Settings → Actions → General 和分支保护。
