# petals.js

花瓣飘落背景特效。单个 JS 文件，零依赖，花瓣贴图已 base64 内联——**只引入一个文件即可生效**。

## 文件

| 文件 | 说明 |
|---|---|
| `petals.js` | 特效脚本，接入只需这一个 |
| `index.html` | 示例页，可实时调整参数 |

## 引入方式

**1. jsDelivr CDN（推荐）**

```html
<script src="https://cdn.jsdelivr.net/gh/bufork/cdn-assets@main/effects/petals/petals.js"></script>
```

**2. 固定版本**：`@main` 跟随仓库最新提交，生产环境建议锁定 tag 或 commit（需仓库中已存在对应 tag），避免上游改动影响线上

```html
<script src="https://cdn.jsdelivr.net/gh/bufork/cdn-assets@v2.0.0/effects/petals/petals.js"></script>
```

**3. 打包工具（Vite / Webpack 等）**

```js
import 'https://cdn.jsdelivr.net/gh/bufork/cdn-assets@main/effects/petals/petals.js';
```

放在 `<head>` 或 `</body>` 前都可以，脚本会自行等待 DOM 就绪，不会因加载时机报错。

## 设置参数的方式

**方式一：全局配置**（必须写在引入脚本**之前**）

```html
<script>
window.PETALS_CONFIG = {
    count: 30,
    speed: 0.8
};
</script>
<script src="https://cdn.jsdelivr.net/gh/bufork/cdn-assets@main/effects/petals/petals.js"></script>
```

**方式二：运行时修改**（调用后立即生效）

```js
Petals.set({ count: 20, opacity: 0.8 });
```

**方式三：关闭自动启用**，改由代码控制时机

```html
<script>window.PETALS_AUTO = false</script>
<script src="https://cdn.jsdelivr.net/gh/bufork/cdn-assets@main/effects/petals/petals.js"></script>
<script>Petals.enable()</script>
```

三种方式可以混用：`PETALS_CONFIG` 决定初始值，`Petals.set()` 在其基础上覆盖。

## 参数

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `count` | number | `50` | 花瓣数量，设为 0 即清空 |
| `size` | number | `1` | 尺寸倍率，每片在 0.7–1.3 倍之间浮动（基准 40px） |
| `speed` | number | `1` | 下落速度倍率 |
| `opacity` | number | `1` | 整体不透明度，0–1 |
| `sway` | boolean | `true` | 是否左右摇摆，`false` 为垂直落下 |
| `zIndex` | number | `9999` | 画布层级，被弹窗遮挡时调高 |

示例：

```js
window.PETALS_CONFIG = {
    count: 30,      // 花瓣数量
    size: 1,        // 尺寸倍率
    speed: 0.8,     // 下落速度
    opacity: 0.9,   // 不透明度
    sway: true,     // 左右摇摆
    zIndex: 9999    // 图层层级
};
```

## API

| 方法 / 属性 | 说明 |
|---|---|
| `Petals.set(options)` | 修改参数，立即生效 |
| `Petals.enable()` | 启用（首次调用会创建画布） |
| `Petals.disable()` | 暂停并隐藏画布 |
| `Petals.destroy()` | 移除画布与事件监听，可再次 `enable()` |
| `Petals.active` | 是否正在播放（只读） |
| `Petals.options` | 当前参数副本（只读） |

## 注意事项

- 画布为 `position: fixed` 全屏覆盖，`pointer-events: none`，不拦截点击与滚动。
- 页面隐藏时自动暂停动画，切回后恢复，后台不耗电。
- 系统开启「减少动态效果」时不自动启用；确需强制，设 `PETALS_AUTO = false` 后手动 `Petals.enable()`。
- jsDelivr 对 `@main` 有缓存（约 12 小时）：推送后若仍是旧版本，访问 `https://purge.jsdelivr.net/gh/bufork/cdn-assets@main/effects/petals/petals.js` 手动刷新。
- SPA 路由切换时调用 `Petals.destroy()`，否则画布与监听会累积。
- 自定义花瓣：把自己的图片转成 base64，替换 `petals.js` 顶部的 `IMG` 常量即可。转换命令：`base64 -w0 your-petal.png`（Windows PowerShell：`[Convert]::ToBase64String([IO.File]::ReadAllBytes("your-petal.png"))`）。

