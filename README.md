# canvas-nest.js
对 [canvas-nest.js](https://github.com/hustcc/canvas-nest.js.git) 的修改，支持 ES6-Module 导入并指定渲染目标和样式参数，主要是为了方便引入 [Material for MkDocs](https://github.com/squidfunk/mkdocs-material.git) 而改写

## 引入：

下载 `canvas-nest.js` 并放入 `docs/assets/js/` 中

在需要插入效果的页面插入以下目标元素：

```html
<div id="cnest" style="width:100vw;height:100vh;position:fixed;top:0;left:0;z-index: 0;pointer-events: none;"></div>
```

其中目标元素样式可按需求自定义。

接下来在 overrides 中（需要在 `mkdocs.yml` 中设置 `theme.custom_dir: overrides`）创建一个继承 `base` 模板的模板，将以下脚本引入到 `extrahead` 块中：

```html
{% extends "base.html" %}

{% block extrahead %}
<script type="module">
    import {createCanvasNest} from '/assets/js/canvas-nest.js';
    document.addEventListener('DOMContentLoaded', function () {
        document$.subscribe(({ body }) => { 
            createCanvasNest("cnest", {
                zIndex: 0,
                lineColor: "180,180,180",
                lineOpacity: 0.8,
                pointColor: "180,180,180",
                pointOpacity: 0.9,
                count: 75
            });
        })
    });
</script>
{% endblock %}
```

此时在需要渲染的页面的 `yaml` 头中添加使用新增的模板，或选择命名为 `main.html` 覆盖默认模版，刷新页面，可以看到效果在开启 theme.features.instant` 功能时切换页面部分渲染时也能正确应用。

## 备注

当然，这个项目也可以使用在别的地方，只需要在模块中引入并调用相应函数渲染到指定位置 id 的元素上即可。
