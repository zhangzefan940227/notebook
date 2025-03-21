" ================================================================================================
" 🍰🍰🍰 Extensions 🍰🍰🍰
" ================================================================================================
"这两个插件，只有写了↓，idea会自动下载：
Plug 'preservim/nerdtree'
Plug 'easymotion/vim-easymotion'
Plug 'tpope/vim-surround'
Plug 'terryma/vim-multiple-cursors'

"下列插件需要在IDEA中下载:
"ideaVim
"IdeaVim-EasyMotion
"IdeaVimExtension
"which-key
"CodeGlance Pro
"Translation
"Idea-QuickScope

" ================================================================================================
" 🐧🐧🐧 Basic settings 🐧🐧🐧
" ================================================================================================

set easymotion
"设置在光标距离窗口顶部或底部一定行数时，开始滚动屏幕内容的行为
set scrolloff=10

set history=200
set clipboard+=unnamed

"--递增搜索功能：在执行搜索（使用 / 或 ? 命令）时，
"Vim 会在您输入搜索模式的过程中逐步匹配并高亮显示匹配的文本。
set incsearch

"--在搜索时忽略大小写
set ignorecase

"--将搜索匹配的文本高亮显示
set hlsearch

"--设置相对行号 和 当前行的绝对行号
set number

"--设置返回normal模式时回到英文输入法
set keep-english-in-normal

set surround
set ReplaceWithRegister
set NERDTree
set quickscope
set noerrorbells
set visualbell

"--将 jj 和 jk 映射为 <Esc>
"jj和jk为主流配置，可按喜好自行调整
imap jk <Esc>
imap jj <Esc>

" ================================================================================================
" 🌍🌍🌍 No Leader Keymaps 🌍🌍🌍
" ================================================================================================
"--普通模式下使用回车键，向下/向上 增加一行
nmap <CR> o<Esc>
nmap <S-Enter> O<Esc>

"--在普通和插入模式下，向下交换行/向上交换行
nnoremap <A-j> :m +1<CR>
nnoremap <A-k> :m -2<CR>
inoremap <A-j> <Esc> :m +1<CR>gi
inoremap <A-k> <Esc> :m -2<CR>gi
xnoremap <A-j> :m '>+1<cr>gv=gv
xnoremap <A-k> :m '<-2<cr>gv=gv

"--格式化（规范化）文本，即对选定的文本进行换行或重排，适应指定的文本宽度。

"全文规范化：Ctrl+Alt+l
map Q gq
"跳转到下一个错误或警告
nmap ge <action>(GotoNextError)
let g:WhichKeyDesc_GotoNextError = "ge 跳转到下一个错误或警告"
"在源代码和测试代码之间快速切换
nmap gt <action>(GotoTest)
let g:WhichKeyDesc_GotoTest = "gt 在源代码和测试代码之间快速切换"
"将光标移动到上一个方法的声明处
" last changed in current buffer(file)
nmap gm <action>(MethodUp)
let g:WhichKeyDesc_MethodUp = "gm 将光标移动到上一个方法的声明处"
"跳转到当前接口或抽象类的实现处
nmap ga <action>(GotoImplementation)
let g:WhichKeyDesc_GotoImplementation = "ga 跳转到当前接口或抽象类的实现处"


" bookmark 切换书签
nmap ma <action>(ToggleBookmark)
let g:WhichKeyDesc_bookmark = "ma 书签"

"切换标签页
nmap L <action>(NextTab)
nmap H <action>(PreviousTab)

"代码跳转
nmap J <action>(Back)
nmap K <action>(Forward)

"将Ctrl + s 映射为保存文档(也可以在VIM设置里将此快捷键设置为IDEA的快捷键)
nmap <C-S> <action>(SaveAll)
imap <C-S> <Esc><action>(SaveAll)

"alt+h/l 光标快速到行首或者行尾)
inoremap <A-h> ^
inoremap <A-l> <End>
nnoremap <A-h> ^
nnoremap <A-l> <End>
vnoremap <A-h> ^
vnoremap <A-l> <End>

"取消撤回
nnoremap U <C-r>

" 映射 Ctrl + h/j/k/l 切换窗口
nmap <C-h> <C-w>h
nmap <C-j> <C-w>j
nmap <C-k> <C-w>k
nmap <C-l> <C-w>l

