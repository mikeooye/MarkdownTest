# MarkdownTest
格式化文章使用

# [代码规范] Cocoa Code Guideline

## 一般性原则 
- 命名应尽可能的简洁清楚，但，应更注重清楚 
 - `insertObject: atIndex:` Good
 - `insert: at:` Not clear
- 为了清楚表达意思会写很长，但，也不要简写
 - `destinationSelection` Good 
 - `destSel` Not Clear 
 
  *注：不使用简写可以让其他开发人员（如与你文化背景不同的）也能明白含义 
- 众所周知的缩写在命名时是可以继续使用的  
[Acceptable Abbreviations and Acronyms](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE)
- 避免使用歧义的名字
  - `sendPort` Does it send the port or return it?
- 保持一致性
- 不引用自身
  - `NSString` Okay.
  - `NSStringObject` 引用了自身(`Object`) 
  *注：常量(Constants)和通知(Notification)命名除外

## 约定
- 在名字中间不适用任何符号（如下划线、书名号）分割，取而代之，将每个单词的首字母大写，即驼峰命名法（camel-casing）
  - 对于method（类方法）命名，起始字母小写，余下每个单词首字母大写，不要使用前缀（Prefix）
    - `fileExistsAtPath:isDirectory:`
  - 对于functions（方法）和常量的命名，用相关类名前缀起始，其余单词首字母大写
    - `NSRunAlertPanel`
    - `NSCellDisabled`
  - 不要使用下划线作为开始命名一个私有类方法（但可以用于命名实例变量），因为这种命名是Apple的保留命名格式；可以使用`XX_`的形式作为前缀代替下划线
    - `_privateMethod` 不好
    - `BP_privateMethod` 可以

## 类(Class)的命名
类的名字中应该包含一个名词，用来表达这个类是什么，是用来做什么的。并且应该包含一个前缀（限Objective-C，Swift中已经不使用前缀了），Foundation和application框架中有很多例子可以参考，如：`NSString`,`UIButton`等

## 头文件（Header Files）
如何定义头文件名很重要，好的命名可以表示文件中都包含了什么
- 如果文件中只声明了类或协议，文件名就使用类名或协议名就可以
- 声明相关的类和协议
  - `NSString.h`    声明了`NSString`和`NSMutableString`类
- 框架头文件。每个框架库（framework）都应该有一个头文件与框架同名，其中包含了（include）框架中所有的公开头文件
  - `Foundation.h`  (`Foundation.framework`)
  - 扩展其他框架中的类。如果扩展（Category）了其他框架中某个类的方法，在其头文件末尾添加`Additions`，如`NSBundleAdditions.h`
  - 在合适的头文件中定义方法（function），常量，结构体和其他数据类型 

## Methods命名  
- 起始字母小写，其他单词首字母大写，不使用前缀
- 如果方法表述一个动作，起始单词使用动词
  *注：不要使用`do`或`does`这类没有具体意义的动词，也不要在动词前添加形容词和副词
  - `- (void)invokeWithTarget:(id)target;`
- 多个参数前应该有关键词描述
  - `- (void)sendAction:(SEL)aSelector toObject:(id)anObject forAllCells:(BOOL)flag;`
- 参数前的描述是用来描述参数的
  - `- (id)viewWithTag:(NSInteger)aTag;`  正确
  - `- (id)taggedView:(int)aTag;` 错误
- 已经存在的方法，想要增加参数，放到后面去
  - `- (id)initWithFrame:(CGRect)frameRect;`
  - `- (id)initWithFrame:(NSRect)frameRect mode:(int)aMode cellClass:(Class)factoryId numberOfRows:(int)rowsHigh numberOfColumns:(int)colsWide;`
- 参数之间没有必要使用`and`连接
  - `- (int)runModalForDirectory:(NSString *)path file:(NSString *) name types:(NSArray *)fileTypes;`
- 如果方法表述了两个动作，使用`and`连接
  - `- (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;` 

### 存取方法（Accessor Methods）
- 名词
  - `- (NSString *)title;`
  - `- (void)setTitle:(NSString *)aTitle;`
- 形容词
  - `- (BOOL)isEditable;`
  - `- (void)setEditable:(BOOL)flag;`
- 动词
  - `- (BOOL)showsAlpha;`
  - `- (void)setShowsAlpha:(BOOL)flag;`
- 不要使用动词的过去式或现在分词作为形容词
  - `- (void)setAcceptsGlyphInfo:(BOOL)flag;` 对
  - `- (void)setGlyphInfoAccepted:(BOOL)flag;` 错
- 可以使用`can`,`should`,`will`等词语放在动词前，是意思表达更明确，但不要使用`do`和`does`
- `get`只应该用在间接获取数据时
  - `- (void)getLineDash:(float *)pattern count:(int *)count phase:(float *)phase;` 

### 代理方法
- 以发送消息的类的名字作为起始，去掉前缀并且首字母小写，参数放在后面
  - `- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;`
- 如果方法只有一个参数，是发送消息类的自身
  - `￼- (BOOL)applicationOpenUntitledFile:(NSApplication *)sender;`
- 例外，通知
  - `- (void)windowDidChangeScreen:(NSNotification *)notification;`
- 用`did`或`will`表示某个事件已经发生还是将要放生  
  - `- (void)browserDidScroll:(NSBrowser *)sender;`
  - `- (NSUndoManager *)windowWillReturnUndoManager:(NSWindow *)window;`
- 用`should`表示向delegate请求某个操作是否被允许
  - `- (BOOL)windowShouldClose:(id)sender;`
  
## Functions 命名  
- Function 的命名与Method 有点像，稍微有点区别
  - 以前缀开始（与类或常量相同的前缀）
  - 首字母大写

## Properties和Data Types命名  
- 动词或名词
  - `@property (strong) NSString *title;`
  - `@property (assign) BOOL showsAlpha;`
- 形容词
  - `￼@property (assign, getter=isEditable) BOOL editable;`

很多情况下，我们会把`property`与合适的变量进行`synthesize`。  
```
@implementation MyClass {
    BOOL _showsTitle;
}
@synthesize showsTitle=_showsTitle;
```

确保变量名字能够正确表达存储的数据，通常，应避免直接操作变量，取而代之，使用存取方法（可以在`init`和`dealloc`方法中直接处理变量） 

#### Notifications
格式  
`[Name of associated class] + [Did | Will] + [UniquePartOfName] + Notification`  
举例：  
```
NSApplicationDidBecomeActiveNotification
NSWindowDidMiniaturizeNotification
NSTextViewDidChangeSelectionNotification
NSColorPanelColorDidChangeNotification
```
