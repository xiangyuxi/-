# iOS编程规范文档  
**本文主要翻译自苹果官方文档，翻译不当之处希望issue**

## 一、代码命名规范  
本节主要介绍类、方法、函数、常量以及编程接口的其他元素的命名规范。应该在项目中严格遵守本准则。  
### 一般原则 
* 1.清晰：在命名时尽量做到清晰简洁，如果清晰和简洁相互制约时，应当以清晰为首。 

>|代码|结论| 
|---|:---:|
|`insertObject:atIndex:`|正确|
|`insert:at:`|错误。插入什么？在哪里插入？|
|`removeObjectAtIndex:`|正确|
|`removeObject:`|正确|
|`remove:`|错误。什么被删除？|

命名时应当使用全称，而不是简写。

>|代码|结论| 
|---|:---:|
|`destinationSelection`|正确|
|`destSel`|错误。|
|`setBackgroundColor:`|正确|
|`setBkgdColor:`|错误|

不使用简写并不是说完全不能使用，对于某些国际通用的简写是可以使用的。[点这里](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE)  
尽管做到这些，我们还是无法保证对一些命名的清晰度，那么就应该使用注释来加以说明。  

* 2.一致性：在不同类中执行相同操作的方法应或属性该具有相同的名称。

>|代码|结论| 
|---|:---:|
|`@property(nonatomic) NSInteger tag;`|定义在`UIView,NSView`等中，表示视图标签。|

### 前缀
在`Objective-C`中，由于没有命名空间的说法，使得前缀变得尤为重要。它们区分软件的功能区域，防止开发人员之间或与系统的命名重合。一般情况下前缀会和所属框架紧密集合，命名类、协议、函数、常量和`typedef`结构时使用前缀，方法命名时一般不需要添加前缀，除非你非要加。

>|前缀|结论| 
|---|:---:|
|NS|Foundation、Application Kit|
|AB|Address Book|
|IB|Interface Builder|

### 驼峰法则
* 1.方法命名时，尽量不使用前缀。以小写字母开头，此后每个单词的首字母大写。

>`fileExistsAtPath:isDirectory:`  

* 2.常量、类、协议以前缀大写开头，此后每个单词首字母大写。

>```
NSRunAlertPanel
NSCellDisabled
```

### 类和协议名称
* 1.类的名称应当清除的包涵该类代表什么或做什么的名称，并且使用恰当的类前缀。

>`UIView，NSString，NSDate，NSScanner，NSApplication，UIApplication，NSButton和UIButton`

* 2.协议表示一类方法的集合，与类有本质上的区别。为了区分类和协议，协议命名时尽量使用一些恰当的动词来命名：`NSLocking`
* 3.有部分协议只是组合了一些不相关的方法，通常这些方法将作用于具体某一个类的，这时候建议将协议名和类名进行映射，甚至一样：`NSObject`

### 头文件
* 1.头文件命名尽量保证能够清楚知晓内部所包含的内容和适当的前缀。
* 2.如果是框架类型的，需要添加一个包含所有依赖关系的头文件。

## 二、方法命名
### 通用规则
* 1.方法以小写字母开头，此后每个单词的首字母大写。别使用前缀。有两个例外的是:

>1.类似于PDF、TIFF、HTTP等这种固有名称大写来开头一个方法。  
>2.可以使用前缀来分组和标识私有方法。

* 2.若方法为表示对对象的某种操作，需要使用该操作的动词起头。不要使用“do”或“does”作为名称的一部分，因为这些辅助动词很少添加意义。 另外，在动词前不要使用副词或形容词

>```
- (void)invokeWithTarget:(id)target;
- (void)selectTabViewItem:(NSTabViewItem *)tabViewItem
```

* 3.如果该方法返回接收方的属性，请在该属性后面指定该方法。 使用`get`是不必要的，除非间接返回一个或多个值。

>|代码|结论| 
|---|:---:|
|`- (NSSize)cellSize;`|正确|
|`- (NSSize)calcCellSize;`|错误。|
|`- (NSSize)getCellSize;`|错误。|

* 4.多个参数时，善于利用关键字进行区分。

>|代码|结论| 
|---|:---:|
|`- (void)sendAction:(SEL)aSelector toObject:(id)anObject forAllCells:(BOOL)flag;`|正确|
|`- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;`|错误|