"复制粘贴,都使用系统剪切板
nnoremap dd "+dd
vnoremap d "+d
nnoremap p "+p
vnoremap p "+p
nnoremap yy "+yy
vnoremap y "+y

" ================================================================================================
" ⭐️⭐️⭐️ Leader Keymaps ⭐️⭐️⭐️ =====================================
" ================================================================================================
"--将<leader>设置为 空格 键
"可自行更改，只需更改双引号内的内容即可
"推荐<leader>:  "空格"  ";"  "\"  "-"  ","
let mapleader = " "

" ================================================================================================
" 👻👻👻 Which-Key 👻👻👻
" ================================================================================================

"which-key的官方推荐配置
set which-key
set notimeout

" ================================================================================================
" 🌟🌟🌟 <leader>详细配置 🌟🌟🌟
" ================================================================================================

"========= NULL ========
"这一行为在按下<leader>后显示的,甭管就行
let g:WhichKeyDesc_LeaderKeymap= "【紫色为二级目录】"

"========== a ==========


"========== b ==========


"========== c ==========
let g:WhichKeyDesc_CodeAndClose = "<leader>c 关闭窗口"

"关闭所有标签页
let g:WhichKeyDesc_CodeAndClose_CloseAllEditors = "<leader>ca 关闭所有标签页"
nmap <leader>ca <action>(CloseAllEditors)

"关闭当前标签页
let g:WhichKeyDesc_CodeAndClose_CloseEditor = "<leader>cc 关闭当前标签页"
nmap <leader>cc :action CloseEditor<CR>

" 代码自动补全
let g:WhichKeyDesc_CodeAndClose_CodeCompletion = "<leader>cd 代码自动补全"
nmap <leader>cd <action>(CodeCompletion)

"关闭其他标签页
let g:WhichKeyDesc_CodeAndClose_CloseAllEditorsButActive = "<leader>co 关闭其他标签页"
nmap <leader>co :action CloseAllEditorsButActive<CR>

"========== d ==========
let g:WhichKeyDesc_DeBugOrDelete= "<leader>d 调试&删除"
"
" 打断点/解除断点
let g:WhichKeyDesc_DebugOrDelete_BreakPoint = "<leader>dp 打断点/解除断点 "
nmap <leader>dp <Action>(ToggleLineBreakpoint)

"调试
"let g:WhichKeyDesc_DebugOrDelete_DeBug = "<leader>db 调试"
"nmap <leader>db <Action>(Debug)

" 在可视模式中：删除选择的文本并复制到剪切板
"let g:WhichKeyDesc_DebugOrDelete_DeleteAndCopyToClipboard = "<leader>dd 删除并复制到剪切板"
"vmap <leader>dd "+d


"========== e ==========
"激活项目工具窗口(Alt + 1)
let g:WhichKeyDesc_ToggleExplorerOrExtract = "<leader>e 打开文件列表"
nmap <leader>e <action>(ActivateProjectToolWindow)

" extract method/function 将选中的代码片段提取为一个独立的方法(Ctrl + Alt + M)
"vmap <leader>em <action>(ExtractMethod)

" extract constant （引入常量）的重构操作:将选中的代码片段抽取为一个常量，并自动替换选中的代码片段为新的常量引用(Ctrl + Alt + C)
"vmap <leader>ec <action>(IntroduceConstant)

" extract field （引入字段）的重构操作:将选中的代码片段转化为一个新的字段，并自动将选中的代码片段替换为对该字段的引用(Ctrl + Alt + F)
"vmap <leader>ef <action>(IntroduceField)

" extract variable （引入变量）的重构操作:将选中的代码片段抽取为一个新的变量，并自动替换选中的代码片段为新的变量引用(Ctrl + Alt + V)
"vmap <leader>ev <action>(IntroduceVariable)

"========== f ==========
let g:WhichKeyDesc_FindOrFormat = "<leader>f 查找"

" 快速 导航/查找 项目中的其他文件(Ctrl + Shift + n)
let g:WhichKeyDesc_FindOrFormat_FindFile = "<leader>ff 查找文件"
nmap <leader>ff <action>(GotoFile)

" 快速 查找 当前文件(Ctrl + f)
let g:WhichKeyDesc_FindOrFormat_FindText = "<leader>fs 查找字符"
nmap <leader>fs <action>(Find)

" 在整个项目中查找指定的文本、关键字或正则表达式，包括代码文件、配置文件和其他文件等(Ctrl + Shift + F)
let g:WhichKeyDesc_FindOrFormat_FindAllText = "<leader>fa 全局查找字符"
nmap <leader>fa <action>(FindInPath)

