# 塔岛音乐 · 品牌维护说明

- 产品显示名：塔岛音乐；英文辅助标识：TALIABU MUSIC。
- 视觉系统：深海蓝、海岛青绿、日落金。标志以黑胶唱片、岛屿与日落组合。
- Logo 同时存放于 public/icon.svg、public/music/assets/logo.svg 和 docs/public/icon.svg。
- 统一管理播放器、管理后台、PWA Manifest、Service Worker 缓存、歌单分享页、README 和 Docs 的品牌。
- 为保持兼容，内部 yinyun 包名、JSON 格式、API 路径、Subsonic 接口和数据目录均保持不变。
- 服务器显示名通过环境变量 SERVER_NAME=塔岛音乐 指定。
- 本仓库基于音云 Yinyun（https://github.com/bobcc4/yinyun-lxserver）的私人定制 Fork，保留原作者版权、许可证与致谢。
- 服务器部署见 deploy/taliabu/README.md。严禁把密码、音乐源密钥和服务器私有配置提交到公开仓库。