* 5.在参数前使用关于该参数的描述名称加以修饰。

>|代码|结论| 
|---|:---:|
|`- (id)viewWithTag:(NSInteger)aTag;`|正确|
|`- (id)taggedView:(int)aTag;`|错误|

* 6.在创建比继承的方法更具体的方法时，将新关键字添加到现有方法的末尾。

>|代码|结论| 
|---|:---:|
|`- (id)initWithFrame:(CGRect)frameRect;`|`NSView, UIView.`|
|`- (id)initWithFrame:(NSRect)frameRect mode:(int)aMode cellClass:(Class)factoryId numberOfRows:(int)rowsHigh numberOfColumns:(int)colsWide;`|`NSMatrix`, a subclass of NSView|

* 7.不要使用`and`链接两个参数。

>|代码|结论| 
|---|:---:|
|`- (int)runModalForDirectory:(NSString *)path file:(NSString *) name types:(NSArray *)fileTypes;`|`NSView, UIView.`|
|`- (id)initWithFrame:(NSRect)frameRect mode:(int)aMode cellClass:(Class)factoryId numberOfRows:(int)rowsHigh numberOfColumns:(int)colsWide;`|`NSMatrix`, a subclass of NSView|

* 8.如果该方法描述两个单独的操作，这是可以使用`and`链接它们。

>|代码|结论| 
|---|:---:|
|`- (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;`|`NSWorkspace `|

### 存取方法
* 1.如果属性用名词表示，则格式为：

>```
- (type)noun;
- (void)setNoun:(type)aNoun;
```

* 2.如果属性用形容词表示，则格式为:

>```
- (type)isAdjective;
- (void)setAdjective:(type)flag;
```

* 2.如果属性用动词表示，则格式为:

>```
- (type)verbObject;
- (void)setVerbObject:(type)flag;
```

* 3.您可以使用模态动词（动词前面加上`can，should，will`等）来澄清意思，但不要使用`do，does`。

### 代理方法
委托方法（或委托方法）是当某些事件发生时，委托对象在其委托中调用。  

* 1.使用发送该委托消息的对象的类来作为开头命名。类名省略前缀，第一个字母是小写。

>```
- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
- (BOOL)application:(NSApplication *)sender openFile:(NSString *)filename;
```

* 2.如果有多个参数并且需要引用调用对象时，第一个参数应当为引用本身，直接在类名后添加。否则，只有一个调用对象需要引用时应当尽量清晰的表明参数的使用。

>```
- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
- (BOOL)applicationOpenUntitledFile:(NSApplication *)sender;
```

* 3.发布通知而调用的方法，唯一的参数是通知对象。

>```
- (void)windowDidChangeScreen:(NSNotification *)notification;
```

* 4.对于被调用的方法，使用`did`或`will`通知委托人已经发生或将要发生的事情。

>```
- (void)browserDidScroll:(NSBrowser *)sender;
- (NSUndoManager *)windowWillReturnUndoManager:(NSWindow *)window;
```

* 5.虽然您可以使用`did”或“will`来表示方法的调用时间顺序，但是当你建议委托对象执行某些操作时，`should`是首选项。

>```
- (BOOL)windowShouldClose:(id)sender;
```

### 集合对象
当对象需要对集合类的对象（`arrays,dictionaries,sets`）进行管理时，需要遵守以下规则：

>```
- (void)addElement:(elementType)anObj;
- (void)removeElement:(elementType)anObj;
- (NSArray *)elements;
```

* 1.如果集合是真正无序的，则返回NSSet对象而不是NSArray对象。
* 2.如果将元素插入集合中的特定位置很重要，请使用与以下内容类似的方法:

>```
- (void)insertLayoutManager:(NSLayoutManager *)obj atIndex:(int)index;
- (void)removeLayoutManagerAtIndex:(int)index;
```

### 方法参数
* 1.遵守驼峰原则。
* 2.让参数类型而不是其名称声明它是否是一个指针。
* 3.为参数避免使用一个和两个字母的名称。
* 4.避免只保留几个字母的缩写。  
举个例子，以下关键字和参数一起使用：

