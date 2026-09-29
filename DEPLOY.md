# MoonTVPlus Vercel 部署文档

## 站点信息
- **线上地址**: https://moon-tv-plus-chi-six.vercel.app
- **管理后台**: https://moon-tv-plus-chi-six.vercel.app/admin
- **登录账号**: admin
- **登录密码**: MoonTV2026!
- **Vercel 项目**: moon-tv-plus (georgezhou2024's project, Hobby plan)
- **GitHub Fork**: https://github.com/georgezhou2024/MoonTVPlus
- **上游仓库**: https://github.com/mtvpls/MoonTVPlus (v226.0.1)

## 环境变量配置（Vercel → Settings → Environment Variables）

| 变量名 | 值 | 类型 | 环境 | 说明 |
|---|---|---|---|---|
| NEXT_PUBLIC_STORAGE_TYPE | upstash | Config | Production + Preview | 存储类型，必须为 upstash（不能是 localstorage） |
| UPSTASH_URL | https://flexible-stag-319917.upstash.io | Secret | Production | Upstash Redis 连接地址 |
| UPSTASH_TOKEN | gQAAAAAABOGtAAIgcDI5NWYxYzAxYmI1Yzc0YTg2YWUxMGU0ODMxZjlkOWE1Yw | Secret | Production | Upstash Redis 认证 Token |
| USERNAME | admin | Secret | Production + Preview | 后台登录用户名 |
| PASSWORD | MoonTV2026! | Secret | Production + Preview | 后台登录密码 |
| INIT_CONFIG | 见下方 JSON | Secret | Production | 初始播放源配置 |

### INIT_CONFIG 完整 JSON
```json
{"cache_time":7200,"api_site":{"chuyuan":{"api":"https://video.adminqt.cn/api.php/provide/vod/","name":"Chuyuan"},"xiongzhang":{"api":"https://xzcjz.com/api.php/provide/vod","name":"Xiongzhang"},"ikun":{"api":"https://ikunzyapi.com/api.php/provide/vod","name":"IKun"},"haohua":{"api":"https://hhzyapi.com/api.php/provide/vod/","name":"Haohua"},"hongniu":{"api":"https://www.hongniuzy2.com/api.php/provide/vod/","name":"Hongniu"},"guangsu":{"api":"https://api.guangsuapi.com/api.php/provide/vod/","name":"Guangsu"},"liangzi":{"api":"https://cj.lziapi.com/api.php/provide/vod/","name":"Liangzi"},"suoni":{"api":"https://suoniapi.com/api.php/provide/vod/","name":"Suoni"}}}
```

## 播放源列表（来自 Lightconer/tvbox-ysc-config）

| KEY | 名称 | API 地址 |
|---|---|---|
| chuyuan | Chuyuan | https://video.adminqt.cn/api.php/provide/vod/ |
| xiongzhang | Xiongzhang | https://xzcjz.com/api.php/provide/vod |
| ikun | IKun | https://ikunzyapi.com/api.php/provide/vod |
| haohua | Haohua | https://hhzyapi.com/api.php/provide/vod/ |
| hongniu | Hongniu | https://www.hongniuzy2.com/api.php/provide/vod/ |
| guangsu | Guangsu | https://api.guangsuapi.com/api.php/provide/vod/ |
| liangzi | Liangzi | https://cj.lziapi.com/api.php/provide/vod/ |
| suoni | Suoni | https://suoniapi.com/api.php/provide/vod/ |

> 源仓库: https://github.com/Lightconer/tvbox-ysc-config （每日自动更新）
> 以上为 output/4k.json 中 type=1 的标准苹果CMS V10 API 源

## Upstash Redis 配置
- **控制台**: https://console.upstash.com/redis/86cfde3a-4889-4822-b666-2f5d830bf712
- **数据库名**: phclub（免费版仅允许1个）
- **方案**: Free（256MB 存储，10000命令/天）

## 部署步骤回顾
1. Fork mtvpls/MoonTVPlus 到自己的 GitHub
2. Vercel → Import Git Repository → 选择 Fork 仓库
3. 设置环境变量（见上表）
4. 注意：NEXT_PUBLIC_STORAGE_TYPE 必须设为 **Config** 类型（不能是 Secret），否则 Vercel 会因 NEXT_PUBLIC 前缀阻止保存
5. 部署约 3 分钟完成
6. 部署后访问 /admin 确认数据库连接正常

## 常见问题
- **搜索返回0组结果**：检查 NEXT_PUBLIC_STORAGE_TYPE 是否为 upstash，UPSTASH_URL/TOKEN 是否正确
- **管理后台提示"不支持本地存储"**：说明存储类型设错了，必须用 upstash/redis 等数据库
- **Vercel 保存变量报错**：NEXT_PUBLIC_* 前缀的变量必须用 Config 类型，不能用 Secret
- **免费版限制**：Vercel Hobby 每月 100GB 带宽，Upstash 免费版 10000 命令/天

## 文件说明
- `moon-config.json` — 当前播放源配置（可作为订阅源）
- `moontv-logo.png` — MoonTV 图标
