# HTML5 面试题及标准答案（100 道）

## 一、HTML5 基础概念题（1-20 题）

| 题号 |                             问题                             |                           标准答案                           |
| :--: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  1   |          HTML5 的 DOCTYPE 声明是什么？有什么作用？           | `<!DOCTYPE html>`；作用：1. 告诉浏览器文档使用 HTML5 标准解析；2. 区分 HTML5 与 HTML4，避免浏览器进入怪异模式（Quirks Mode）。 |
|  2   |              HTML5 相比 HTML4 有哪些主要改进？               | 1. 新增语义化标签（header/nav/main 等）；2. 原生支持音视频（audio/video）；3. 增强表单（新输入类型、新属性）；4. 新增 Canvas、本地存储、地理定位等 API；5. 简化 DOCTYPE 声明，废除过时标签（如 font/center）。 |
|  3   |            HTML5 的页面基本结构包含哪些核心标签？            | `<!DOCTYPE html>`（声明）、`<html>`（根元素）、`<head>`（元信息）、`<meta charset="UTF-8">`（编码）、`<title>`（标题）、`<body>`（可视内容）。 |
|  4   |    什么是 HTML5 的语义化标签？使用语义化标签有什么好处？     | 语义化标签是指有明确含义的标签（如 header/nav/article），而非无意义的 div；好处：1. 提高代码可读性和可维护性；2. 利于 SEO（搜索引擎抓取）；3. 提升无障碍访问（屏幕阅读器识别）。 |
|  5   |              HTML5 中`<main>`标签的特点是什么？              | 表示页面的核心内容，一个页面**只能有一个**`<main>`标签；不能嵌套在 header、footer、aside、nav 等标签内。 |
|  6   | `lang`属性的作用是什么？`<html lang="zh-CN">`和`lang="zh"`有什么区别？ | 作用：指定页面 / 元素的语言，辅助浏览器、搜索引擎、翻译工具识别；区别：`zh-CN`特指简体中文（中国大陆），`zh`是通用中文（无地域限定）。 |
|  7   | HTML5 中`<meta>`标签的`viewport`属性作用是什么？核心值有哪些？ | 作用：适配移动端页面，控制视口的大小和缩放；核心值：`width=device-width`（视口宽度等于设备宽度）、`initial-scale=1.0`（初始缩放比例 1:1）、`user-scalable=no`（禁止用户缩放）。 |
|  8   |          HTML5 废除了哪些 HTML4 的标签？举例说明。           | 废除无语义 / 纯样式标签：`<font>`（字体样式）、`<center>`（居中）、`<frame>`（框架）、`<marquee>`（滚动文字）等；建议用 CSS 替代样式，语义化标签替代结构。 |
|  9   |          什么是 HTML5 的全局属性？列举 5 个常用的。          | 可作用于所有 HTML 元素的属性；常用：class（类名）、id（唯一标识）、style（行内样式）、title（悬停提示）、data-*（自定义属性）、hidden（隐藏元素）、required（表单必填）。 |
|  10  |         `data-*`属性的作用是什么？如何通过 JS 获取？         | 作用：存储页面自定义数据（不影响渲染）；JS 获取：元素`.dataset.属性名`（如`<div data-id="123">` → `div.dataset.id`）。 |
|  11  |       HTML5 中`<figure>`和`<figcaption>`的作用是什么？       | `<figure>`用于包裹独立的媒体内容（图片、图表、代码）；`<figcaption>`是`<figure>`的标题 / 说明，嵌套在`<figure>`内。 |
|  12  |             HTML5 的字符编码为什么推荐用 UTF-8？             | 1. UTF-8 是万国码，支持所有语言（中文、英文、特殊符号等）；2. 避免页面出现乱码；3. 是现代浏览器默认推荐的编码格式。 |
|  13  |               `<head>`标签内通常包含哪些内容？               | 元信息：`<meta>`（编码、视口）、`<title>`（页面标题）、`<link>`（引入 CSS / 图标）、`<script>`（引入 JS，建议放 body 底部）、`<style>`（内部样式）。 |
|  14  |            HTML5 中`<link>`标签的常用属性有哪些？            | `rel`（关系，如 stylesheet/CSS、icon / 图标）、`href`（资源路径）、`type`（资源类型，如 text/css）。 |
|  15  |      什么是自闭合标签？HTML5 中常见的自闭合标签有哪些？      | 无需结束标签的标签（`<标签 />`）；常见：`<meta />`、`<link />`、`<img />`、`<input />`、`<br />`、`<hr />`。 |
|  16  |          HTML5 中`<img>`标签的`alt`属性作用是什么？          | 1. 图片加载失败时显示替代文本；2. 辅助屏幕阅读器识别图片内容；3. 利于 SEO 优化。 |
|  17  |      `<a>`标签的`target`属性有哪些取值？分别代表什么？       | `_self`（默认，在当前窗口打开）、`_blank`（新窗口打开）、`_parent`（父框架打开）、`_top`（顶层框架打开）。 |
|  18  |   HTML5 中`<script>`标签的`defer`和`async`属性有什么区别？   | 相同点：异步加载 JS 文件，不阻塞页面渲染；区别：`defer`加载完成后**等待 DOM 解析完成再执行**（按引入顺序执行）；`async`加载完成后**立即执行**（不保证顺序）。 |
|  19  |              如何在 HTML5 中引入外部 CSS 文件？              | 使用`<link>`标签：`<link rel="stylesheet" href="style.css" type="text/css">`。 |
|  20  |       HTML5 中注释的写法是什么？注释会显示在页面上吗？       | 写法：`<!-- 注释内容 -->`；注释不会显示在页面可视区域，但可在开发者工具中看到，用于代码说明。 |

