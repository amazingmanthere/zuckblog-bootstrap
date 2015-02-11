---
layout: post
---

# Sublime Text 2的界面风格配置

Sublime Text 2(简称ST2)是一款非常强悍的文本编辑器，支持非常多的文件格式，连exe文件都可以编辑。  
不过，ST2的默认界面风格实在是太单调了，这点实在不爽。好在ST2强大的插件体系，并且ST2的配置非常灵活，用户可以最大限度定制自己喜欢的界面。

## 基本配置  

首先，从ST2的基本配置入手，配置字体、字号和颜色方案等。Preferences > Settings - User打开ST2的个人配置文件`Preferences.sublime-settings`，并添加以下文本：  

    {
        // Sets the colors used within the text area
        "color_scheme": "Packages/Color Scheme - Default/Blackboard.tmTheme",

        // Font face & size
        "font_face": "Consolas",
        "font_size": 16,

        // Characters that are considered to separate words
        "word_separators": "./\\()\"'-:,.;<>~!@#%^&*|+=[]{}`~?",

        // The number of spaces a tab is considered equal to
        "tab_size": 4,

        // Set to true to insert spaces when tab is pressed
        "translate_tabs_to_spaces": true,

        // OS X only: When files are opened from finder, or by dragging onto the
        // dock icon, this controls if a new window is created or not.
        "open_files_in_new_window": false,

        // Makes tabs with modified files more visible
        "highlight_modified_tabs": true,

        // List any packages to ignore here. When removing entries from this list,
        // a restart may be required if the package contains plugins.
        "ignored_packages":
        [
            "Vintage",
            "Sublime Linter"
        ],
        "line_numbers": false
    }  

然后点击保存。  

+ "color_scheme"：这个字段用来设置ST2的颜色方案，ST2默认提供的颜色主题都在Color Scheme - Default文件夹里面，修改这个字段，或者进入Preferences > Color Scheme选择，就可以设置自己喜欢的颜色方案。  
+ "font_face"： 这个字段用来设置ST2的字体，修改这个字段就可以设置自己喜欢的字体，但前提是电脑上已经安装了该字体。  
+ "word_separators"： "./\\()\"'-:,.;<>~!@#%^&*|+=[]{}`~?"。这个字段用来设置单词的分隔符，比如wo+rd，双击这个词时，wo+rd不是整个都是选中状态，因为word的中间有分隔符+，wo+rd当成三个词处理。  
+ "ignored_packages"： 这个字段用来设置需要忽略的插件，被忽略的插件是无法使用的。
+ "line_numbers"： 这个字段用来设置ST2是否显示行号。

## 指定文件类型配置  

平常使用中，用户需要根据打开的文件类型来配置不同的颜色方案或者主题，这时候，ST2的`Syntax Specific`功能就派上用场了。

### HTML文件配置：  

用ST2打开一个HTML文件，然后进入Preferences > Settings - More > Syntax Specific - User，这时候ST2会在Packages > User文件夹中自动新建一个`HTML.sublime-settings`文件。添加以下文本：

    {
        // Sets the colors used within the text area
        "color_scheme": "Packages/Color Scheme - Default/Blackboard.tmTheme",

        // The number of spaces a tab is considered equal to
        "tab_size": 2
    }

然后点击保存。
这样，ST2打开HTML文件时，编辑界面就会根据上面文本指定的配置变化。  同理，CSS等文件的配置方法跟HTML文件的基本一样。

### Markdown文件配置：  

用ST2打开一个后缀名为.md的文件，然后进入Preferences > Settings - More > Syntax Specific - User，这时候ST2会在Packages > User文件夹中自动新建一个`Markdown.sublime-settings`文件。添加以下文本：

    {
        // Which file extensions go with this file type?
        "extensions":
        [
            "md",
            "mdown",
            "mdwn",
            "mmd",
            "txt"
        ], 

        // Sets the colors used within the text area
        "color_scheme": "Packages/Color Scheme - Default/Sunburst.tmTheme",

        // Set to true to removing trailing white space on save
        "trim_trailing_white_space_on_save": false
    }