>```
...action:(SEL)aSelector
...alignment:(int)mode
...atIndex:(int)index
...content:(NSRect)aRect
...doubleValue:(double)aDouble
...floatValue:(float)aFloat
...font:(NSFont *)fontObj
...frame:(NSRect)frameRect
...intValue:(int)anInt
...keyEquivalent:(NSString *)charCode
...length:(int)numBytes
...point:(NSPoint)aPoint
...stringValue:(NSString *)aString
...tag:(int)anInt
...target:(id)anObject
...title:(NSString *)aString
```

### 私有方法
在私用方法命名是和上面提到的大相径庭，不同的是你可以使用前缀来区分。Cocoa框架中大多数私有方法的名称都有一个下划线前缀（例如_fooData），将其标记为私有。 从这个事实可以看出两个建议：

* 1.如果您将一个大型Cocoa框架类（如NSView或UIView）进行子类化，并且您希望绝对确保您的私有方法的名称与超类中的名称不同，则可以为您的私有方法添加自己的前缀。 前缀应尽可能独一无二，也许根据您的公司或项目以及`XX_`的格式。 所以如果您的项目称为Byte Flogger，则前缀可能为`BF_addObject:`
* 2.在使用下划线字符作为您的私有方法的前缀时请谨慎，官方有这个习惯。

## 函数命名
`Objective-C`允许您通过函数和方法来表达行为。底层对象始终是单例时，您应该使用函数而不是类方法。

* 1.函数名称类似于方法命名，但有一些例外:

> * 他们的前缀和作用于的类或常量相同。  
> * 若没有在前缀后面跟随下划线时，就应当保证驼峰原则了。

* 2.大多数函数名称以描述函数的效果的动词开头。

>```
NSHighlightRect
NSDeallocateObject
```

* 3.查询属性的函数还有一组命名规则:

> * 如果函数返回其第一个参数的属性，则省略动词。
> 	
	```
	unsigned int NSEventMaskFromType(NSEventType type)
	float NSHeight(NSRect aRect)
	```
> * 如果通过引用返回值，请使用`Get`。
> 	
	```
	const char *NSGetSizeAndAlignment(const char *typePtr, unsigned int *sizep, unsigned int *alignp)
	```
> * 如果返回的值是一个布尔值，则该函数应以一个变形的动词开头。
> 	
	```
	BOOL NSDecimalIsNotANumber(const NSDecimal *decimal)
	```

## 属性和数据类型的命名
本节介绍声明属性，实例变量，常量，通知和异常的命名约定。

### 属性和实例变量的命名

* 1.属性只是集变量、存取器于一体的操作，基本也和存取器的命名一致。通用情况下声明属性模板为：

>```
@property (…) type nounOrVerb;
```

* 2.很多情况下，不仅会声明一个属性，还是声明一个与之对应的实例变量。确保实例变量的名称简明扼要地描述了存储的属性。 通常，你不应该直接访问实例变量; 您应该使用访问器方法（您可以直接在init和dealloc方法中访问实例变量）。 为了有助于表明这一点，前缀实例变量名称带有下划线（_），例如：

>```
@implementation MyClass {
    BOOL _showsTitle;
}
```

* 3.如果声明的属性和实例变量需要一一对应时，请使用`@synthesize`来指定：

>```
@implementation MyClass
@synthesize showsTitle = _showsTitle;
```

* 4.声明实例变量时需要注意一下几个问题：

> * 避免显式声明公共实例变量。 
> * 如果需要声明实例变量，请使用@private或@protected显式声明它.
> * 如果实例变量是类的实例的可访问属性，请确保为其编写访问器方法（如果可能，请使用声明的属性）。

### 常量
* 1.枚举常量

> * 对具有整数值的相关常量的组使用枚举。
> * 枚举常数和它们分组的`typedef`遵循函数的命名约定（参见函数命名）。以下示例来自NSMatrix.h：\
> 
	```
	typedef enum _NSMatrixMode {
    	NSRadioModeMatrix           = 0,
    	NSHighlightModeMatrix       = 1,
    	NSListModeMatrix            = 2,
   		NSTrackModeMatrix           = 3
	} NSMatrixMode;
	```
