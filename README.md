# tvbox-web-models

浏览器端可直接运行的小模型集合。模型以 ONNX 发布，用 onnxruntime-web（WASM 后端）在页面内推理，不上传任何数据。

## 目录约定

```
models/
  index.json                  索引；前端只读这一个文件
  <model-id>/
    README.md                 模型卡（输入输出、指标、短板）
    DISCLAIMER.md             免责声明
    <variant>/model.onnx      按量化档分目录：fp32 / int8 / q8
```

新增模型时按此结构放入，并在 `index.json` 的 `models` 数组里追加一条。文件名一律 `model.onnx`，量化档由目录区分，避免 `model.fp32.onnx` 这类后缀越堆越长。

## 现有模型

| ID | 定位 | 参数 | 体积 | 状态 |
|---|---|---|---|---|
| `silver-adskip-1-mini-beta` | 三模态广告识别，三路等权 | 48,963 | 83–195 KB | beta |

## 版本与标签

仓库本身不打 `vX.Y.Z` 标签——那是仓库的版本，会跟模型版本混。每个模型单独打标签：

```
<model-id>-<version>        例：silver-adskip-1-mini-beta-1.0.0-beta.1
```

Release 标题写模型名与定位，正文写该模型的指标与限制，一律标 pre-release 直到正式版。

## 许可

AGPL-3.0。模型权重与推理代码同许可。

## 免责

各模型自带 `DISCLAIMER.md`，使用前必读。概要：输出是概率估计不是事实判定；beta 阶段可能随时变更或停止分发；不得用于作出对他人不利的决定。
