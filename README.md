#  SyntaxHighlighting for leaf files

Just run this project one time and restart XCode


## Source

https://stackoverflow.com/questions/9050035/how-to-make-xcode-recognize-a-custom-file-extension-as-objective-c-for-syntax-hi

https://stackoverflow.com/questions/41961953/adding-syntax-coloring-to-xcode-for-unknown-file-extension-such-as-tcl


Xcode determines how to represent a file in its user interface based on the file's Uniform Type Identifier. As far as I know it's not possible to add additional file extension tags to an existing UTI, but you can declare a new UTI that conforms to the type you want to map to. The system will then associate the specified file extension(s) with your new UTI and through conformance Xcode and every other UTI-aware application will recognize the files as source code of the mapped type.

You may want to give some thought to where to declare to the new UTI. For example, if files of this type are being created by a tool the bundle for that tool would be the most appropriate location. In the absence of a better alternative you can create a stub application bundle and declare the new UTI there:

Create a new Cocoa Application project in Xcode.
In project settings, select the application target, then the Info tab.
Create a new Exported UTI. UTI definition example
Set the Identifier field to a unique name using reverse DNS notation for a domain that you control. For example, com.yourdomain.objective-c-source.
Set the Conforms To field to the UTI that you want to map to, such as public.objective-c-source. You can find this by browsing the list of system-declared UTIs or those exported in Xcode's Info.plist.
Set the Extensions field to the comma-separated list of extensions that you want to associate with the new UTI.
Commit the change to the last field by pressing return or moving the focus to a different field.
Build and run the application to register it with Launch Services.
Restart Xcode.
Xcode should now use appropriate syntax highlighting for files with the specified extension(s).

If this doesn't work, check the Info.plist of the built application to ensure that all of the expected information is there without any trailing whitespace. You can also check that the UTI has been registered using lsregister:

/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -dump
Search the output for your UTI's identifier and verify that it is present and active.
