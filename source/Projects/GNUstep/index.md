# GNUstep

## GitUp port

<https://github.com/ethanc8/GitUp>

I have been working on porting GitUp to Linux for a couple of years now. GitUp is a Git client for macOS with unique features such as a backend faster than libgit2, the ability to undo changes to the repository or revert to Time Machine-style backups, and a graph view able to handle large repositories such as the Linux kernel.

This required working to improve the GNUstep project, a project that implements a lot of NeXT and macOS APIs, providing a nice extremely portable GUI toolkit and standard library. 

### Current status

It's currently blocked due to some bugs that I have been unable to solve. I hope to get some help from other GNUstep contributors eventually.

Right now, GitUpKit and the examples (GitY, GitDown, and GitDiff) build. GitY doesn't allow you to select repositories (the file picker only allows picking files), and GitDown and GitDiff allow selecting repositories and closing them but the actual diff interface is completely missing (the main window is empty). I think some of GitUpKit's unit tests fail, but I'm not sure exactly why.

## CoreFoundation port and bridging

<https://github.com/ethanc8/CoreFoundation/tree/gnustep-bridging/CoreFoundation>

On Apple platforms, large parts of the Objective-C standard library (Foundation) are wrappers around CoreFoundation, a C library similar in scope to GLib which provides basic data structures, networking, threading, string handling, localization, and other common standard library features. Many macOS and iOS apps use CoreFoundation for these kinds of basic functions. I ported Apple's CoreFoundation to work on Linux in the GNUstep environment, and implemented bridging between CoreFoundation and Foundation datatypes so that they can be simply typecasted to each other as can be done on Apple platforms.

## Documentation work

I've worked on building the **new documentation website at <https://gnustep.github.io/>** this fall.

This builds on past work I've done to document GNUstep:
* <https://ethanc8.github.io/Sphinx-Documentation/>
* <https://ethanc8.github.io/NewDocumentation-Tutorials>
* <https://ethanc8.github.io/NewDocumentation-ObjectiveC/>
* <https://ethanc8.github.io/NewDocumentation-Make/>
* <https://ethanc8.github.io/NewDocumentation-Main/>

I did a lot of my own writing, along with a lot of revisions and reorganizations of older documentation.

## inkle-android (UIKit implementation) port

<https://github.com/ethanc8/inkle-android>

UIKit is the iOS/iPadOS and tvOS UI framework, so there are thousands of UIKit applications in the wild that might be useful to port. In the mid-2010s, Iain Merrick helped Inkle, an indie game development company, port their UIKit-based games to Android, macOS, and Windows. In order to do this, he implemented many parts of UIKit on top of SDL2. I helped to convince him and Inkle to release the code, and worked on porting it to GNUstep/Linux. I fixed many issues with the code that prevented it from building, rewrote the entrypoint to avoid using Android specific code, and redid the build system. I eventually found that it wasn't suitable for our needs, so I decided to try to work on other UIKit implementations.

## AutoBindings

<https://github.com/ethanc8/AutoBindings>

In this project, I built a binding generator for Objective-C code. It currently takes the output of the GSDoc documentation generator in order to make the language bindings. It can make correct Objective-C headers and C bindings. I was planning to make Swift and Python bindings this way, but I realized that since Swift has complicated Objective-C binding functionality for macOS already which would be difficult to implement this way, and since Python is a dynamic language, that this approach is not suitable. However, I am currently planning to make GObject bindings this way.

### Example 1

For the Objective-C method

```objc
@implementation NSObject 
- (NSMethodSignature*) methodSignatureForSelector: (SEL) aSelector ;
@end
```

here is the C binding's implementation:

```objc
NSMethodSignature* NSObject_inst_methodSignatureForSelector_(id self, SEL aSelector) {
    return (NSMethodSignature*)[(NSObject*)self methodSignatureForSelector:aSelector ];
}
```

### Example 2

The C bindings can be used like so:

```c
int main(int argc, char** argv) {
    id class_NSString = objc_lookUpClass("NSString");
    id myString = NSObject_cls_alloc(class_NSString);
    myString = NSString_inst_initWithUTF8String_(myString, "Hello, world!\n");
    id stdoutPath = NSObject_cls_alloc(class_NSString);
    stdoutPath = NSString_inst_initWithUTF8String_(stdoutPath, "/dev/stdout");
    NSString_inst_writeToFile_atomically_(myString, stdoutPath, 0);
    return 0;
}
```

This is roughly equivalent to the Objective-C code:

```objc
int main(int argc, char** argv) {
    NSString* myString = @"Hello, world!";
    [myString writeToFile: @"/dev/stdout" atomically: NO];
    return 0;
}
```

## GNUstep packaging with conda

<https://github.com/ethanc8/gnustep-forge-feedstocks>

This project was to package GNUstep with the Conda package manager. Conda makes it much easier to get a working GNUstep environment and to share these environments with others. So far, one can easily get a GNUstep environment up and running, ready to start developing. It would be quite easy to package applications developed in this environment for distribution to end users, and I am working on making that even easier using more modern Conda tools.
