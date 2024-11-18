---
title: Blog Configuration on Icarus Theme
date: 2023-08-27 21:47:59
toc: true
tags:
	- Hexo
categories: blog
excerpt: This is an article about blog configuration on icarus theme.
---
## Blog Configuration On Github

### 1. Setup

+ initialize Hexo project in the target `<folder>`

```apl
$ hexo init <folder>
$ cd <folder>
```

### 2. Install Icarus Theme

+ To install Icarus as a node package via NPM, run the following command from the root of your Hexo site:

```apl
npm install -S hexo-theme-icarus hexo-renderer-inferno
```

+ Use the `hexo` command to change the theme to Icarus:

```apl
hexo config theme icarus
```

### 3. Theme Configuration

> Icarus’ default theme configuration file is _config.icarus.yml.

#### 3.1 Overall style configuration

##### Version

This version of the theme configuration file is not advised to change by yourself, determining whether to upgrade the theme configuration.

```apl
version: 5.1.0
```

#### Theme Variant

`default` and `cyberpunk` determine the skin of Icarus theme. `default` has always been suggested if you want to make a acadmic style blog.

```apl
variant: default
```

#### Logo

The logo of your site will display on the navigation bar and the footer. The value of the logo can either be the path or URL to your logo image:

```apl
logo: https://ms-blogimage.oss-cn-chengdu.aliyuncs.com/picture/img/EBlog202306222351609.png
```

#### Favicon

Set a icon for your blog website in the form of URL or path to the website's icon.

```apl
head:
    favicon: https://ms-blogimage.oss-cn-chengdu.aliyuncs.com/picture/img/EBlog202306222351609.png
```

#### Navigation Bar

The navbar section defines the menu items and links in the navigation bar.

+ You may put any menu item in the navigation bar by adding <link_name>: <link_url> to the menu setting.
+ To put links on the right side of the navigation bar, add <link_name>: <link_url> to the links setting.

```apl
navbar:
    # Navigation menu items
    menu:
        Zhihao Li's Blog: /
        Archives: /archives
        Categories: /categories
        Tags: /tags
        About: /about
    # Links to be shown on the right of the navigation bar
    links:
        GitHub:
            icon: fab fa-github
            url: https://github.com/LZHMS/LZHMS.github.io
```

#### Footer

The footer section defines the links on the right side of the page footer. The link format is exactly the same as links in the navbar section.

```apl
footer:
    # Copyright text
    copyright: © 2023 Zhihao Li
    # Links to be shown on the right of the footer section
    links:
        Creative Commons:
            icon: fab fa-creative-commons
            url: https://creativecommons.org/
        Attribution 4.0 International:
            icon: fab fa-creative-commons-by
            url: https://creativecommons.org/licenses/by/4.0/
        GitHub:
            icon: fab fa-github
            url: https://github.com/LZHMS/LZHMS.github.io
```

### 3.2 Article configurtion

#### Code Highlight

+ You can choose a theme from all themes listed under `highlight.js/src/styles` to customize the code blocks. Copy the file name (without the .css extension) to the theme setting.
+ To hide the "copy" button of every code block, set `clipboard` to `false`.
+ If you wish to fold or unfold all code blocks, set the `fold` setting to `folded` or `unfolded`.

```apl
   highlight:
        # Code highlight themes
        # https://github.com/highlightjs/highlight.js/tree/master/src/styles
        theme: atom-one-light
        # Show copy code button
        clipboard: true
        # Default folding status of the code blocks. Can be "", "folded", "unfolded"
        fold: unfolded
```

#### Read Time

You can show a word counter and the estimated reading time of your article above the article title by setting `readtime` to `true` in the `article` section.

```apl
article:
	readtime: true
```

### Update Time

set `update_time` to `true` in the `article` section of your theme configuration file to show every article updated time.

```apl
article:
    update_time: true
```

### Article Licensing

