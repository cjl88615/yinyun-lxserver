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