## 二、HTML5 表单增强题（21-40 题）

| 题号 |                           问题                           |                           标准答案                           |
| :--: | :------------------------------------------------------: | :----------------------------------------------------------: |
|  21  |        HTML5 新增了哪些表单输入类型？列举 8 个。         | email、tel、url、number、date、time、range、search、color、datetime-local 等。 |
|  22  |        `type="number"`和`type="tel"`有什么区别？         | `number`：限制输入数字（含小数点），移动端调数字键盘，可设置 min/max/step；`tel`：无格式验证，仅提示移动端调数字键盘（支持输入 -、空格等分隔符），适合手机号 / 固定电话。 |
|  23  |         HTML5 表单中`required`属性的作用是什么？         | 标记输入框为必填项，表单提交时自动验证，若为空则提示用户填写（无需额外 JS）。 |
|  24  |     `placeholder`属性的作用是什么？有什么注意事项？      | 作用：在输入框中显示提示文字（占位符），用户输入后消失；注意：不能替代 label 标签（不利于无障碍访问），且占位符颜色默认较浅。 |
|  25  |              `autofocus`属性的作用是什么？               | 页面加载完成后，自动将光标聚焦到该表单元素（一个页面建议只使用一次）。 |
|  26  |          `pattern`属性的作用是什么？举例说明。           | 作用：通过正则表达式验证输入内容；示例：手机号验证 `<input type="tel" pattern="^1[3-9]\d{9}$" title="请输入正确的手机号">`。 |
|  27  | HTML5 表单中`autocomplete`属性的取值有哪些？作用是什么？ | 取值：`on`（开启，默认）、`off`（关闭）；作用：控制是否开启输入自动补全（基于用户历史输入）。 |
|  28  |  `min`/`max`/`step`属性适用于哪些表单类型？作用是什么？  | 适用于 number、date、range、time 等类型；`min`（最小值）、`max`（最大值）、`step`（步长，每次增减的数值）。 |
|  29  |     HTML5 中`<datalist>`标签的作用是什么？如何使用？     | 作用：为输入框提供下拉建议列表；使用：`<input list="fruits"> <datalist id="fruits"> <option value="苹果"> <option value="香蕉"> </datalist>`。 |
|  30  |           HTML5 表单提交时，如何实现原生验证？           | 1. 使用 required、pattern、min/max 等属性；2. 表单提交时浏览器自动验证，验证失败则提示并阻止提交。 |
|  31  |             如何禁用 HTML5 表单的原生验证？              |            在`<form>`标签中添加`novalidate`属性。            |
|  32  |      `type="search"`与普通`type="text"`有什么区别？      | `search`：移动端显示搜索键盘，部分浏览器会在输入后显示 “清除” 按钮（×），样式更贴合搜索场景。 |
|  33  |               `type="color"`的作用是什么？               |    提供原生颜色选择器，返回十六进制颜色值（如 #ff0000）。    |
|  34  |           HTML5 中`<output>`标签的作用是什么？           | 用于显示表单元素的计算结果（如 range 滑块的数值），需通过 JS 关联表单元素。 |
|  35  |   如何实现表单的文件上传？HTML5 对文件上传有哪些增强？   | 基础写法：`<input type="file">`；增强：1. `multiple`属性支持多文件上传；2. `accept`属性限制文件类型（如`accept="image/*"`）；3. 可通过 File API 读取文件内容。 |
|  36  |          `type="datetime-local"`的作用是什么？           | 提供本地日期时间选择器（年 - 月 - 日 时：分），返回格式如 2026-01-15T10:30。 |
|  37  |           HTML5 表单中`form`属性的作用是什么？           | 允许表单元素不在`<form>`标签内，通过`form="表单id"`关联到指定表单（如`<input type="text" form="myForm">`）。 |
|  38  |          `readonly`和`disabled`属性有什么区别？          | 相同点：都让输入框不可编辑；区别：1. `readonly`元素仍可提交数据，`disabled`元素不会提交；2. `readonly`元素可聚焦，`disabled`元素不可聚焦。 |
|  39  |             如何设置表单元素为只读且可提交？             |            使用`readonly`属性（而非 disabled）。             |
|  40  |  HTML5 中`<label>`标签的作用是什么？有哪两种使用方式？   | 作用：关联表单元素，点击 label 可聚焦对应输入框（提升用户体验和无障碍性）；方式：1. `<label for="username">用户名</label> <input id="username">`；2. `<label>用户名 <input></label>`。 |

