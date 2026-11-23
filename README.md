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

## 索引字段

`models/index.json` 里每个模型条目除常规信息外，有一个决定展示的开关：

- `frontend`：是否在前端的模型列表里出现。`true` 出现，`false` 只留在仓库里，前端看不到也选不到。目前只有 `silver-adskip-1-mini-omega` 是 `true`，其余模型前端一律不显示。
- `beta`：模型本身是否仍处于实验状态。功能是否正式、模型是否实验，是两件事——广告识别这个功能已经是正式功能，Omega 里的 `beta` 字段为 `false`。
- `threshold` 与 `threshold_source`：模型建议的判定阈值及其由来，前端默认采用。

字段缺失时按兼容处理：`frontend` 缺省视为显示，`beta` 缺省视为实验状态。

## 现有模型

| ID | 定位 | 参数 | 体积 | 前端 | 状态 |
|---|---|---|---|---|---|
| `silver-adskip-1-mini-omega` | 三模态广告识别，音频主判 + 画面佐证 | 70,483 | 87–279 KB | 显示 | 正式 1.0.0 |
| `silver-adskip-1-mini-alpha` | 同上，音频优先的旧判定策略 | 70,483 | 87–285 KB | 不显示 | alpha |
| `silver-adskip-1-mini-beta` | 三路等权的第一版 | 48,963 | 83–195 KB | 不显示 | beta |

## 版本与标签

仓库本身不打 `vX.Y.Z` 标签——那是仓库的版本，会跟模型版本混。每个模型单独打标签：

```
<model-id>-<version>        例：silver-adskip-1-mini-omega-1.0.0
```

Release 标题写模型名与定位，正文写该模型的指标与限制，一律标 pre-release 直到正式版。

## 许可

AGPL-3.0。模型权重与推理代码同许可。

## 免责

各模型自带 `DISCLAIMER.md`，使用前必读。概要：输出是概率估计不是事实判定；实验阶段可能随时变更或停止分发；不得用于作出对他人不利的决定。