" 将当前编辑的文件在项目视图中进行选中定位(Alt + F1)
let g:WhichKeyDesc_FindOrFormat_FindFileLocation = "<leader>fl 定位文件位置"
nmap <leader>fl <action>(SelectInProjectView)

" 打开 "Find Action"（查找动作）对话框(Ctrl + Shift + A)
let g:WhichKeyDesc_FindOrFormat_Commands = "<leader>fc 打开查找菜单"
nmap <leader>fc <action>(GotoAction)

" 重新格式化代码，使其符合预定义的代码样式和规范 \| 优化导入语句，删除未使用的导入，并将导入语句按字母顺序进行排列
" let g:WhichKeyDesc_FindOrFormat_Format = "<leader>fm 格式化代码"
" nmap <leader>fm <action>(ReformatCode) \| <action>(OptimizeImports)


"========== g ==========
let g:WhichKeyDesc_GitOrGenerate = "<leader>g Git&构造"

"执行版本控制（VCS）的回滚操作，将修改的代码还原到之前的版本
let g:WhichKeyDesc_GitOrGenerate_RollbackHunk = "<leader>gr git回滚"
let g:WhichKeyDesc_GitOrGenerate_RollbackHunk2 = "gr git回滚"
nmap <leader>gr :action Vcs.RollbackChangedLines<CR>
"生成构造器
let g:WhichKeyDesc_GitOrGenerate_GenerateConstructor = "<leader>gc 生成构造器"
nmap <leader>gc :action GenerateConstructor<CR>
"生成getter
let g:WhichKeyDesc_GitOrGenerate_GenerateGetter = "<leader>gg 生成getter"
nmap <leader>gg :action GenerateGetter<CR>
"生成setter
let g:WhichKeyDesc_GitOrGenerate_GenerateSetter = "<leader>gs 生成setter"
nmap <leader>gs :action GenerateSetter<CR>
"生成setter和getter
let g:WhichKeyDesc_GitOrGenerate_GenerateGetterAndSetter = "<leader>ga 生成setter和getter"
nmap <leader>ga <action>(GenerateGetterAndSetter)
"生成 equals() 和 hashcode() 的重写方法
let g:WhichKeyDesc_GitOrGenerate_GenerateEquals = "<leader>ge 生成equals和hashcode的重写"
nmap <leader>ge <action>(GenerateEquals)
"生成toString
let g:WhichKeyDesc_GitOrGenerate_GenerateToString = "<leader>gt 生成toString"
nmap <leader>gt <action>(Actions.ActionsPlugin.GenerateToString)
"diff 显示文件的版本控制历史(git)
nmap <leader>gd <action>(Vcs.ShowTabbedFileHistory)
let g:WhichKeyDesc_DebugOrDelete_ShowTabbedFileHistory = "<leader>gd 显示文件的版本控制历史"
"生成重写方法
nmap <leader>go <action>(OverrideMethods)
let g:WhichKeyDesc_DebugOrDelete_OverrideMethods = "<leader>go 生成重写方法"


"========== h ==========
"跳转到左边的分割窗口
"let g:WhichKeyDesc_Show_MoveToLeft = "<leader>h 向左跳转"
"nmap <leader>h <c-w>h