## 三、HTML5 多媒体与 Canvas 题（41-60 题）

| 题号 |                           问题                           |                           标准答案                           |
| :--: | :------------------------------------------------------: | :----------------------------------------------------------: |
|  41  |      HTML5 中 `` 和`<video>`标签的核心属性有哪些？       | controls（显示播放控件）、autoplay（自动播放）、loop（循环播放）、muted（静音）、src（资源路径）、poster（video 封面）。 |
|  42  |    为什么`<video>`标签需要提供多个`<source>`子标签？     | 不同浏览器支持的音视频格式不同（如 MP4、WebM、Ogg），多 source 可实现兼容，浏览器会加载第一个支持的格式。 |
|  43  |      ``/`<video>`的`autoplay`属性为什么有时不生效？      | 现代浏览器为了用户体验，限制自动播放：1. 需满足 “静音” 条件（muted 属性）；2. 页面需有用户交互（如点击）后才能自动播放。 |
|  44  |           如何在 HTML5 中实现视频的全屏播放？            | 1. 原生控件自带全屏按钮；2. 通过 JS：获取 video 元素，调用`video.requestFullscreen()`（需用户交互触发）。 |
|  45  |       HTML5 支持的音频格式有哪些？视频格式有哪些？       |         音频：MP3、WAV、OGG；视频：MP4、WebM、OGG。          |
|  46  |         `<video>`标签的`poster`属性作用是什么？          |           设置视频未播放时的封面图，提升用户体验。           |
|  47  |            如何隐藏 ``/`<video>`的原生控件？             |        移除`controls`属性，可通过 JS 自定义播放控件。        |
|  48  |         Canvas 是什么？它的核心使用步骤是什么？          | Canvas 是 HTML5 的绘图标签，用于通过 JS 绘制 2D/3D 图形；核心步骤：1. 获取 Canvas 元素；2. 获取绘图上下文（`getContext('2d')`）；3. 调用上下文方法绘图（如 fillRect、arc）。 |
|  49  |                Canvas 和 SVG 有什么区别？                | 1. Canvas：基于像素（位图），绘制后无法修改（需重绘），适合动画、游戏；2. SVG：基于矢量（图形），可修改元素属性，适合静态图形、图标，放大不失真。 |
|  50  |              如何在 Canvas 中绘制一个矩形？              | `js const ctx = document.getElementById('canvas').getContext('2d'); ctx.fillStyle = 'red'; ctx.fillRect(10, 10, 100, 80); // x,y,宽,高 ` |
|  51  |                如何在 Canvas 中绘制圆形？                | `js ctx.beginPath(); ctx.arc(50, 50, 30, 0, Math.PI*2); // 圆心x,y,半径,起始角度,结束角度 ctx.fillStyle = 'blue'; ctx.fill(); ` |
|  52  | Canvas 的`width`/`height`属性和 CSS 设置宽高有什么区别？ | Canvas 自身的 width/height 是绘图分辨率，CSS 宽高是显示尺寸；若仅用 CSS 设置，会导致图形拉伸变形（需保持分辨率和显示尺寸比例）。 |
|  53  |                  如何清空 Canvas 画布？                  | `js ctx.clearRect(0, 0, canvas.width, canvas.height); // 清空整个画布 ` |
|  54  |           HTML5 中`<track>`标签的作用是什么？            | 为`<video>`/`` 添加字幕 / 字幕轨道（如英文字幕、中文字幕），属性`kind="subtitles"`指定轨道类型。 |
|  55  |           如何通过 JS 控制音频的播放 / 暂停？            | `js const audio = document.getElementById('audio'); // 播放 audio.play(); // 暂停 audio.pause(); ` |
|  56  |            Canvas 支持绘制文本吗？如何实现？             | 支持；示例：`js ctx.font = '20px 微软雅黑'; ctx.fillText('Hello HTML5', 10, 50); // 文本内容,x,y ` |
|  57  |                如何在 Canvas 中绘制图片？                | `js const img = new Image(); img.src = 'image.jpg'; img.onload = () => { ctx.drawImage(img, 10, 10); // 图片,x,y }; ` |
|  58  |           HTML5 的音视频标签是否支持跨域播放？           | 默认不支持，需服务器配置 CORS（跨域资源共享）头（如 Access-Control-Allow-Origin）。 |
|  59  |        Canvas 的`getImageData()`方法作用是什么？         | 获取 Canvas 指定区域的像素数据（RGBA 值），可用于图像处理（如滤镜、抠图）。 |
|  60  |                  如何实现 Canvas 动画？                  | 核心：使用`requestAnimationFrame`循环重绘；步骤：1. 清空画布；2. 修改图形位置 / 属性；3. 重新绘制；4. 调用`requestAnimationFrame`重复执行。 |

