---
layout: post
title: Xcode 6 and Code Formatting
tags: 
    - Xcode
    - ClangFormat
    - CodeFormatting
---

Code formatting is necessary for readability. Unfortunately Xcode doesn't provide code formatting tools out of the box unlike some popular IDEs as of now. That's where [ClangFormat](https://github.com/travisjeffery/ClangFormat-Xcode) come in handy. In this article I am going to explore some tools to make code formatting and your life easier.

Tool name                       | What is it?
-------------                   | -------------
[Alcatraz](http://alcatraz.io/) | Plugin manager for Xcode
ClangFormat                     | Code formatting plugin
XAlign                          | Code alignment plugin

Installing Alcatraz
-------------------

Paste the following in Terminal.

{% highlight bash %}
curl -fsSL https://raw.github.com/supermarin/Alcatraz/master/Scripts/install.sh | sh
{% endhighlight %}

Now you can manage your Xcode plugins from `Window->PackageManager` menu.

![Alcatraz Plugin Manager](/img/alcatraz_window.png)

#### Search and Install 

* ClangFormat
* XAlign

plugins.

After installation restart the Xcode. 

Go to `Edit->Clang Format` and `Edit->XAlign` menus to explore the options those formatters provide.

###ClangFormat

ClangFormat is a powerful code formatter which provides various styles of formatting options to choose from.

You might want to enable `Edit->Clang Format->Enable Format on Save`. After enabling this setting simply pressing (`⌘+S`). You can start using it Vanilla without any additional configuration. But there is a catch!

A word of caution! ClangFormat by default imposes an obsolete 80 column restriction over the length of each line of code. Meaning that if a line of code is exceeding 80 characters then that line will be broken in to multiple lines that fit under 80 characters. Some like it because it's cozy. To many devs this is a big PIA. I would not personally take sides on whether 80 character restriction is a good or bad guideline. It should be a personal preference. 

However, if you like to keep the goodies of Clang format but remove only the 80 column restriction then you have to mess with configurations.

Create a new plain text file named `.clang-format` and paste the following `.yml` configuration, or just download [this file](https://drive.google.com/file/d/0B-CC21e7Rr0ZQmE1bGhrXzBJcXM/view?usp=sharing){:target="_blank"}
(This file will be treated as a hidden file and can not be seen in `Finder` by default. You need to do {% highlight bash %}defaults write com.apple.finder AppleShowAllFiles YES{% endhighlight %} in `Terminal` and then relaunch the Finder in order to see the hidden files.)


{% highlight YAML %}

---
Language:        Cpp
# BasedOnStyle:  LLVM
AccessModifierOffset: -2
ConstructorInitializerIndentWidth: 4
AlignEscapedNewlinesLeft: false
AlignTrailingComments: true
AllowAllParametersOfDeclarationOnNextLine: true
AllowShortBlocksOnASingleLine: false
AllowShortCaseLabelsOnASingleLine: false
AllowShortIfStatementsOnASingleLine: false
AllowShortLoopsOnASingleLine: false
AllowShortFunctionsOnASingleLine: All
AlwaysBreakAfterDefinitionReturnType: false
AlwaysBreakTemplateDeclarations: false
AlwaysBreakBeforeMultilineStrings: false
BreakBeforeBinaryOperators: None
BreakBeforeTernaryOperators: true
BreakConstructorInitializersBeforeComma: false
BinPackParameters: true
BinPackArguments: true
ColumnLimit:     0
ConstructorInitializerAllOnOneLineOrOnePerLine: false
DerivePointerAlignment: false
ExperimentalAutoDetectBinPacking: false
IndentCaseLabels: false
IndentWrappedFunctionNames: false
IndentFunctionDeclarationAfterType: false
MaxEmptyLinesToKeep: 1
KeepEmptyLinesAtTheStartOfBlocks: true
NamespaceIndentation: None
ObjCSpaceAfterProperty: false
ObjCSpaceBeforeProtocolList: true
PenaltyBreakBeforeFirstCallParameter: 19
PenaltyBreakComment: 300
PenaltyBreakString: 1000
PenaltyBreakFirstLessLess: 120
PenaltyExcessCharacter: 1000000
PenaltyReturnTypeOnItsOwnLine: 60
PointerAlignment: Right
SpacesBeforeTrailingComments: 1
Cpp11BracedListStyle: true
Standard:        Cpp11
IndentWidth:     2
TabWidth:        8
UseTab:          Never
BreakBeforeBraces: Attach
SpacesInParentheses: false
SpacesInSquareBrackets: false
SpacesInAngles:  false
SpaceInEmptyParentheses: false
SpacesInCStyleCastParentheses: false
SpaceAfterCStyleCast: false
SpacesInContainerLiterals: true
SpaceBeforeAssignmentOperators: true
ContinuationIndentWidth: 4
CommentPragmas:  '^ IWYU pragma:'
ForEachMacros:   [ foreach, Q_FOREACH, BOOST_FOREACH ]
SpaceBeforeParens: ControlStatements
DisableFormat:   false
...

{% endhighlight %}

Once you have the `.clang-format` configuration file, place it in your home directory.

You can place it there via `Terminal` if you don't want to change the hidden files setting.

{% highlight bash %}
mv .clang-format ~/.clang-format
{% endhighlight %}

### Tell ClangFormat to use the configuration file

Now you just need to tell the ClangFormat plugin to use the configuration file for formatting.

You can do that by choosing `Edit->Clang Format-> File` option from Xcode menu.

![ClangFormatFileConfiguration](/img/ClangFormat_SubMenu.png)

After this step, when you do (`⌘+S`) you can see that the 80 column restriction is not imposed. yay!

That is because we have specified in the configuration file 0 for the key `ColumnLimit`. 0 stands for unlimited characters per line. You can change this value to suit your formatting needs.

### XAlign

This is another awesome formatting tool. It can align your `=` symbols in a straight line, giving your code an extremely neat look. Have a look the code that is aligned using ClangFormat first, then XAlign after.

{% highlight ObjectiveC %}

- (void)takePhoto {
if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {
hasLoadedCamera                 = YES;
UIImagePickerController *picker = [[NonRotatingUIImagePickerController alloc] init];
picker.sourceType               = UIImagePickerControllerSourceTypeCamera;
picker.delegate                 = self;
picker.allowsEditing            = NO;
picker.cameraCaptureMode        = UIImagePickerControllerCameraCaptureModePhoto;

[self presentViewController:picker animated:YES completion:NULL];
}
}

{% endhighlight %}

Select some or all text in a file. Use `⌘+⇧+X` shortcut key to pretty align the code.

After doing this you may want to do `Clang Format -> Disable Format on Save` in order to stop Clang Format from changing the alignment.