"========== i ==========
"快速查找并跳转到下一个以 ( 开始的函数或方法调用的位置️
"let g:WhichKeyDesc_InsertAfterBrackets = "<leader>i 跳转到选一个("
"nmap <leader>i f(a


"========== j ==========
"跳转到下边的分割窗口
"let g:WhichKeyDesc_Show_MoveToDown = "<leader>j 向下跳转"
"nmap <leader>j <c-w>j


"========== k ==========
"跳转到上边的分割窗口
"let g:WhichKeyDesc_Show_MoveToUp = "<leader>k 向上跳转"
"nmap <leader>k <c-w>k


"========== l ==========
"跳转到右边的窗口
"let g:WhichKeyDesc_Show_MoveToRight = "<leader>l 向右跳转"
"nmap <leader>l <c-w>l


"========== m ==========
"打开/关闭 代码小地图
" let g:WhichKeyDesc_CodeGlance = "<leader>m 开关小地图"
" nmap <leader>m <action>(CodeGlance.toggle)


"========== n ==========
let g:WhichKeyDesc_NERDTreeOrNew = "<leader>n 目录树&新建"

"取消搜索高亮显示(No Highlight)
let g:WhichKeyDesc_NERDTreeOrNew_Highlight = "<leader>nl 取消搜索高亮"
nmap <leader>nl :nohlsearch<CR>
"在当前目录新建文件夹
let g:WhichKeyDesc_NERDTreeOrNew_NewDir = "<leader>nd 新建文件夹"
nmap <leader>nd <action>(NewDir)
"在当前目录新建类
let g:WhichKeyDesc_NERDTreeOrNew_NewClass = "<leader>nc 新建Java文件"
nmap <leader>nc <action>(NewClass)

let g:WhichKeyDesc_NERDTreeOrNew_NewKotlinClass = "<leader>nk 新建Kotlin文件"
nmap <leader>nk <action>(Kotlin.NewFile)

"========== p ==========
"从剪切板粘贴到下面行
let g:WhichKeyDesc_PasteClipboardDown = "<leader>p 从剪切板粘贴到下面行"
nmap <leader>p "+p
let g:WhichKeyDesc_PasteClipboardDown = "<leader>p 从剪切板粘贴到下面行"
vmap <leader>p "+p

"========== q ==========
let g:WhichKeyDesc_CloseContent = "<leader>q 关闭当前标签页"
nnoremap <Space>q :q<CR>



"========== r ==========
let g:WhichKeyDesc_RunOrRe = "<leader>r 运行&重新"

"运行当前编辑器中的文件或类(Shift + F10)
let g:WhichKeyDesc_RunOrRe_RunCalss = "<leader>rc 运行当前文件"
nmap <leader>rc :action RunClass<CR>

"重新运行最近一次运行的程序或测试(Ctrl+Shift + F10)
let g:WhichKeyDesc_RunOrRe_ReRun = "<leader>rr 重新运行"
nmap <leader>rr <action>(Rerun)

"重新运行最近一次运行的测试（Unit Tests）(Ctrl + Shift + F10)
"let g:WhichKeyDesc_RunOrRe_ReRunTests = "<leader>rt 重新运行Test"
"nmap <leader>rt <action>(RerunTests)

"在代码中快速更改一个标识符的名称，并自动处理所有相关的引用(Shift + F6)
"let g:WhichKeyDesc_RunOrRe_Rename = "<leader>rn 重构"
"map <leader>rn <action>(RenameElement)
"运行
let g:WhichKeyDesc_RunOrRe_Run = "<leader>ru 运行"
map <leader>ru <action>(Run)

"========== s ==========
let g:WhichKeyDesc_Show = "<leader>s 显示&停止"

"显示当前打开文件的文件结构弹出窗口，其中包含文件中的类、方法、字段等结构(Alt + 7)
let g:WhichKeyDesc_Show_FileStructure = "<leader>ss 显示文件结构"
nmap <leader>ss <action>(FileStructurePopup)
"显示书签（Bookmarks）工具窗口，其中包含当前文件中设置的书签列表(Ctrl + F11)
let g:WhichKeyDesc_Show_Bookmarks = "<leader>sb 显示书签工具窗口"
nmap <leader>sb <action>(ShowBookmarks)
"用于显示方法或函数的参数信息(Ctrl + p)
let g:WhichKeyDesc_Show_ParameterInfo = "<leader>sp 显示方法或函数的参数信息"
nmap <leader>sp <action>(ParameterInfo)
"Stop
let g:WhichKeyDesc_Show_Stop = "<leader>st 停止运行"
nmap <leader>st <action>(Stop) <action>(Stop)

"========= t ==========
"翻译
"ubuntu: Translation.EditorTranslateAction
"windows: $EditorTranslateAction
let g:WhichKeyDesc_Translate = "<leader>t 翻译"
nmap <leader>t <action>($EditorTranslateAction)
vmap <leader>t <action>($EditorTranslateAction)

"========== v ==========
let g:WhichKeyDesc_Edit_RC = "<leader>ve 编辑配置文件"
nnoremap <leader>ve :e ~/.ideavimrc<CR>
let g:WhichKeyDesc_Source_RC = "<leader>vs 配置立即生效"
nnoremap <leader>vs :source ~/.ideavimrc<CR>


"========== w ==========
let g:WhichKeyDesc_Windows = "<leader>w 窗口"
let g:WhichKeyDesc_Windows_Hide = "<leader>ww 关闭提示窗口"
let g:WhichKeyDesc_Windows_Move = "<leader>wm 移动窗口"

" 向右拆分标签页
let g:WhichKeyDesc_Windows_Move_MoveTabRight = "<leader>wmv 垂直分割并向右拆分标签页"
nmap <leader>wmv <action>(MoveTabRight)

" 向下拆分标签页
let g:WhichKeyDesc_Windows_Move_MoveTabDown = "<leader>wmh 水平分割并向下拆分标签页"
nmap <leader>wmh <action>(MoveTabDown)

"在另一边打开（前提是有另一边的分割窗口）
"let g:WhichKeyDesc_Windows_MoveEditorToOppositeTabGroup = "<leader>wo 在另一边打开"
"nmap <leader>wo <action>(MoveEditorToOppositeTabGroup)

"向右复制标签页
let g:WhichKeyDesc_Windows_SplitVertically = "<leader>wv 垂直分割"
nmap <leader>wv <action>(SplitVertically)

" 向下复制标签页
let g:WhichKeyDesc_Windows_SplitHorizontally = "<leader>wh 水平分割"
nmap <leader>wh <action>(SplitHorizontally)

"取消所有分割窗口
let g:WhichKeyDesc_Windows_UnsplitAll = "<leader>wa 取消所有分割窗口"
nmap <leader>wa <action>(UnsplitAll)

"关闭当前窗口或分割窗格
let g:WhichKeyDesc_Windows_closeActiveWindow = "<leader>wd 关闭当前分割窗口"
nmap <leader>wd <c-w>c

"关闭提示窗口
"let g:WhichKeyDesc_Windows_Hide_HideActiveWindow = "<leader>wwa 关闭提示窗口"
"nmap <leader>wwa <action>(HideActiveWindow)

" 取消拆分当前分割窗口
let g:WhichKeyDesc_Windows_Unsplit = "<leader>wu 取消拆分当前分割窗口"
nmap <leader>wu <action>(Unsplit)

"========== y ==========
"普通模式下将 "+ (复制到剪切板）简化为 <leader>y
let g:WhichKeyDesc_CopyClipboard = "<leader>y 复制到剪切板"
vmap <leader>y "+y
"可视模式下将 "+ (复制到剪切板）简化为 <leader>y
let g:WhichKeyDesc_CopyClipboard= "<leader>y 复制到剪切板"
nmap <leader>y "+yy


"========== z ==========
let g:WhichKeyDesc_Zip = "<leader>z 折叠"

"展开所有代码折叠区域(Ctrl + Shift + 加号键)
let g:WhichKeyDesc_Zip_unZipAll = "<leader>zo 展开所有折叠"
nmap <leader>zo <action>(ExpandAllRegions)
"折叠所有代码折叠区域(Ctrl + Shift + 减号键)
let g:WhichKeyDesc_Zip_ZipAll = "<leader>zc 折叠所有代码"
nmap <leader>zc <action>(CollapseAllRegions)




" ================================================================================================
" 🌸🌸🌸 NERDTree 🌸🌸🌸
" ================================================================================================
"<C-w-w>：在多个打开的编辑器窗口之间切换
"在目录中，按下 go 打开文件并保持光标在目录
"在目录中，按下 gi 以上下并排窗口形式打开文件(并关闭目录)
"在目录树中，使用空格预览文件
"光标在目录时，按Esc回到编辑器
"编辑器和目录间切换存在许多功能类似的快捷键，相似但不完全相同



"按下 <leader>nn 将使焦点转移到 NERDTree 窗口
nnoremap <leader>nn :NERDTreeFocus<CR>
let g:WhichKeyDesc_NERDTreeOrNo_NERDTreeFocus = "<leader>nn 转移到目录树"

"按下 <C-n> 将打开 NERDTree 文件资源管理器(==<leader>nn)
nnoremap <C-n> :NERDTree<CR>

"按下 <C-f> 将在 NERDTree 文件资源管理器中定位当前编辑文件所在的节点，并使其可见(<leader>fl)
"nnoremap <leader>nf :NERDTreeFind<CR>

" ================================================================================================
" 🌸🌸🌸 Easymotion 🌸🌸🌸
" ================================================================================================

let g:WhichKeyDesc_easymotionkey = "<leader><leader> 快速跳转插件"
nmap s <Plug>(easymotion-s)
let g:WhichKeyDesc_easymotion = "s 快速跳转"