> * 可以为位掩码创建未命名的枚举，例如：
> 
	```
	enum {
    	NSBorderlessWindowMask      = 0,
    	NSTitledWindowMask          = 1 << 0,
    	NSClosableWindowMask        = 1 << 1,
    	NSMiniaturizableWindowMask  = 1 << 2,
    	NSResizableWindowMask       = 1 << 3
	};	
	```

* 2.常量

> * 如果常数与其他常量无关，则可以使用const创建整数常量；否则，使用枚举。
> * 常量的格式以下列声明为例：
> 
	```
	const float NSLightGray;
	```
> * 与枚举常量一样，命名约定与函数相同。

* 3.其他

> * 对于整数常量，使用枚举，而浮点常量使用const限定符。尽量不适用`#define`
> * 使用大写字母作为预处理程序评估的符号，以确定是否处理代码块。 例如：
> 
	```
	#ifdef DEBUG
	```
> * 请注意，编译器定义的宏具有前导和后置双下划线字符。 例如：
> 
	```
	__MACH__
	```
> * 为用于通知名称和字典键等用途的字符串定义常量。 通过使用字符串常量，您确保编译器验证指定了正确的值（即，它执行拼写检查）。 Cocoa框架提供了许多字符串常量的例子，例如(对于Objective-C，APPKIT_EXTERN宏对外部进行评估)：
> 
	```
	APPKIT_EXTERN NSString *NSPrintCopies;
	```
	
### 通知和异常

* 1.通知

> * 通知名称应当清晰的知晓通知所做的事情，通知事件应当和通知名称相应一致。 例如，当应用程序发布`NSApplicationDidBecomeActiveNotification`时，全局`NSApplication`对象的委托将自动注册以接收`applicationDidBecomeActive：`消息。
> * 通知通过以这种方式组成名称的全局NSString对象来标识:
>
	```
	[Name of associated class] + [Did | Will] + [UniquePartOfName] + Notification
	```
> * 例如：
>
	```
	NSApplicationDidBecomeActiveNotification
	NSWindowDidMiniaturizeNotification
	NSTextViewDidChangeSelectionNotification
	NSColorPanelColorDidChangeNotification
	``` 
	
* 2.异常

> * 异常由全局NSString对象识别，其名称以这种方式组成：
> 
	```
	[Prefix] + [UniquePartOfName] + Exception
	```
> * 例如：
>
	```
	NSColorListIOException
	NSColorListNotEditableException
	NSDraggingException
	NSFontUnavailableException
	NSIllegalSelectorException
	```

## 框架开发技巧

### 初始化
* 1.类的初始化

> * 在调用类之前需要对其做一些初始社会，并且只需要执行一次时，你可以使用`+ (void)initialize`方法。不能自行调用`initialize`。因为运行时向每个类发送初始化，所以可能在子类的上下文中调用`initialize`，如果子类不实现`initialize`，那么调用将落入超类。如果您特别需要在相关类的上下文中执行初始化，则可以执行以下检查:

>
	```
	if (self == [NSFoo class]) {
    // the initializing code
	}
	```

* 2.指定初始化器

> * 指定的初始值设置应该被清楚地识别，并且参数所表述的信息也应当十分清晰。当你的类需要执行归档时，你必须实现`initWithCoder:`和`encodeWithCoder:`

* 3.初始化时检查错误

> * 通过调用`super`的指定的初始化程序来加载父类的实现。
> * 检查返回的值为nil，表示超类初始化中发生一些错误。
> * 如果在初始化当前类时发生错误，请释放该对象并返回nil。
> 
	```
	- (id)init {
    	self = [super init];  // Call a designated initializer here.
    	if (self != nil) {
        	// Initialize object  ...
        	if (someError) {
            	[self release];
            	self = nil;
        	}
    	}
    	return self;
	}
	```

### 版本和兼容性
* 1.框架版本

> * 版本号中记录更改情况。
> * 设置您的框架的当前版本号，并提供一些方法使其可全局访问。您可以将版本号存储在框架的信息属性列表（Info.plist）中，并从中进行访问。

* 2.密匙归档

