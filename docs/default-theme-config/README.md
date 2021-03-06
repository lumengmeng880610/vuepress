---
sidebar: auto
---

# 默认主题配置（Default Theme Config）

::: tip 提示
此页面上列出的所有选项仅适用于默认主题。如果你使用的是自定义主题，则选项可能会有所不同。
:::

## 主页（Homepage）

默认主题提供了一个主页布局（用于[该网站的主页](/)）。要使用它，需要在你的根目录 `README.md` 的 YAML 前端中指定 `home：true` 加上一些其他元数据。这是本网站使用的实际数据：

``` yaml
---
home: true
heroImage: /hero.png
actionText: Get Started →
actionLink: /guide/
features:
- title: Simplicity First
  details: Minimal setup with markdown-centered project structure helps you focus on writing.
- title: Vue-Powered
  details: Enjoy the dev experience of Vue + webpack, use Vue components in markdown, and develop custom themes with Vue.
- title: Performant
  details: VuePress generates pre-rendered static HTML for each page, and runs as an SPA once a page is loaded.
footer: MIT Licensed | Copyright © 2018-present Evan You
---
```

前面的内容之后的任意其他内容将被解析为正常 markdown 并在特性部分之后渲染。

## 导航链接（Navbar Links）

你可以通过 `themeConfig.nav` 将链接添加到导航栏中：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    nav: [
      { text: 'Home', link: '/' },
      { text: 'Guide', link: '/guide/' },
      { text: 'External', link: 'https://google.com' },
    ]
  }
}
```

## 侧边栏（Sidebar）

要启用侧边栏，请使用 `themeConfig.sidebar`。基本配置需要一系列链接：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: [
      '/',
      '/page-a',
      ['/page-b', 'Explicit link text']
    ]
  }
}
```

你可以省略 `.md` 扩展名，以 `/` 结尾的路径被推断为 `*/README.md` 。该链接的文本是自动推断的（从页面的第一个标题或 YAML 前端中的显式标题）。如果你希望明确指定链接文本，请使用 `[link,text]` 形式的数组。

### 嵌套标题链接（Nested Header Links）

侧边栏自动显示当前激活页面中标题的链接，嵌套在页面本身的链接下。你可以使用 `themeConfig.sidebarDepth` 自定义此行为。默认深度是 `1`，它提取 `h2` 标题。将其设置为 `0` 将禁用标题链接，最大值为`2`，同时提取 `h2` 和 `h3` 标题。

页面也可以在使用 YAML 前端时覆盖此值：

``` md
---
sidebarDepth: 2
---
```

### 侧边栏组（Sidebar Groups）

你可以使用对象将侧边栏链接分成多个组：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: [
      {
        title: 'Group 1',
        collapsable: false,
        children: [
          '/'
        ]
      },
      {
        title: 'Group 2',
        children: [ /* ... */ ]
      }
    ]
  }
}
```

侧边栏组默认情况下是可折叠的。你可以强制一个组始终以 `collapsable：false` 打开。

### 多个侧边栏（Multiple Sidebars）

如果你希望为不同的页面组显示不同的侧边栏，请先将页面组织到目录中：

```
.
├─ README.md
├─ foo
│  ├─ README.md
│  ├─ one.md
│  └─ two.md
└─ bar
   ├─ README.md
   ├─ three.md
   └─ four.md
```

然后，使用以下侧边栏配置：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: {
      // 侧边栏在 /foo/ 上
      '/foo/': [
        '',
        'one',
        'two'
      ],
      // 侧边栏在 /bar/ 上
      '/bar/': [
        '',
        'three',
        'four'
      ]
    }
  }
}
```

### 单页自动补充工具栏（Auto Sidebar for Single Pages）

如果你希望自动生成仅包含当前页面的标题链接的侧边栏，则可以在该页面上使用 YAML 前端：

``` yaml
---
sidebar: auto
---
```

### 禁用侧边栏（Disabling the Sidebar）

你可以使用 YAML 前端禁用特定页面上的侧边栏：

``` yaml
---
sidebar: false
---
```

## 上一页/下一页链接（Prev / Next Links）

根据激活页面的侧边栏顺序自动推断上一个和下一个链接。你也可以使用 YAML 前端来显式覆盖或禁用它们：

``` yaml
---
prev: ./some-other-page
next: false
---
```

## GitHub 仓库和编辑链接

提供 `themeConfig.repo` 会在导航栏中自动生成一个 GitHub 链接，并在每个页面的底部显示「编辑此页面」链接。

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    // 假定 GitHub。也可以是一个完整的 GitLab 网址
    repo: 'vuejs/vuepress',
    // 如果你的文档不在仓库的根部
    docsDir: 'docs',
    // 可选，默认为 master
    docsBranch: 'master',
    // 默认为 true，设置为 false 来禁用
    editLinks: true
  }
}
```

## 简单的 CSS 覆盖

如果你希望对默认主题的样式应用简单覆盖，则可以创建一个 `.vuepress/override.styl` 文件。 这是 [Stylus](http://stylus-lang.com/) 文件，但你也可以使用普通的 CSS 语法。

有几个颜色变量可以调整：

``` stylus
// 显示默认值
$accentColor = #3eaf7c
$textColor = #2c3e50
$borderColor = #eaecef
$codeBgColor = #282c34
```

## 特定页面的自定义布局（Custom Layout for Specific Pages）

默认情况下，每个 `*.md` 文件的内容都会显示在一个 `<div class =“page”>` 容器中，以及侧边栏，自动生成的编辑链接和 prev/next 链接。如果你希望使用完全自定义的组件代替页面（同时只保留导航栏），则可以使用 YAML 前端再次指定要使用的组件：

``` yaml
---
layout: SpecialLayout
---
```

这将为给定页面渲染 `.vuepress/components/SpecialLayout/vue`。