## 四、HTML5 本地存储与进阶 API 题（61-80 题）

| 题号 |                             问题                             |                           标准答案                           |
| :--: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  61  |          HTML5 提供了哪些本地存储方式？区别是什么？          | 1. localStorage：永久存储（除非手动删除），约 5MB，同源共享，页面刷新 / 关闭后仍存在；2. sessionStorage：会话存储，关闭标签 / 浏览器后删除，约 5MB，同源同会话共享；3. Cookie：存储量小（4KB），可设置过期时间，每次请求携带到服务器。 |
|  62  |            如何使用 localStorage 存储和获取数据？            | 存储：`localStorage.setItem('name', '张三')`；获取：`localStorage.getItem('name')`；删除单个：`localStorage.removeItem('name')`；清空：`localStorage.clear()`。 |
|  63  |            localStorage 能否存储对象？如何实现？             | 不能直接存储对象（会转为字符串`[object Object]`）；需先序列化：`localStorage.setItem('user', JSON.stringify({name: '张三'}))`；获取后反序列化：`JSON.parse(localStorage.getItem('user'))`。 |
|  64  |               localStorage 的存储限制是什么？                | 1. 同源限制（不同域名无法访问）；2. 存储大小约 5MB；3. 仅存储字符串类型；4. 无过期时间（需手动删除）。 |
|  65  |         sessionStorage 和 localStorage 的核心区别？          | sessionStorage 的生命周期是 “会话”（关闭标签 / 浏览器即删除），localStorage 是永久存储（除非手动删除）；两者均为同源限制。 |
|  66  | HTML5 的 Web Storage（localStorage/sessionStorage）和 Cookie 的区别？ | 1. 存储大小：Web Storage 约 5MB，Cookie 约 4KB；2. 传输：Cookie 每次请求携带到服务器，Web Storage 仅在本地；3. 过期：Web Storage 无默认过期（localStorage）/ 会话过期（sessionStorage），Cookie 可设置过期时间；4. 接口：Web Storage 接口更简洁（setItem/getItem）。 |
|  67  |               HTML5 的地理定位 API 如何使用？                | `js navigator.geolocation.getCurrentPosition( (pos) => { console.log(pos.coords.latitude, pos.coords.longitude); // 经纬度 }, (err) => { console.log('定位失败：', err); } ); ` |
|  68  |             地理定位 API 可能失败的原因有哪些？              | 1. 用户拒绝授权；2. 设备无定位功能（如无 GPS）；3. 网络异常；4. 浏览器不支持。 |
|  69  |             什么是 Web Worker？它的作用是什么？              | Web Worker 是 HTML5 的后台 JS 运行环境，允许在主线程外创建子线程；作用：处理大量计算（如数据解析、复杂运算），避免阻塞主线程（页面卡顿）。 |
|  70  |                  Web Worker 的限制有哪些？                   | 1. 不能访问 DOM（document、window）；2. 同源限制（只能加载同域名的 JS 文件）；3. 通信通过`postMessage()`实现；4. 不能使用 alert/confirm。 |
|  71  |                 如何创建和使用 Web Worker？                  | 主线程：`js const worker = new Worker('worker.js'); worker.postMessage({data: 'hello'}); // 发送数据 worker.onmessage = (e) => { console.log('接收：', e.data); }; `；worker.js：`js self.onmessage = (e) => { self.postMessage('收到：' + e.data.data); }; ` |
|  72  |              什么是 WebSocket？它的作用是什么？              | WebSocket 是 HTML5 的实时通信协议，实现客户端与服务器的双向实时通信；作用：替代传统的轮询（定时请求服务器），降低服务器压力，提升实时性（如聊天、弹幕、实时数据展示）。 |
|  73  |              WebSocket 的核心方法和事件有哪些？              | 方法：`send()`（发送数据）、`close()`（关闭连接）；事件：`onopen`（连接成功）、`onmessage`（接收数据）、`onclose`（连接关闭）、`onerror`（连接错误）。 |
|  74  |                  如何创建 WebSocket 连接？                   | `js const ws = new WebSocket('ws://localhost:8080'); ws.onopen = () => { ws.send('连接成功'); }; ws.onmessage = (e) => { console.log('接收：', e.data); }; ` |
|  75  |              HTML5 的拖放 API 核心步骤是什么？               | 1. 设置元素`draggable="true"`；2. 监听拖拽事件：`ondragstart`（开始拖拽）、`ondragover`（拖拽经过）、`ondrop`（释放拖拽）；3. `ondragover`需阻止默认行为（否则 drop 不触发）。 |
|  76  |              什么是 IndexedDB？它的作用是什么？              | IndexedDB 是 HTML5 的本地数据库（NoSQL），存储量更大（无明确限制），支持异步操作；作用：存储大量结构化数据（如离线应用数据），弥补 localStorage 的不足。 |
|  77  |           localStorage 的`storage`事件作用是什么？           | 当同源页面修改 localStorage 时，其他页面会触发`storage`事件，可监听数据变化（如多标签同步数据）。 |
|  78  |      HTML5 的离线应用（Application Cache）作用是什么？       | 允许将页面资源缓存到本地，离线时仍可访问；核心是`manifest`文件（列出需要缓存的资源）。 |
|  79  |  如何检测浏览器是否支持某个 HTML5 特性（如 localStorage）？  | 示例：`js if (typeof localStorage !== 'undefined') { // 支持 } else { // 不支持 } ` |
|  80  |      HTML5 的 History API 有哪些常用方法？作用是什么？       | 方法：`pushState()`（添加历史记录）、`replaceState()`（替换当前历史记录）、`popstate`（监听历史记录变化）；作用：实现无刷新修改 URL（前端路由核心）。 |

