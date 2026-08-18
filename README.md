# Phenome · Digital Twin

一个单文件的健康扫描门户设计稿：`digitaltwin.html` 里同时包含了设计系统说明和两个可交互的 WebGL 示例——屏幕空间半调（halftone）人体主视觉，以及分步引导的器官视图。

**在线预览：https://tiffanydesign.github.io/phenome-digital-twin/**

- 排版系统：Contralto（四个光学尺寸）+ New Science Mono（仅 80px 主数字）+ Helvetica Neue
- 三维部分：three.js r160，自带最小 GLB 解析器（不依赖 GLTFLoader）
- 人体为静态构图：不旋转、不可轨道拖拽，主光在被摄体后方，无投影

## 本地运行

**必须通过 HTTP 服务器打开。** 页面用相对路径 `fetch()` 加载 `GLB/*.glb`，在 `file://` 协议下会被同源策略拦截，抛出 `Failed to fetch` 并回退到 2D 程序化人体。

```bash
python -m http.server 8777
```

然后访问 `http://localhost:8777/`（根路径会自动跳转到 `digitaltwin.html`）。

直接双击 HTML 文件**不会**加载三维模型。

## 目录结构

```
index.html                 跳转到 digitaltwin.html（供 GitHub Pages 根路径使用）
digitaltwin.html           设计系统文档 + 两个 WebGL 示例

organ-material-lab.html    材质实验台（拖入任意 .glb 即可换标本），材质参数的准绳
home-hero-lab.html         主视觉预设表：男/女 × 全身/半身
organ-body-full.html       器官视图完整版（用到 realistic_stomach / realistic_human_lungs）
organ-body-stage1.html     器官视图早期版本，保留作演进记录

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
    realistic_human_lungs.glb        organ-body-full.html 专用
    realistic_stomach.glb            organ-body-full.html 专用
```

四个 lab 页与 `digitaltwin.html` 用的是同一套根相对路径（`vendor/`、`GLB/`），所以必须留在仓库根目录，同一个服务器起来后都能直接访问，例如 `http://localhost:8777/home-hero-lab.html`。

`_local/` 是不纳入版本控制的本地目录（见 `.gitignore`），存放没有任何页面引用的备选模型、来源文档，以及仅作设计参考的第三方截图。**它不会被提交，也就没有备份**，需要留存请自行另行归档。

## 浏览器要求

需要支持 WebGL2 与 ES modules 的现代浏览器。three.js 加载顺序为：本地 `vendor/` → jsDelivr → unpkg；三者皆失败时页面保留 2D 图形，不会白屏。

## 字体

Contralto 与 New Science Mono 为商业授权字体，**未随仓库分发**，也未嵌入页面。未安装时会回退到系统衬线 / 等宽字体，版式比例仍然成立，只是字形不同。

## 三维模型来源与授权

`GLB/` 下的全部模型均来自 Sketchfab，采用 [CC Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/)（CC BY 4.0）授权，**允许商业使用，但要求署名**。

| 文件 | 模型 | 作者 | 来源 |
| :--- | :--- | :--- | :--- |
| `Male.glb` `Female.glb` | Male & Female Base Mesh Pack | FormForge3D (`aleenasani841`) | [Sketchfab](https://sketchfab.com/3d-models/male-female-base-mesh-pack-ec3041da6a214c1c995f3d47dc7d04c1) |
| `pink_brain.glb` | pink brain | msurovik | [Sketchfab](https://sketchfab.com/3d-models/pink-brain-b032ee889d844af9b4acd4a2c1ccbba5) |
| `realistic_human_heart.glb` | Realistic Human Heart | neshallads | [Sketchfab](https://sketchfab.com/3d-models/realistic-human-heart-3f8072336ce94d18b3d0d055a1ece089) |
| `lungs.glb` | lungs | reynosa2000 | [Sketchfab](https://sketchfab.com/3d-models/lungs-981d026657984895a90422d5e99e7ac2) |
| `small_and_large_intestine.glb` | Small and large intestine | antonia.sundberg | [Sketchfab](https://sketchfab.com/3d-models/small-and-large-intestine-8a1ca8e3ca224cdeb9264674416bde38) |
| `human_liver_and_gallbladder.glb` | Human liver and gallbladder | ElliotSS | [Sketchfab](https://sketchfab.com/3d-models/human-liver-and-gallbladder-6c4e9bd0d49f4828b804259330c0c6c4) |
| `human_kidney.glb` | Human Kidney | neshallads | [Sketchfab](https://sketchfab.com/3d-models/human-kidney-e1476ceb1e3b4412af5418eee9c5ed08) |

### 前端署名格式

模型经过修改（重拓扑 / 材质替换），按 CC BY 要求，界面上的 ⓘ hover 署名格式为：

> "Realistic Human Heart" by neshallads is licensed under CC BY 4.0. Modified by Phenome Longevity

## License

代码部分未指定 license。三维模型依 CC BY 4.0 授权，字体权利归各自作者所有，详见上文。
