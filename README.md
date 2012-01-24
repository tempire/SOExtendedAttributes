# SOExtendedAttributes

SOExtendedAttributes is a category on NSURL providing methods for manipulating extended attributes on a file system object.

The implementation uses [listxattr(2)](x-man-page://listxattr), [getxattr(2)](x-man-page://getxattr), [setxattr(2)](x-man-page://setxattr), and [removexattr(2)](x-man-page://removexattr).


**Introspection**

	- (BOOL) hasExtendedAttributeWithName:(NSString *)name;

**Accessing attributes individually**

	- (id) valueOfExtendedAttributeWithName:(NSString *)name error:(NSError * __autoreleasing *)outError;
	- (BOOL) setExtendedAttributeValue:(id)value forName:(NSString *)name error:(NSError * __autoreleasing *)outError;

**Accessing attributes in batch**

	- (NSDictionary *) extendedAttributesWithError:(NSError * __autoreleasing *)outError;
	- (BOOL) setExtendedAttributes:(NSDictionary *)attributes error:(NSError * __autoreleasing *)outError;

**Removing an extended attribute**

	- (BOOL) removeExtendedAttributeWithName:(NSString *)name error:(NSError * __autoreleasing *)outError;


 **Use with iCloud Backup**

 iCloud (as of iOS 5.0.1) honors the extended attribute `@"com.apple.MobileMeBackup"` as a flag to exclude a file system item from iCloud backup. This category defines the constant `iCloudDoNotBackupAttributeName` to work with this iCloud behavior.

 **Constants**

 `iCloudDoNotBackupAttributeName = @"com.apple.MobileMeBackup"`

 E.g. To determine if a file system item is marked to be excluded from iCloud backup:

    NSURL fileURL = ...;
    BOOL isExcluded = [fileURL hasExtendedAttributeWithName:iCloudDoNotBackupAttribute];


**Extended Attribute Values**

An extended attribute value can be any Foundation object that can be serialized into a property list:

- NSData
- NSString
- NSNumber
- NSArray
- NSDictionary
- NSDate

##Known Issues, Stuff & Bother

**Maximum Value Size**

The maximum size of an extended attribute value? I don't know. The Internet says anywhere from 3802 bytes to 128 KB. So the standard disclaimers apply: _Your mileage may vary. Void where prohibited. Do not operate heavy machinery._

Refs: 
[Google Search](http://www.google.com/?q=hfs%2B+extended+attributes+max+size)
- [ArsTechnica 10.4 Review](http://arstechnica.com/apple/reviews/2005/04/macosx-10-4.ars/7)
- [ArsTechnica 10.6 Review](http://arstechnica.com/apple/reviews/2009/08/mac-os-x-10-6.ars/3)
- [MacFuse Release Notes](http://code.google.com/p/macfuse/source/browse/trunk/CHANGELOG.txt)

**Non-file URLs**

An `NSInternalInconsistencyException` is thrown if these methods are invoked on a non-file URL.

**Resolving Symbolic links**
 
 These methods act on the explicitly given URL. If that URL points to a symbolic link, you'll be manipulating extended attributes on the symlink, not its original file. Use `-URLByResolvingSymlinksInPath` to get a URL pointing to the original file system item.

**ARC**

If your project is not using ARC, you will need to set a per-file build flag in Xcode on `NSURL+SOExtendedAttributes.m`:

	-fobc-arc

## Installation

For either Mac OS X or iOS projects, add these files:

	NSURL+SOExtendedAttributes.h
	NSURL+SOExtendedAttributes.m

## Compatibility

SOExtendedAttributes is compatible with Mac OS X 10.6+ and iOS 5. The clang compiler is required. The source file `NSURL+SOExtendedAttributes.m` must be compiled with ARC enabled. 

For an alternate Cocoa implementation compatible with Mac OS X 10.4 and greater, see [UKXattrMetadataStore](http://zathras.de/angelweb/sourcecode.htm).

## License

 Copyright (c) 2012, Standard Orbit Software, LLC. All rights reserved.
 
 Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:
 
 Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
 Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
 Neither the name of the Standard Orbit Software, LLC nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.
 THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.