## 五、HTML5 实战与场景题（81-100 题）

| 题号 |                             问题                             |                           标准答案                           |
| :--: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  81  |               如何优化 HTML5 页面的加载速度？                | 1. 压缩 HTML/CSS/JS 文件；2. 图片懒加载（`loading="lazy"`）；3. JS 异步加载（defer/async）；4. 利用 localStorage 缓存静态数据；5. 减少 HTTP 请求（合并文件）。 |
|  82  |                 HTML5 中图片懒加载如何实现？                 | 1. 原生方式：`<img src="placeholder.jpg" data-src="real.jpg" loading="lazy">`；2. JS 方式：监听滚动事件，判断图片是否进入视口，若进入则将`data-src`赋值给`src`。 |
|  83  |              如何实现 HTML5 页面的响应式布局？               | 1. 设置`viewport`（`width=device-width`）；2. 使用 CSS 媒体查询（`@media`）；3. 采用弹性布局（Flex）/ 网格布局（Grid）；4. 图片自适应（`max-width: 100%`）。 |
|  84  |          HTML5 页面出现乱码的原因有哪些？如何解决？          | 原因：1. 字符编码不一致（如 HTML 是 UTF-8，服务器返回 GBK）；2. 未设置`<meta charset="UTF-8">`；解决：1. 统一使用 UTF-8 编码；2. 服务器配置响应头`Content-Type: text/html; charset=utf-8`。 |
|  85  |                如何在 HTML5 中实现锚点跳转？                 | 1. 基础方式：`<a href="#section1">跳转</a> <div id="section1"></div>`；2. JS 方式：`document.getElementById('section1').scrollIntoView({behavior: 'smooth'})`（平滑滚动）。 |
|  86  | HTML5 中`<meta>`标签的`description`和`keywords`属性作用是什么？ | `description`：页面描述（搜索引擎结果页显示）；`keywords`：页面关键词；均用于 SEO 优化。 |
|  87  |                      如何禁止页面缩放？                      | 在 viewport 中设置：`<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">`。 |
|  88  |           HTML5 中如何实现自定义复选框 / 单选框？            | 1. 隐藏原生控件（`opacity: 0`）；2. 用 div/span 模拟样式；3. 通过 JS 监听原生控件的`checked`状态，同步模拟控件的样式。 |
|  89  |            如何解决 HTML5 音视频自动播放的限制？             | 1. 添加`muted`属性（静音自动播放）；2. 绑定用户交互事件（如点击），在事件内调用`play()`。 |
|  90  |             HTML5 页面的 SEO 优化有哪些关键点？              | 1. 使用语义化标签（header/nav/article 等）；2. 设置 title/description/keywords；3. img 标签添加 alt 属性；4. 合理使用 h1-h6 标题标签（h1 仅用一次）；5. 减少 iframe/flash（搜索引擎无法解析）。 |
|  91  |                如何在 HTML5 中嵌入 PDF 文件？                | 1. 使用`<embed>`：`<embed src="file.pdf" width="100%" height="600px">`；2. 使用`<iframe>`：`<iframe src="file.pdf" width="100%" height="600px"></iframe>`。 |
|  92  |       什么是 HTML5 的微数据（Microdata）？作用是什么？       | 微数据是用于标记页面结构化数据的语法（如商品信息、人物信息）；作用：帮助搜索引擎理解页面内容，提升搜索结果展示效果（如富文本摘要）。 |
|  93  |              如何检测用户的网络状态（HTML5）？               | 使用`navigator.connection`或`navigator.onLine`：`js // 检测是否在线 if (navigator.onLine) { console.log('在线'); } else { console.log('离线'); } // 监听网络变化 window.addEventListener('online', () => {}); window.addEventListener('offline', () => {}); ` |
|  94  |     HTML5 中`<meta>`标签的`http-equiv`属性有哪些常用值？     | `Content-Type`（设置内容类型）、`X-UA-Compatible`（指定 IE 渲染模式）、`refresh`（页面刷新）。 |
|  95  |               如何实现 HTML5 页面的打印功能？                | 1. 原生方式：`window.print()`；2. 自定义打印样式：`@media print { /* 打印样式 */ }`。 |
|  96  |      HTML5 中`<iframe>`标签的`sandbox`属性作用是什么？       | 限制 iframe 的权限（如禁止脚本、禁止表单提交、禁止跨域），提升安全性；常用值：`sandbox="allow-scripts allow-same-origin"`。 |
|  97  |       如何解决移动端 HTML5 页面点击延迟（300ms）问题？       | 1. 设置 viewport 的`user-scalable=no`；2. 使用 FastClick.js 库；3. 现代浏览器已默认优化（如 Chrome）。 |
|  98  |            HTML5 中`<noscript>`标签的作用是什么？            | 当浏览器禁用 JS 或不支持 JS 时，显示`<noscript>`内的内容（用于降级提示）。 |
|  99  |            如何在 HTML5 中实现复制文本到剪贴板？             | 使用 Clipboard API：`js async function copyText() { await navigator.clipboard.writeText('要复制的文本'); alert('复制成功'); } ` |
| 100  |                   总结 HTML5 的核心优势？                    | 1. 语义化更强，利于 SEO 和维护；2. 原生支持音视频、Canvas，无需第三方插件；3. 丰富的本地存储和进阶 API（WebSocket、Web Worker）；4. 更好的移动端适配；5. 离线应用能力，提升用户体验。 |

## 总结

这份 100 道 HTML5 面试题覆盖了面试核心考点，可归纳为 3 个关键方向：

1. **基础核心**：文档结构、语义化标签、全局属性是所有面试的必问基础，需熟记；
2. **特性增强**：表单、多媒体、Canvas 是 HTML5 区别于 HTML4 的核心，重点掌握使用方式和属性；
3. **进阶应用**：本地存储、Web Worker、WebSocket 等 API 是中高级面试的重点，需理解原理和使用场景。

建议先掌握前 60 题（基础 + 核心特性），再攻克后 40 题（进阶 + 实战），结合代码实操记忆，能快速应对不同层级的 HTML5 面试。