然后点击保存。
这样，ST2打开Markdown文件时，编辑界面就会根据上面文本指定的配置变化。由于Markdown文件是没有扩展集的，用户需要自己制定扩展名称，比如：  

        "extensions":
        [
            "md",
            "mdown",
            "mdwn",
            "mmd",
            "txt"
        ]

用ST2打开的文件，如果后缀名为上面的其中一个，那么ST2就会应用Markdown.sublime-settings文件指定的配置。另外，ST2默认支持Markdown文件高亮显示的颜色方案有：

    Packages/Color Scheme - Default/Dawn.tmTheme
    Packages/Color Scheme - Default/Sunburst.tmTheme
    Packages/Color Scheme - Default/Cobalt.tmTheme

建议设置Markdown文件的颜色方案为上面的其中一个。同理，MultiMarkdown文件的配置方跟Markdown文件的基本一样。  

## 使用插件  

得益于ST2强大的插件体系，用户可以安装各式各样的插件来满足自己的需求，让ST2的界面变得更加绚丽多彩。  

### 安装方法：  

安装并打开Package Control插件，参照[这里](http://zuckchen.com/post/2013/06/01/Sublime-Text-2%E7%9A%84%E5%AD%A6%E4%B9%A0%E4%B8%8E%E4%BD%BF%E7%94%A8.aspx)。快捷键ctrl + shite + P打开命令面板（Tools > Command Palette），输入插件名称并回车开始安装。

### 应用插件的颜色方案和主题：  

Preferences > Settings - User打开ST2的个人配置文件，添加修改"color_scheme"和"theme"字段。比如使用FlatLand插件的颜色方案和主题：  

    "color_scheme": "Packages/Theme - Flatland/Flatland Dark.tmTheme",
    "theme": "Flatland Dark.sublime-theme"

### 插件推荐：  

    FlatLand  
    Nil  

## 手动DIY  

用户不仅可以通过各种各样的插件来丰富ST2的界面，还可以自己手动修改颜色方案和主题。  

### 手动修改颜色方案：  

1. 进入Preferences > Browser Package，找到当前ST2应用的颜色方案配置文件，颜色方案的配置文件后缀名为.tmTheme。
2. 使用ST2打开配置文件，打开的内容如：

        <dict>
            <key>name</key>
            <string>Keyword</string>
            <key>scope</key>
            <string>keyword</string>
            <key>settings</key>
            <dict>
                <key>fontStyle</key>
                <string></string>
                <key>foreground</key>
                <string>#fa9a4b</string>
            </dict>
        </dict>

key用于标识属性，string用于标识值，dict用于嵌套多个key和string。比如上面配置的名称是Keyword，配置的范围是Keyword，配置的详细信息有字体风格和前景色。这样就可以根据自己的需要手动修改里面相应的字段了。另外，还可以在线编辑.tmTheme文件，点[这里](http://tmtheme-editor.herokuapp.com/)。

### 手动修改主题：  

1. 进入Preferences > Browser Package，找到当前ST2应用的主题配置文件，主题的配置文件后缀名为.sublime-theme。  

2. 使用ST2打开配置文件，打开的内容如：  

        {
            "class": "icon_button_control",
            "layer0.texture": "Theme - Flatland/Flatland Dark/btn-group-middle.png",
            "layer0.opacity": 1.0,
            "content_margin": [4, 4,4,5]
        }

主题的配置写法跟CSS非常相似。可以尝试根据各个字段的信息去揣测修改字段的值，或者直接到主题的图片文件夹里面，直接替换相应的图片文件。

## 参考  

1. <http://www.granneman.com/webdev/editors/sublime-text/configuring-sublime-text/>