> * 如果存档中缺少密钥，请求其值将返回`nil`，`NULL`，`NO`，`0`或`0.0`，具体取决于所请求的类型。测试此返回值以减少您写出的数据。 另外，您可以确定是否将密钥写入存档。
> * 编码和解码方法都可以做到这一点，以确保向后兼容。例如，类的新版本的编码方法可能会使用密钥写入新值，但是仍然可以写出较旧的字段，以便旧版本的类仍然可以理解该对象。此外，解码方法可能希望以合理的方式处理缺少的值，以便为将来的版本保持一定的灵活性.
> * 框架类的归档密钥的推荐命名是从框架的其他API元素使用的前缀开始，然后使用实例变量的名称。只要确保名称不能与任何超类或子类的名称冲突。
> * 请确保使用唯一的键为需要保存的密匙的映射值。例如，归档矩形的“archiveRect”例程应该采用一个关键参数，并使用给定的键，或者如果它写出多个值（例如，四个浮点数），它应该将附加的唯一位添加到提供的键。
> * 由于编译器和字节依赖关系，原来存档的位域可能是危险的。

### 错误和异常
大多数Cocoa框架方法不会强制开发人员捕获和处理异常。 这是因为异常不会作为正常的执行部分提出，通常不用于传达预期的运行时或用户错误。 这些错误的例子包括：
> * 文件为找到
> * 无使用者
> * 尝试在应用程序中打开错误类型的文档
> * 将字符串转换为指定的编码时出错

但是，Cocoa确实引发了异常以指示编程或逻辑错误，如下所示
> * 数组索引超出界限
> * 尝试改变不可变对象
> * 错误的参数类型

Cocoa方法一般不会返回错误代码。在有一个合理或可能的错误原因的情况下，这些方法依赖于布尔值或对象（nil / non-nil）返回值的简单测试;记录NO或NIL返回值的原因。您不应该使用错误代码来指示在运行时处理的编程错误，而是引发异常，或者在某些情况下只需记录错误而不会引发异常。

### 框架数据

* 1.常量数据

> * 出于性能原因，尽可能多地将框架数据标记为不变，因为这样做可以减小Mach-O二进制数的__DATA段的大小。不是const的全局和静态数据最终在__DATA段的__DATA部分。这种数据在使用该框架的应用程序的每个运行实例中占用内存。虽然额外的500字节（例如）可能看起来不太糟糕，但可能会导致需要的页数增加，每个应用程序额外增加四千字节。您应该将任何常量的数据标记为const。如果块中没有char *指针，这将导致数据降落在__TEXT段中（这使得它真正不变）;否则它将保留在__DATA段中，但不会被写入（除非预绑定未完成，否则必须在加载时滑动二进制文件）。您应该初始化静态变量，以确保它们被合并到__DATA段的__data部分，而不是__bss部分。如果没有明显的值用于初始化，请使用0，NULL，0.0或任何适当的值。

* 2.为字段

> * 如果代码假定该值为一个布尔值，则使用位域（特别是一位）的带符号值可能会导致未定义的行为。一位位域应始终为无符号。因为可以存储在这样的位域中的唯一值为0和-1（取决于编译器的实现），将该位域与1进行比较是`false`。 例如，如果您在代码中遇到这样的问题：
> 
	```
	BOOL isAttachment:1;
	int startTracking:1;
	```
你应当把类型更改为:`unsigned int`。

* 3.内存分配

> *  如果由于某种原因需要临时缓冲区，通常使用堆栈比分配缓冲区更好。但是，堆栈的大小有限（通常为512千字节），因此决定使用堆栈取决于所需的缓冲区的功能和大小。通常，如果缓冲区大小为1000字节（或MAXPATHLEN）或更小，则使用堆栈是可以接受的。
> * 使用堆栈和malloc开辟空间:
> 
	```
	#define STACKBUFSIZE (1000 / sizeof(YourElementType))
 	YourElementType stackBuffer[STACKBUFSIZE];
 	YourElementType *buf = stackBuffer;
 	int capacity = STACKBUFSIZE;  // In terms of YourElementType
 	int numElements = 0;  // In terms of YourElementType
	while (1) {
    	if (numElements > capacity) {  // Need more room
        	int newCapacity = capacity * 2;  // Or whatever your growth algorithm is
        	if (buf == stackBuffer) {  // Previously using stack; switch to allocated memory
            	buf = malloc(newCapacity * sizeof(YourElementType));
            	memmove(buf, stackBuffer, capacity * sizeof(YourElementType));
        	} else {  // Was already using malloc; simply realloc
            	buf = realloc(buf, newCapacity * sizeof(YourElementType));
        	}
        	capacity = newCapacity;
    	}
    	// ... use buf; increment numElements ...
  	}
  	// ...
  	if (buf != stackBuffer) free(buf);
	```
	
