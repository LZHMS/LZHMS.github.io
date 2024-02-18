---
title: EBlog Configuration on Icarus Theme
date: 2023-08-27 21:47:59
toc: true
tags:
	- Hexo
categories: blog
excerpt: This is an article about EBlog Configuration on Icarus Theme.
---
### Blog Configuration On Github

#### 1. Setup

+ initialize Hexo project in the target `<folder>`

```apl
$ hexo init <folder>
$ cd <folder>
```

#### 2. Install Icarus Theme

+ To install Icarus as a node package via NPM, run the following command from the root of your Hexo site:

```apl
npm install -S hexo-theme-icarus hexo-renderer-inferno
```

+ Use the `hexo` command to change the theme to Icarus:

```apl
hexo config theme icarus
```

#### 3. Theme Configuration

> Icarus’ default theme configuration file is _config.icarus.yml.

##### 3.1 Overall style configuration

###### Version

This version of the theme configuration file is not advised to change by yourself, determining whether to upgrade the theme configuration.

```apl
version: 5.1.0
```

##### Theme Variant

`default` and `cyberpunk` determine the skin of Icarus theme. `default` has always been suggested if you want to make a acadmic style blog.

```apl
variant: default
```

##### Logo

The logo of your site will display on the navigation bar and the footer. The value of the logo can either be the path or URL to your logo image:

```apl
logo: https://ms-blogimage.oss-cn-chengdu.aliyuncs.com/picture/img/EBlog202306222351609.png
```

##### Favicon

Set a icon for your blog website in the form of URL or path to the website's icon.

```apl
head:
    favicon: https://ms-blogimage.oss-cn-chengdu.aliyuncs.com/picture/img/EBlog202306222351609.png
```

##### Navigation Bar

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

##### Footer

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

#### 3.2 Article configurtion

##### Code Highlight

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

##### Read Time

You can show a word counter and the estimated reading time of your article above the article title by setting `readtime` to `true` in the `article` section.

```apl
article:
	readtime: true
```

#### Update Time

set `update_time` to `true` in the `article` section of your theme configuration file to show every article updated time.

```apl
article:
    update_time: true
```

#### Article Licensing

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

##### Sidebar

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

#### 3.3 Widgets Configuration

##### Profile Widget

+ Set multiple `author_title` and display by rows and set font-family in `hexo-theme-icarus\layout\widget\profile.jsx`

```profile.jsx
{author ? <p class="title is-size-4 is-block" style="line-height: 'inherit'; font-family: Times New Roman">{author}</p> : null}
{authorTitle ? <p style="white-space: pre-line; font-style: italic; font-family: Times New Roman; margin-bottom: 0.50rem; font-size: 1.0em">{authorTitle}</p> : null}
{location ? <p class="is-size-5 is-flex justify-content-center" style="font-family: Times New Roman">
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

#### 4. Configure Home Page on the Site

In the normal case, the default home page includes some abstacts of blogs. However, in some case we want to to display our information or introduction about the website. That's the time when we need to individually configure the home page.

+ Create a `index.md` anticle under the `source` directory
  This is an anticle used for our individual content to be display on the home page.
+ Modify `index_generator` in the file of `_config.yml`
  We need to modify the `index_generator:path` to an invalid value for example `default-index` in order to shield the default home page.
+ Add the `home` page to the website
  In the configuration file of theme, we can add an item of `home` under `menu` item. And then set the `home` value like `/ || fa fa-home` if we need an icon for the home page.

#### 5. Open the Gallery

If we want to display multiple pictures in the gallery, we can use the following code to open the gallery.

```
<div class="justified-gallery">
</div>
```
