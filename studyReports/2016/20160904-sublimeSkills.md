##2016-9-4 sublime技巧

一、快捷键（限windows）

1、选择类

快捷键

说明

Ctrl+D	选中光标所占的文本，继续操作则会选中下一个相同的文本

Ctrl+K	跳过该次选中

Alt+F3	

一次性选择全部

Ctrl+L	选中整行，继续操作则继续选择下一行

Ctrl+Shift+L	先选中多行，再按下快捷键，会在每行行尾插入光标，即可同时编辑这些行

Ctrl+M	光标移动至括号内结束或开始的位置

Ctrl+Shift+M	选择括号内的内容（继续选择父括号）

Ctrl+Enter	在下一行插入新行，即使光标不在行尾，也能快速向下插入一行

Ctrl+Shift+Enter	在上一行插入新行，即使光标不在行首，也能快速向上插入一行

Ctrl+Shift+[	选中代码，按下快捷键，折叠代码

Ctrl+Shift+]	选中代码，按下快捷键，展开代码

Ctrl+K+0	展开所有折叠代码

Ctrl+←	向左单位性地移动光标

Ctrl+→	向右单位性地移动光标

Shift+←

向左选中文本

Shift+→	向右选中文本

shift+↑	向上选中多行

shift+↓	向下选中多行

Ctrl+Shift+←	向左单位性地选中文本

Ctrl+Shift+→	向右单位性地选中文本

Ctrl+Shift+↑	将光标所在行和上一行代码互换

Ctrl+Shift+↓	将光标所在行和下一行代码互换

Shift+右键拖动	多行相同列插入光标

2、编辑类

快捷键

说明

Ctrl+J 	合并选中的多行代码为一行

Ctrl+Shift+D	复制光标所在整行，插入到下一行

Ctrl+K+K	从光标处开始删除代码至行尾

Ctrl+Shift+K	删除整行

Ctrl+X	删除当前行

Ctrl+K+U 	转换大写

Ctrl+K+L 	转换小写

Ctrl+F2	设置书签

Ctrl+T 	左右字母互换

3、搜索类

快捷键

说明

Ctrl+shift+F 	在文件夹内查找

Ctrl+P	

打开搜索框，说明：

输入当前项目中的文件名，快速搜索文件

输入@和关键字，查找文件中函数名

输入：和数字，跳转到文件中该行代码

输入#和关键字，查找变量名

Ctrl+G	打开搜索框，自动带：，输入数字跳转到该行代码

Ctrl+R 	打开搜索框，自动带@，输入关键字，查找文件中的函数名

Ctrl+：	打开搜索框，自动带#，输入关键字，查找文件中的变量名、属性名等

4、显示类

快捷键

说明

Ctrl+Tab	按文件浏览过的顺序，切换当前窗口的标签页

Ctrl+PageDown	向左切换当前窗口的标签页

Ctrl+PageUp	向右切换当前窗口的标签页

Ctrl+W	关闭当前打开文件

Ctrl+Shift+W	关闭所有打开文件

Alt+数字	切换打开第N个文件

Alt+Shift+数字	窗口分屏，恢复默认N屏

Ctrl+K+B	开启/关闭侧边栏

F11	全屏模式

Shift+F11	免打扰模式

二、常用插件

Emmet：更快更高效地编写HTML和CSS

SideBarEnhancements：增强右键菜单

CSSComb：对CSS属性进行排序

DocBlockr：生成注释范式

Terminal：Sublime版的在当前文件夹内打开，快捷键为Ctrl+Shift+T

SideBarFolders：用来管理文件夹

Compare Side-By-Side：Sublime版本的Beyond Compare

BracketHighlighter：显示我在哪个括号内

TrailingSpaces：高亮显示尾部多余的空格

Javascript-API-Completions：支持Javascript、JQuery、Twitter Bootstrap框架、HTML5标签属性提示的插件

jsFormat：格式化js代码

Autoprefixer：CSS3私有前缀自动补全

HTML-CSS-JS Prettify：格式化（美化）html、css、js三种文件类型

三、基础用户设置

 工具栏 Preferences – Settings-User 加入下面的代码：

"trim_trailing_white_space_on_save": true,

"ensure_newline_at_eof_on_save": true,

"font_face": "Microsoft YaHei Mono",

"disable_tab_abbreviations": true,

"translate_tabs_to_spaces": true,

"tab_size": 2,

"draw_minimap_border": true,

"save_on_focus_lost": true,

"highlight_line": true,

"word_wrap": "true",

"fade_fold_buttons": false,

"bold_folder_labels": true,

"highlight_modified_tabs": true,

"default_line_ending": "unix", 

"auto_find_in_selection": true

说明：

trim_trailing_white_space_on_save：自动移除行尾多余空格

ensure_newline_at_eof_on_save：文件末尾自动保留一个空行

font_face：设置字体

disable_tab_abbreviations：设置为 true ，禁用 Emmet 的 tab 键功能（请使用 ctrl+e）

translate_tabs_to_spaces：把代码 tab 对齐转换为空格对齐，tab_size 配合设置空格数

draw_minimap_border：用于右侧代码预览时给所在区域加上边框，方便识别。

save_on_focus_lost：窗口失焦立即保存文件

highlight_line：当前行高亮

word_wrap：设置自动换行

fade_fold_buttons：默认显示行号右侧的代码段，闭合展开三角号

bold_folder_labels：侧边栏文件夹显示加粗，区别于文件

highlight_modified_tabs：高亮未保存文件

default_line_ending: “unix”：使用 unix 风格的换行符

auto_find_in_selection: true：开启选中范围内搜索（而不是整个文档）
