Complete iOS Developer Style Guide
=======================

There are many Objective-C / CocoaTouch / iOS style guides out there but none of them touch on everything. This is an attempt to be a complete style guide that will cover all aspects of iOS/Objective-C development.

This guide is ultimately intended to be a community project, owned by everyone. Push requests are asked for to fill in the holes, fix problems, or add clarity and detail. If you are submitting a change or addition that is non-standard or obscure, please include some reference as to the reasoning. Apple documentation and community consensus will trump personal preference.

Version History
---------------
v0.0.1 - 2013-08-31 - Initial version with many missing pieces

Comments
--------
Objective-C should be coded in a way that is very descriptive. If done right, comments can be kept to a minimum. Only comment when you need to explain *why* the code is written the way it is.

Comments should be written on their own line and have a blank line between itself and the code above it. Block comments should be avoided. 

**Example**
```Objective-C
int carCount = 0;

//This is a comment for the line below
carCount++;
```

Prefixing
---------
Every project should have a three character, capitolized prefix to be used in class names, static variables, `#define`s, etc. Each project should have its own prefix rather than using a company prefix or a person's initials.

Core data names and NSManagedObject subclasses should not use the prefix.

Variables
---------
Variables should be named descriptively and avoid use of acronyms. Pointer asterisk should be with the variable name. If the variable is of a common type, avoid putting that in the name. Variable names should start with a lowercase letter and be camelcased.

**Examples**
```Objective-C
//Good
NSString *carModel = @"Mustang";
CGFloat noteViewWidth = 30;


//Bad
NSString * carModelString = @"Mustang";
CGFloat nwidth = 30;
```

There are times when it is acceptible to add the type in the variable name when omitting it may lead to confusion. For example:

```Objective-C
NSDate *currentDate = [NSDate date];
//'String' is appended to the variable name to avoid confusion that it might be an NSDate object.
NSString *currentDateString = [XYZDateFormatter stringFromDate:currentDate];
```

Methods
-------
Methods should be formatted as so (notice the space after the scope symbol)

```Objective-C
//Instance method
- (void)buildJSONRequestFromString:(NSString *)request withTag:(int)tag;
 
//Class method
+ (XYZString *)uppercaseStringFromString:(NSString *)string;
```
Method names should be as descriptive as possible. Avoid using names that start off with *set* or *get* unless it is a setter or getter method. Also avoid the prefix *is* unless it returns a BOOL.

If possible, methods should return its last line of code:

```Objective-C
//Good
- (NSString *)stringFromInt:(int)number {
	return [NSString stringWithFormat:@"%d", number];
}

//Bad
- (NSString *)stringFromInt:(int)number {
	NSString *numberString = [NSString stringWithFormat:@"%d", number];
	return numberString;
}
```

Properties
----------
Properties should be defined for all instance variables whenever possible. Only declare properties in the header file when outside classes need access. Avoid using @synthesize unless needed to make a public readonly property privately writable. Avoid accessing instance variables directly unless in initializer methods, dealloc, or getter and setter methods.

**Examples**
```Objective-C
//Good
self.notes = [[NSArray alloc] init];

//Bad
_notes = [[NSArray alloc] init];

//Good
@interface XYZNoteView : XYZSuperView

@property (nonatomic, strong) NSMutableArray *notes;

@end

//Bad
@interface XYZNoteView : XYZSuperView {
	NSArray _notes;
}

@property (nonatomic, strong) NSMutableArray *notes;

@end
```

Properties should always be accessed using dot notation (`car.model` rather than `[car model] or [car setModel]`).

Constants
---------
Instance constants should start with a lowercase `k`, followed by the project prefix, followed by a descriptive name.

**Example**
```Objective-C
static NSString * const kXYZProjectCellID = @"ProjectCell";
static CGFloat const kXYZMenuTopMargin = 80;
```

Use of `#define` to set constants should be avoided.

Literals
--------
`NSString`, `NSNumber`, `NSArray`, and `NSDictionary` literals should be used whenever possible.

```Objective-C
NSString *name = @"John Doe";
NSNumber *maxSpeed = @160;
NSNumber *halfSpeed = @(maxSpeed / 2);
NSArray *colorNames = @[@"blue", @"red", @"green"];
NSDictionary *driver = @{@"name": name, @"maxSpeed": maxSpeed, @"nickName": @"crash"};
```

View Controller Class Names
-----------
View controller names follow standard class naming conventions (capitolized project prefix, camelcased descriptive name) but are also suffixed with either `ViewController`, `TableViewController` or `CollectionViewController`, depending on type. Avoid shortening to `VC`, `TVC`, or `CVC`.

Class Headers
------------
Keep headers as simple as possible. Only include properties and methods that other classes need access to. Consider making some properties readonly if they shouldn't be changed by an outside class. 

Header files should be organized as so:

```Objective-C
//Import statements first. Import in this order: superclass, frameworks, local classes
#import "XYZSuperView.h"
#import <UIKit/UIKit.h>
#import "XYZCar.h"

//Classes should be forward declared whenever possible. Just make sure you #import in the .m file.
@Class XYZDriver;
@Class XYZPassenger;

//Any protocol definitions next. Protocol should always include the class name in its own name.
@protocol XYZNoteViewDelegate <NSObject>

- (void)closedPopover;

@end

//Interface
@interface XYZNoteView : XYZSuperView <XYZCarDelegate>

//Properties - Only properties that need to be included publicly. Otherwise, define in the .m file
@property (nonatomic, strong) XYZDriver *driver;

//Even create properties for C primitive types
@property int numberOfCrashes;

//Instance methods
- (NSString *)driverNameFromDriver:(Driver *)driver;

//Class methods
+(XYZNoteView *)noteViewFromPassenger:(Passenger *)passenger;
```

Implementation
--------------
Implementation files should be organized in a useful way.

Methods should be organized in the following order: 

* Init and dealloc methods
* Super class methods being overwritten
* Custom getters and setters
* Delegate methods
* Private instance methods
* Public instance methods (methods defined in the header file)
* Public class methods (methods defined in the header file)

Each of these sections should be seperated with a `#pragma mark` as so:

```Objective-C
#pragma mark - UIViewController methods
```

Sections can be further broken down into subsections by using `#pragma mark` without the hyphen.

Subclassing, Categories and Extensions
--------------------------------------
When possible, it is best to use categories rather than subclassing. This will encourage composition over inheritance. Categories should serve one function. For example, you might define `UIColor+XYZProjectColor` to house methods for retrieving common colors used in your project. If you have a need for custom color blending methods, create a second category named `UIColor+XYZColorBlend`.

Extensions are another way to avoid subclassing. Use these when you simply need to add additional properties to a custom class. Private properties should be declared in an anonymous extension.

When subclassing must be used, name the subclass in a way that suggests the super class. For example, `XYZMenuView` might be subclassed as `XYZCarMenuView`.

Project Organization
--------------------
Files should be organized in directories that match their XCode groups.

MVC seperation should be applied to directories and groups: data model classes, view controllers and custom views should be seperate directories. 

Other groups and directories should be created for other assets and classes in some sort of logical scheme based on the project. The root directory should only contain `AppDelegate.h` and `AppDelegate.m`

Leave things that XCode creates automatically (such as `main.m` and `Prefix.pch`) in their original location and their original name whenever possible.
