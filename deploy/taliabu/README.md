# 塔岛音乐部署手册

应用：塔岛音乐（TALIABU MUSIC），基于音云 Yinyun v1.9.1。
目标：192.168.2.95（Debian 13），播放器 http://192.168.2.95:9527/，管理 /admin。
代码：/opt/web/taliabu-music；镜像：taliabu-music:1.9.1-brand1。
密码：/etc/taliabu-music/credentials.env（仅 root 可读，不要提交 GitHub）。
持久化：/opt/web/taliabu-music/{data,logs,cache,music}。
此发行版由个人 Fork 源码构建，包含品牌定制的网页和歌单分享页。

## 部署与检查

    cd /opt/web/taliabu-music
    docker compose -f deploy/taliabu/compose.yaml build
    docker compose -f deploy/taliabu/compose.yaml up -d
    docker compose -f deploy/taliabu/compose.yaml ps

    curl -I http://192.168.2.95:9527/
    curl -I http://192.168.2.95:9527/admin
    docker inspect -f '{{.State.Health.Status}}' taliabu-music

更新前记录当前 Git SHA 和 Docker 镜像 ID；如果有问题恢复到此前代码/镜像，不要删除 data/music/cache 目录。
仅绑定局域网 IP，默认不向公网开放；音源脚本须来自可信来源，不开启 unsafe VM。音乐内容须遵守使用权限和版权授权。
原项目、许可证及贡献者署名：[bobcc4/yinyun-lxserver](https://github.com/bobcc4/yinyun-lxserver)。


## 在线音源恢复与维护（2026-11-05）

在线搜索来自内置音乐 SDK，**在线播放 URL 仍依赖独立的自定义音源**。未配置时服务端日志会出现 `未找到支持 tx 平台的自定义源`、`POST /api/v1/player/music/url 500`。容器健康检查和搜索正常都不能替代实际在线播放验收。

- 当前部署从 `pdone/lx-music-source` 取得 Huibq v1.2.0 的透明 JS 音源，原文件的 Git blob SHA 为 `f2028068db448a9e0b738106112eb300bcefbcc2`，上游 Git commit 为 `000578ddca4a676012cf735abdc3489efdc67c74`。不得自动执行未审查的远程新版本。
- 音源代码与用户分享状态位于持久化目录 `data/users/source/`，**不提交到公开 GitHub，也不写入镜像层**。服务器现有音源所有者为默认服务账户 `taliabu`；`shares.json` 的 `targetUsers: ["*"]` 向其他音乐账户授权使用。
- `allowUnsafeVM` / `system.allowUnsafeVM` 均保持关闭。新脚本应静态审核、在独立测试容器验证后，再导入正式服务器，绝不因为兼容性而直接开启不安全 VM。
- 该来源声明禁止批量下载，仅支持 `kw/kg/tx/wy/mg` 的标准音质（主要为 128k、320k），属于社区第三方服务。**不能保证在线资源授权、可用率、无损质量或长期免费**。用户仅应播放/下载有权限使用的歌曲。
- 已用隔离测试用户验证酷我/QQ 音频解析返回 HTTP 200，以及播放器服务器音频代理 HTTP 206 分段响应，正式部署已验证所有用户共享配置生效；尚未逐首验证完整在线曲库。
- 修改前备份在 `/var/backups/taliabu-music/before-source-20261105.tar.gz`，该文件仅 root 可读；恢复时先备份现有数据，切勿覆盖后续新增的用户与歌单。
- 启动后从 `docker logs taliabu-music` 确认出现 `成功加载: Huibq_lxmusic源`，然后浏览器重新登录，先用 `128k` 测试无会员限制的歌曲。第三方源失效不应影响本地 `/music` 目录歌曲的播放。

## 2026-11-05 用户界面精简（brand2）

- 播放器与管理员界面去除 GitHub、源码版权页脚和软件版本更新入口；关于页统一展示「塔岛音乐 · 2D厨房」。初次使用的合规提示仍保留，但不再导航到仓库。
- 原作者版权及开源许可持续保留于 `LICENSE`、`NOTICE.md`、源文件头部和本项目文档中；UI 隐去开发者链接不代表代码来源不可追溯。
- 运行镜像：`taliabu-music:1.9.1-brand2`。更改 PWA/Service Worker 缓存版本以避免旧页面残留。