You can show a section at the end of your posts/pages describing the licensing of your work. Both text and icons are accepted as license links.

```apl
article:
    # Article licensing block
    licenses:
        Creative Commons:
            icon: fab fa-creative-commons
            url: https://creativecommons.org/
        Attribution:
            icon: fab fa-creative-commons-by
            url: https://creativecommons.org/licenses/by/4.0/
        Noncommercial:
            icon: fab fa-creative-commons-nc
            url: https://creativecommons.org/licenses/by-nc/4.0/
```

#### Sidebar

To make a sidebar fixed when you scroll the page, set the sticky setting of that sidebar to true in the sidebar section.

```apl
sidebar:
    # Left sidebar configurations
    left:
        # Whether the sidebar sticks to the top when page scrolls
        sticky: false
    # Right sidebar configurations
    right:
        # Whether the sidebar sticks to the top when page scrolls
        sticky: true
```

### 3.3 Widgets Configuration

#### Profile Widget

+ Set multiple `author_title` and display by rows and set font-family in `hexo-theme-icarus\layout\widget\profile.jsx`

```profile.jsx
{author ? <p class="title is-size-4 is-block" style="line-height: 'inherit'; font-family: Times New Roman">{author}</p> : null}
{authorTitle ? <p style="white-space: pre-line; font-style: italic; font-family: Times New Roman; margin-bottom: 0.50rem; font-size: 1.0em">{authorTitle}</p> : null}
{location ? <p class="is-size-5 is-flex justify-content-center" style="font-family: Times New Roman">}
```

```_config.icarus.yml
    # Author title
    author_title: |
        Computer Science
        Machine Learning
```

+ Set `social_links` using Font Awesome Icons

```apl
social_links:
    Github:
        icon: fab fa-github
        url: https://github.com/LZHMS
    Facebook:
        icon: fab fa-facebook
        url: https://www.facebook.com/profile.php?id=100094074308733
    Twitter:
        icon: fab fa-twitter
        url: https://twitter.com/ZhihaoLi1376106
    Email:
        icon: fa-solid fa-envelope
        url: mailto:LZH1314521ligao@163.com
    QQ:
        icon: fab fa-qq
        url: https://w.4rxb.com/s/yq3hxp
```

### 4. Configure Home Page on the Site

In the normal case, the default home page includes some abstacts of blogs. However, in some case we want to to display our information or introduction about the website. That's the time when we need to individually configure the home page.

+ Create a `index.md` anticle under the `source` directory
  This is an anticle used for our individual content to be display on the home page.
+ Modify `index_generator` in the file of `_config.yml`
  We need to modify the `index_generator:path` to an invalid value for example `default-index` in order to shield the default home page.
+ Add the `home` page to the website
  In the configuration file of theme, we can add an item of `home` under `menu` item. And then set the `home` value like `/ || fa fa-home` if we need an icon for the home page.

### 5. Open the Gallery

If we want to display multiple pictures in the gallery, we can use the following code to open the gallery.

```
<div class="justified-gallery">
</div>
```

## 主题魔改
### 自定义 ICON 图标
Hexo + Icarus 采用 FontAwesome 图标，但很多图标实际上并未包含，包括国内的 <i class="iconfont icon-bilibili"></i>、<i class="iconfont icon-zhihu"></i> 以及 <i class="iconfont icon-csdn"></i> 以及国外的 <i class="iconfont icon-huggingface"></i>，尤其是我们想自定义地引入一些 icon 的话也不能够实现，所以我们需要让 hexo 添加对这些网站图标的支持，使博客正常显示他们图标。

