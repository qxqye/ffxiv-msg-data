# ffxiv-data

FF14 聊天 App（消息转发）用的**游戏数据包**，由国服解包数据转换而来，供手机端本地查询。

## 里面是什么

| 文件 | 说明 |
|---|---|
| `ffxiv-data.db.gz` | gzip 压缩的 SQLite 库（约 1.7 MB，解压后约 7 MB） |
| `manifest.json` | 版本号、条数、大小、sha256，客户端拿它判断要不要更新 |

库里有三张表：

- `phrase(grp, key, text)` —— 定型文（auto-translate）词条，游戏里那句「你会说德语吗」之类
- `item(...)` —— 道具 / 装备的名称、品级、部位、可用职业、伤害、物防、属性
- `meta(key, value)` —— 版本信息

数据来源：[InfSein/ffxiv-datamining-mixed](https://github.com/InfSein/ffxiv-datamining-mixed) 的 `chs/` 目录（国服解包 CSV）。

## 版本号怎么读

```json
"version": "国服 7.57 · 2026.10.06 · 935169e",
"versionCode": 20261006
```

`versionCode` 就是解包日期，客户端只比这个整数：比本地大就下载新包，否则跳过。

## 客户端怎么取

```
manifest.json  →  https://gh-proxy.com/https://raw.githubusercontent.com/qxqye/ffxiv-msg-data/main/manifest.json
数据包          →  https://gh-proxy.com/https://raw.githubusercontent.com/qxqye/ffxiv-msg-data/main/ffxiv-data.db.gz
```

`gh-proxy.com` 只是国内访问 raw.githubusercontent 的加速前缀，别的镜像（jsdelivr 等）同样可用。

## 更新方式

游戏大版本更新后，重新从上游拉 CSV → 生成数据包 → 覆盖这两个文件。客户端下次打开会自动发现新版本。
