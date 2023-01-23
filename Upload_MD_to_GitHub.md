# Upload MD to GitHub

---

## Initialization with docsify

- 选择一个路径`MD_page`，初始化项目`docsify init ./MD_page` 或在`cd`到`MD_page`下后，使用`docsify init ./`，作为本地存档  

- 初始化完成后可以进行本地预览`docsify serve MD_page` 或 `docsify serve ./`， 使用网址http://localhost:3000进行预览

```Terminal
Initiation on PowerShell

PS C:\WINDOWS\system32> cd D:\Learning_BioInfo\MD_page
PS D:\Learning_BioInfo\MD_page> docsify init ./
./ already exists.
√ Are you sure you want to rewrite it? (y/N) true

Initialization succeeded! Please run docsify serve ./

PS D:\Learning_BioInfo\MD_page>
PS D:\Learning_BioInfo\MD_page> docsify serve ./

Serving D:\Learning_BioInfo\MD_page now.
Listening at http://localhost:3000
```

- 初始化成功后可以在文件夹下看到三个新建文件
  
  - `index.html`  入口文件
  
  - `README.md`  readme文件，会作为主页内容渲染，初始化显示“An awesome project”
  
  - `.nojeky11`  用于阻止GitHub Page忽略下划线开头的文件
  
  - 直接编辑`README.md`可以更新文档内容，也可以添加更多页面

## Writing more pages

- 如果需要添加更多的页面，则在`MD_page`的文件夹下添加其它`name.md`文件

- For example, the directory structure and matching routes are as follow:

```
-- MD_page
    |-README.md            >>http://domain.com
    |-guide.md             >>http://domain.com/#/guide
    --Markdown
        |-README.md        >>http://domain.com/#/Markdown/
        |-guide.md         >>http://domain.com/#/guide
```

## Sidebar

- Step 1: 在`index.html`中设置loadSidebar，设置为`true`则启用侧边栏
  
  具体方法为在`script`板块添加代码`loadSidebar: true`

```
  <script>
    window.$docsify = {
      name: '',
      repo: '',
      loadSidebar: true
    }
  </script>
```

- Step 2: 新建一个md文件`_sidebar.md`，注意需要有`.nojeky11` 才可以被GitHub Pages识别，否则以下划线开头的文件会被忽略。
  
  `_sidebar.md`文件内容示例如下：

```markdown
<!-- docs/_sidebar.md -->

* [Home](/)
* [Guide](guide.md)
```

        链接括号中`guide.md`可以省略`.md`

- Step 2*: 创建多级侧边栏

```markdown
<!-- docs/_sidebar.md -->

* [Home](/)
* [Guide](guide.md)

* Type 1
    * [subtype a](Type_1/subtype_a/subtype_a.md)
    * [subtype b](Type_1/subtype_b/subtype_b.md)
* Type 2
    * [subtype a](Type_2/subtype_a/subtype_a.md)
    * [subtype b](Type_2/subtype_b/subtype_b.md)
```

    对应文件结构

```
-- docs
    |-README.md
    |-guide.md
    --Type_1
        |-subtype_a.md
        |-subtype_b.md
    --Type_2
        |-subtype_a.md
        |-subtype_b.md
```

- Step 2**：Table of contents: 显示目录，即可以把文章里的二级标题显示在侧边栏里

        具体方法为在`script`板块中设置`subMaxLevel`，`2`表示显示二级标题

```
  <script>
    window.$docsify = {
      name: '',
      repo: '',
      loadSidebar: true,
      subMaxLevel: 2
    }
  </script>
```

## Publish on GitHub Pages

[Github Pages entry](https://pages.github.com)
