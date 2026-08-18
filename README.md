# Phenome · Digital Twin

一个单文件的健康扫描门户设计稿：`Digital Twin.html` 里同时包含了设计系统说明和两个可交互的 WebGL 示例——屏幕空间半调（halftone）人体主视觉，以及分步引导的器官视图。

- 排版系统：Contralto（四个光学尺寸）+ New Science Mono（仅 80px 主数字）+ Helvetica Neue
- 三维部分：three.js r160，自带最小 GLB 解析器（不依赖 GLTFLoader）
- 人体为静态构图：不旋转、不可轨道拖拽，主光在被摄体后方，无投影

## 运行方式

**必须通过 HTTP 服务器打开。** 页面用相对路径 `fetch()` 加载 `GLB/*.glb`，在 `file://` 协议下会被同源策略拦截，抛出 `Failed to fetch` 并回退到 2D 程序化人体。

```bash
python -m http.server 8777
```

然后访问：

```
http://localhost:8777/Digital%20Twin.html
```

直接双击 HTML 文件**不会**加载三维模型。

## 目录结构

```
Digital Twin.html          设计系统文档 + 两个 WebGL 示例
vendor/
  three.module.js          three.js r160（本地副本；代码内置 jsDelivr / unpkg 兜底）
GLB/
  Male.glb                 人体模型（男）
  Female.glb               人体模型（女）
  organ/
    pink_brain.glb                   Neurology
    realistic_human_heart.glb        Cardiovascular
    lungs.glb                        Respiratory
    small_and_large_intestine.glb    Gut Health
    human_liver_and_gallbladder.glb  Detox
    human_kidney.glb                 Hormones
```

## 浏览器要求

需要支持 WebGL2 与 ES modules 的现代浏览器。three.js 加载顺序为：本地 `vendor/` → jsDelivr → unpkg；三者皆失败时页面保留 2D 图形，不会白屏。

## 字体

Contralto 与 New Science Mono 为商业授权字体，**未随仓库分发**，也未嵌入页面。未安装时会回退到系统衬线 / 等宽字体，版式比例仍然成立，只是字形不同。

## 素材授权（待确认）

`GLB/` 下的人体与器官模型来自第三方素材库，各自的授权条款尚未逐一核实。在补齐来源与许可信息之前，请勿将这些模型用于再分发或商业用途。

## License

代码部分未指定 license。三维模型与字体的权利归各自作者所有，见上。