### 对象比较
* 1.一般都需要独立一个比较方法当类中，用于比较两个对象是否相同。
* 2.严格区分`isEqual:`和`isEqualToType:`。

## 三、其他

### 代码结构

在方法分组和protocol/delegate实现中使用#pragma mark -来分类方法，要遵循以下一般结构：

```
#pragma mark - Properties

- (void)setCustomProperty:(id)value {}

#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

### 空格
* 1.确保使用Xcode默认的间距。
* 2.方法大括号和其他大括号(if/else/switch/while 等。)，总是在同一行语句打开但在新行中关闭。
	
	```
	if (user.isHappy) {  
    	//Do something  
	} else {  
    	//Do something else  
	}
	```
* 3.应该避免以冒号对齐的方式来调用方法。因为有时方法签名可能有3个以上的冒号和冒号对齐会使代码更加易读。请不要这样做，尽管冒号对齐的方法包含代码块，因为Xcode的对齐方式令它难以辨认。

	```
	// blocks are easily readable  
	[UIView animateWithDuration:1.0 animations:^{  
  		// something  
	} completion:^(BOOL finished) {  
  		// something  
	}];
	```
* 4.应尽量在你的代码中将每行控制在100个字符内。
	
### 注释
* 1.一般应当尽量避免块注释，尽量让每一行代码都自行注释。
* 2.方法注释使用系统提供的快捷键进行注释即可（`command+shift+\`）。

### 属性特性
* 1.属性特性的顺序应该是storage、atomicity，与在Interface Builder连接UI元素时自动生成代码一致。
* 2.NSString应该使用copy而不是strong的属性特性。

### 点语法
* 1.点语法应该 **总是** 被用来访问和修改属性，因为它使代码更加简洁。

### 字面值
* 1.对于`NSString`、`NSDictionary`、`NSArray`和`NSNumber`类型的值确定且不变是应当直接使用字面值，而不是调用初始化方法。

### 枚举类型
* 1.推荐使用`NS_ENUM()`来定义。

### 私有属性
* 1.私有属性应该在类的实现文件中的类扩展(匿名分类)中声明。

### 类构造方法
* 1.当类构造方法被使用时，它应该返回类型是instancetype而不是id。这样确保编译器正确地推断结果类型。

	```
	@interface Airplane  
	+ (instancetype)airplaneWithType:(RWTAirplaneType)type;  
	@end
	```

### CGRect函数
* 1.当访问CGRect里的x, y, width, 或 height时，应该使用CGGeometry函数而不是直接通过结构体来访问。引用Apple的CGGeometry：  

	>在这个参考文档中所有的函数，接受CGRect结构体作为输入，在计算它们结果时隐式地标准化这些rectangles。因此，你的应用程序应该避免直接访问和修改保存在CGRect数据结构中的数据。相反，使用这些函数来操纵rectangles和获取它们的特性。

	```
	CGRect frame = self.view.frame;  
	CGFloat x = CGRectGetMinX(frame);  
	CGFloat y = CGRectGetMinY(frame);  
	CGFloat width = CGRectGetWidth(frame);  
	CGFloat height = CGRectGetHeight(frame);  
	CGRect frame = CGRectMake(0.0, 0.0, width, height); 
	```
	
### 单例模式

	```
	// 单利实现
	static class* _instance = nil; 
	+ (instancetype)shared##class {
    	static dispatch_once_t onceToken; 
    	dispatch_once(&onceToken, ^{ 
        	_instance = [[super allocWithZone:NULL] init]; \
    	}); 
    	return _instance; 
	} 
	+ (id)allocWithZone:(struct _NSZone *)zone { 
    	return [class shared##class]; 
	} 
	- (id)copyWithZone:(struct _NSZone *)zone { 
    	return [class shared##class]; 
	}
	```

### Category的命名
* 1.Category的命名应该包含2-3个字符的前缀，用于说明Category是属于具体的某个工程的。