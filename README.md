# Portrait Gallery 人像参考库

AI 人像参考资产库，来自 <https://ai-portrait-gallery.pages.dev/> 的本地镜像。

相关图库：
- 服装参考图：<https://ai-costume-gallery.pages.dev/>
- 线稿姿态图：<https://ai-pose-gallery.pages.dev/>
- 深度姿态图：<https://ai-pose-depth-gallery.pages.dev/>

## 资产概览

- **总图片量**: 1499 张 PNG 原图（原寸）
- **总体积**: 4,862,010,894 字节（约 4.86 GB）
- **两大分区**: 面部 893（正面向、无地背景的脸部参考）、造型 606（含服装、发型质感与氛围）
- **分类（分区 × 发型）**:

| 分类 | 数量 |
|---|---:|
| 面部·短发 | 433 |
| 面部·中发 | 229 |
| 面部·中长发 | 154 |
| 面部·长发 | 77 |
| 造型·短发 | 294 |
| 造型·中发 | 147 |
| 造型·中长发 | 106 |
| 造型·长发 | 59 |
| **合计** | **1499** |

- **目录结构**: `分类/ID.png`（如 `面部·短发/f71f980dde3.png`）
- **版权**: 站点标注为 CC0，可自由用于商用、编辑与参考（人物为 AI 生成的虚构人物）

## 元数据

[`metadata_index.json`](./metadata_index.json) 每项字段：

- `id`: 图片唯一编号（站点原始 ID）
- `section_ja` / `section_zh`: 分区（フェイス → 面部、スタイリング → 造型）
- `hair_ja` / `hair_zh`: 发型（ショート → 短发、ミディアム → 中发、セミロング → 中长发、ロング → 长发）
- `style`: 更细的发型/风格描述（可能为空）
- `w` / `h`: 原图宽高
- `bytes`: 原图字节数
- `sha256`: 原图 SHA-256 校验值
- `original_url`: 原寸 PNG 地址（已下载）
- `thumb_url`: WebP 缩略图地址（未下载，可另行获取）
- `local_path`: 本地相对路径
- `meta.total` / `meta.total_bytes` / `meta.counts` / `meta.sections`: 总量、总体积与统计

## 校验

下载完成后已逐文件核对：

- 站点 `data/images.js` 共 1499 条，本地目录 1499 个 PNG，ID 集合完全一致
- 每个文件大小与元数据 `bytes` 一致，总体积一致
- 每个文件通过 PNG 签名、Pillow 解码与宽高校验
- 1499 个 SHA-256 互不重复

重新校验：

```bash
python3 - <<'PY'
import json, os, hashlib
root = os.path.dirname(os.path.abspath("metadata_index.json"))
d = json.load(open("metadata_index.json"))
for x in d["items"]:
    p = os.path.join(root, x["local_path"])
    assert os.path.getsize(p) == x["bytes"], p
    h = hashlib.sha256(open(p, "rb").read()).hexdigest()
    assert h == x["sha256"], p
print("OK", len(d["items"]))
PY
```

## 重新同步

```bash
curl -sL https://ai-portrait-gallery.pages.dev/data/images.js    # 完整图片清单（含路径、尺寸、字节数）
# 原寸图：https://ai-portrait-gallery.pages.dev/img/full/<id>.png
# 缩略图：https://ai-portrait-gallery.pages.dev/img/thumb/<id>.webp
```

## 日语 → 中文 对照

| 原文 | 中文 |
|---|---|
| フェイス | 面部 |
| スタイリング | 造型 |
| ショート | 短发 |
| ミディアム | 中发 |
| セミロング | 中长发 |
| ロング | 长发 |
| 原寸 | 原图（原尺寸） |
| サムネ | 缩略图 |
| キーワード | 关键词 |
| カテゴリ | 分类 |
