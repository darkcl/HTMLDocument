Wrapper for HTML parser of libxml2 written in Objective-C and Swift===================================================================This HTML parser gives access to libxml2 with Objective-C in Mac OS (Leopard and higher) and iOS.**The Swift version requires Xcode 6 and Mac OS 10.9+**An optional category/extension provides XPath support.libxml2 is very fast, for less overhead all recursive tasks are realized with C functions. The naming is similar to NSXMLDocument (which lacks in iOS).Unlike NSXMLDocument HTMLDocument does not inherit from HTMLNode, there is no HTMLElement class and you can't create new documents nor change nodes.All methods returning a value/object without parameter(s) are declared as read-only properties for providing dot syntax.Objective-C: Full (ARC) Automatic Reference Counting support using conditional preprocessor macros (Thanks to John Blanco of Rapture In Venice)Objective-C / Swift classes:============================- HTMLDocument- XMLDocument (inherits from HTMLDocument - Objective-C only)- HTMLNodeOptional category / extension of HTMLNode for XPath support:------------------------------------------------------------- HTMLNode+XPathHow to use:===========- Add the class files and the (optional) category/extension files to your project- Add libxml2.dylib to frameworks (Link Binary With Libraries) - not needed with module auto-load (10.9+) - Add $SDKROOT/usr/include/libxml2 to target -> Build Settings > Header Search Paths- Add -lxml2 to target ->  Build Settings -> other linker flagsObjective-C------------ import HTMLDocument.h and HTMLNode+XPath.h (if needed) header filesSwift------ add Bridging-Header.h to your project and rename it as [Modulename]-Bridging-Header.h where [Modulename] is the module name in your project (usually the project name)- enter the name of the Bridging header also in target -> Build Settings > Objective-C Bridging HeaderHTMLDocument (Objective-C only):================================Create an HTMLDocument with one of these init methodsObjective-C-----------`- (id)initWithData:(NSData *)data encoding:(NSStringEncoding )encoding error:(NSError **)error; // designated initializer``- (id)initWithContentsOfURL:(NSURL *)url encoding:(NSStringEncoding )encoding error:(NSError **)error;``- (id)initWithHTMLString:(NSString *)string encoding:(NSStringEncoding )encoding error:(NSError **)error;`For each initializer method there is also a convenience class method`+ (HTMLDocument *)documentWith…`Swift-----`init(data: NSData?, encoding: NSStringEncoding, error: NSErrorPointer)``init(contentsOfURL url: NSURL, encoding: NSStringEncoding, inout error: NSError?)``init(HTMLString string: String, encoding: NSStringEncoding, inout error: NSError?)`The corresponding initializer methods without the encoding parameter assume UTF-8 encoding.Get the root node (actually the <html> node ) or the <body> node of the document with `@property (readonly) HTMLNode *rootNode``@property (readonly) HTMLNode *body`XMLDocument:============A simple subclass XMLDocument (inherits from HTMLDocument) is added to parse also documents containing pure XML text.Internally libxml2 uses the same node type xmlNode for both HTML and XML documents anyway.HTMLNode:=========In HTMLNode search for node(s) only within the first level of children of the current node with the prefix`- (HTMLNode *)child…``- (NSArray *)children…`or search within the siblings of the current node`- (HTMLNode *)sibling…``- (NSArray *)siblings…`or perform a deep search within all descendants of the current node`- (HTMLNode *)descendant…``- (NSArray *)descendants…`the appropriate methods to search with XPath within all descendants are`- (HTMLNode *)node…``- (NSArray *)nodes…`Generic methods to search for a custom XPath are`- (HTMLNode *)nodeForXPath:(NSString *)query error:(NSError **)error;``- (NSArray *)nodesForXPath:(NSString *)query error:(NSError **)error;`There are many methods to look for tag and attribute names and values.*All Objective-C methods and properties have corresponding functions and variables in the Swift version*You can obtain the stringValue of the current text node or the textContent of all descendant text nodes as well as its integerValue, doubleValue (also with a given locale identifier) and dateValue for a format string (also with a given time zone).By default returning string values are trimmed by whitespace and newline characters. The methods starting with raw return the unfiltered values.Differences between the Objective-C and the Swift version---------------------------------------------------------Swift returns all `String` values as unwrapped strings, a potential nil value is returned as empty string, Objective-C returns value or nil.Swift maps the element types as `String` because Swift has no access to the primitive `typedef enum` C-types of libxml2, Objective-C returns the enumerated value.Swift does not support the XPath error callback function.Swift ignores by default all text nodes when using the `children` property and the `for - in [HTMLNode]` loop, to change the behaviour see `children` property and `generate()` function in HTMLNode© 2011-2014 Stefan Klieme 