Complete-iOS-StyleGuide
=======================
v0.0.1

*Note: This is still a work in progress

Class Headers
------------
Header files should be organized as so:

```Objective-C
//Import statements first. Import in this order: superclass, frameworks, local classes
#import "XYZSuperView.h"
#import <UIKit/UIKit.h>
#import "XYZCar.h"

//Classes should be forward declared whenever possible
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
