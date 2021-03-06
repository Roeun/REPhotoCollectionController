# REPhotoCollectionController

REPhotoCollectionController is a simple photo thumbnail viewer for the iOS that groups photos by date.

![Screenshot of REPhotoCollectionController](https://github.com/romaonthego/REPhotoCollectionController/raw/master/Screenshot.png "REPhotoCollectionController Screenshot")

## Requirements
* Xcode 4.5 or higher
* Apple LLVM compiler
* iOS 5.0 or higher
* ARC

## Demo

First, you need to install dependencies using [CocoaPods](http://cocoapods.org/) package manager in the demo project:

``` bash
$ pod install
```

After that, build and run the `REPhotoCollectionControllerExample` project in Xcode to see `REPhotoCollectionController` in action.

If you don't have CocoaPods installed, check section "Installation" below.

## Installation

The recommended approach for installating REActivityViewController is via the [CocoaPods](http://cocoapods.org/) package manager, as it provides flexible dependency management and dead simple installation.

Install CocoaPods if not already available:

``` bash
$ [sudo] gem install cocoapods
$ pod setup
```

Edit your Podfile and add `REPhotoCollectionController`:

``` bash
$ edit Podfile
platform :ios, '5.0'
pod 'REActivityViewController', '~> 1.0'
```

Install into your Xcode project:

``` bash
$ pod install
```

Add `#include "REPhotoCollectionController.h"` to the top of classes that will use it.

## Using REPhotoCollectionController

To use `REPhotoCollectionController` copy the source code into your project then add photo objects to a data source class for your photos. Here is how:

* Add a new class to your project called "Photo" or something similar. This class MUST implement the protocol **REPhotoObjectProtocol**:

```objective-c
@protocol REPhotoObjectProtocol <NSObject>

@property (nonatomic, strong) NSDate *date;

@optional
@property (nonatomic, strong) NSURL *thumbnailURL;
@property (nonatomic, strong) UIImage *thumbnail;

- (id)initWithThumbnailURL:(NSURL *)thumbnailURL date:(NSDate *)date;

@end
```
* Implement the methods required by the protocol REPhotoObjectProtocol and implement the optional methods if needed.
* Add a new class to your project called "ThumbnailView", its parent class MUST be **REPhotoThumbnailView**. This class will be used a photo thumbnail representation view, you can customize it however you want, e.g. use different libraries for remote image fetching, etc. Sample goes below:

```objective-c
#import "ThumbnailView.h"

@implementation ThumbnailView

- (void)setPhoto:(NSObject <REPhotoObjectProtocol> *)photo
{
    if (photo.thumbnailURL) {
        imageButton.imageURL = photo.thumbnailURL;
    } else {
        [imageButton setImage:photo.thumbnail forState:UIControlStateNormal];
    }
}

- (id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        imageButton = [[EGOImageButton alloc] initWithPlaceholderImage:[UIImage imageNamed:@""]];
        imageButton.frame = CGRectMake(1, 1, self.frame.size.width - 2, self.frame.size.height - 2);
        [self addSubview:imageButton];
    }
    return self;
}

@end
```

* Getting things together:

```objective-c
NSMutableArray *datasource = [[NSMutableArray alloc] init];
Photo *photo = [[Photo alloc] init];
photo.thumbnail = [UIImage imageName:@"testimage"]; // Set photo here
photo.date = [NSDate date]; // Set date here
[datasource addObject:photo];

REPhotoCollectionController *photoCollectionController = [[REPhotoCollectionController alloc] initWithDatasource:datasource];
photoCollectionController.title = @"Photos";

// Use custom thumbnail view class
photoCollectionController.thumbnailViewClass = [ThumbnailView class];

[self.navigationController pushViewController:photoCollectionController animated:YES];
```

* Use method `reloadData` to refresh datasource when you add more photo objects.

## Contact

Roman Efimov

- https://github.com/romaonthego
- https://twitter.com/romaonthego

## License

REPhotoCollectionController is available under the MIT license.

Copyright © 2013 Roman Efimov.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