+ Step 1: 下载图标
最开始的准备是收集所需的图标，最常用的网站库还是阿里的矢量库 [Iconfont](https://www.iconfont.cn/)，搜索选择所需的图标，不断添加到购物车。
选购完毕后，再选择 Add to Project，让选择的图标添加到你的一个项目中。

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/20241102104541.png" alt="导出图标项目" width="70%"/>

在项目中，选择 `Font class`，然后`下载至本地`即可，将其保存至主题目录`themes\icarus\source\css` 文件夹中，解压并重新命名为 `icons`。如果想对下载的图标的颜色、大小等进行修改，则打开所下载的文件，找到对应项进行修改。

+ Step 2: 配置图标
我们需要在主题中引入所下载的图标库，在主题目录下 `themes\icarus\layout\common\head.jsxw` 文件中，在 `<link rel="stylesheet" href={iconcdn()} />` 下方添加 `<link rel="stylesheet" href="/css/icons/iconfont.css"></link>`，添加后如下所示:
```js
    <link rel="stylesheet" href={iconcdn()} />
    <link rel="stylesheet" href="/css/icons/iconfont.css"></link>   // add the iconfont
```
然后需要具体添加图标，主要在于图标 icon 的大小位置配置需要与 icarus 主题原生的 icon 相容，所以首先需要统一配置引入的 icon 的大小，具体在 `themes\icarus\source\css\icons\iconfont.css` 中配置大小为 24px，如下所示:
```css
.iconfont {
  font-family: "iconfont" !important;
  font-size: 24px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```
统一配置的大小全局有效，但是具体图标下面配置后，全局配置就会被覆盖，所以对于导航栏的图标，需要额外配置的参数如下，
```css
.icon-github:before {
  content: "\ea0b";
  font-size: 14px;
  display: flex;
  align-items: center;
  margin: 0;
}
```
对于 `Profile` 的图标也需要进一步地调节大小，配置的参数可以参考如下：
```css
.icon-QQ1:before {
  content: "\e667";
  font-size: 20px;
}
```
{% message color:warning %}
仍然存在的问题是，对于白天和黑夜两种主题模式，引入的 icon 图标无法自动切换黑夜模式，只保持一种样式。
{% endmessage %}

+ Step 3: 使用图标
    + 使用图标主要是在配置文件和博客文章两个地方，在博客配置中，直接配置相应的icon即可，`iconfont icon-xxx`;
    + 在博客文章中，需要引入 `<i class="iconfont icon-bilibili"></i>` 即可；
    + 如果需要在引用时配置图标大小，可以这样指定：`<i class="iconfont icon-github7" style="font-size: 20px;"></i>`。

### 引入霞鹜文楷中文字体
Icarus 主题默认使用谷歌的 cdn，因此字体样式只能从其中选择，引入霞鹜文楷中文字体首先需要制作一个字体的 cdn 用于引用，这个可以直接利用项目 [lxgw-wenkai-screen-webfont](https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-webfont@1.7.0/) 中的 cdn，在博客配置文件中引入即可。

+ Step 1: 引入字体 CDN
在主题目录 `themes\icarus\layout\common\head.jsx` 文件中，找到 `const fontCssUrl `，然后将其修改为：
```js
const fontCssUrl = {
    default: 'https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-webfont@1.7.0/style.css',
    // default: fontcdn('Ubuntu:wght@400;600&family=Source+Code+Pro', 'css2'),
    //cyberpunk: fontcdn('Oxanium:wght@300;400;600&family=Roboto+Mono', 'css2')
};
```
霞鹜文楷中文字体的 CDN 有许多版本，可以选择最新的配置。

+ Step 2: 配置字体
在主题目录 `themes\icarus\include\style\base.styl` 文件中，找到 `$family-sans-serif`，然后将其修改为：
```css
$family-sans-serif ?= Times New Roman, LXGW WenKai Screen /* serif  Georgia */
$family-code ?= LXGW WenKai Screen, Cambria, 'Source Code Pro', monospace, 'Microsoft YaHei'
```
其中 `Times New Roman` 首先匹配英文字体，无法匹配中文字体，然后会找到第二个字体样式 `LXGW WenKai Screen` 匹配中文，这样就可以在 icarus 主题中引入霞鹜文楷字体了。
