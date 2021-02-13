---
layout: post
title: Auto formatting for c++
subtitle: Three Steps to Setting clang-format On VSCode
cover-img: /assets/covers/blue_cover.jpg
thumbnail-img: /assets/thumbs/c_thumb.jpg
share-img: /assets/thumbs/love_to_code.png
tags: [Tutorial, C++]
readtime: true
---

Here is a simple tutorial if you want to use an auto formatting for c++ in VSCode. We are going to use clang-format to do so.

# 1º Install

make sure you have clang-format installed.

```
$ sudo apt-get install clang-format
```

# 2º Setting VSCode

Then you need to tell to VSCode to use clang format and search for your file.

Open VSCode pallet(Ctrl + Shift + P) and search for "Open Settings(JSON)".

![Tag](/assets/img/clang/settingsJson.png){: .mx-auto.d-block :}
[Tags](/assets/img/clang/settingsJson.png){: .mx-auto.d-block :}
![Tags](/assets/img/clang/settingsJson.png){: .mx-auto.d-block :}
[Tag](/assets/img/clang/settingsJson.png){: .mx-auto.d-block :}
![Image](/assets/img/clang/settingsJson.png){: .mx-auto.d-block :}

![Crepe](../assets/img/clang/settingsJson.png){: .mx-auto.d-block :}

("/assets/img/clang/settingsJson.png")

Then add those lines to your JSON.

```
"C_Cpp.clang_format_style": "file",
"C_Cpp.clang_format_fallbackStyle": "LLVM",
```

![Crepe](/assets/img/clang/clangSave.png){: .mx-auto.d-block :}

The first line tells VSCode to search for a file named ".clang-format" in the project source and parents sources.

and the second Line will use LLVM If .clang-format is not found.

IF you want to test all possible formats without setting a file you can replace the first line(or the second since you don't have a .clanf-format file) by one of these values: LLVM, Google, Chromium, Mozilla, WebKit, Microsoft and GNU.

# 3º Creating File

Now you just need to add a file named ".clang-format" in the project source or parent source.

Here you have a lot of possibilities so I'm going to show an example of one of the clang-format I use:

```
---
Language:        Cpp
AccessModifierOffset: -4
AlignAfterOpenBracket: AlwaysBreak
AlignConsecutiveAssignments: false
AlignConsecutiveDeclarations: false
AlignEscapedNewlines: Right
AlignOperands:   true
AlignTrailingComments: true
AllowAllParametersOfDeclarationOnNextLine: false
AllowShortBlocksOnASingleLine: true
AllowShortCaseLabelsOnASingleLine: true
AllowShortFunctionsOnASingleLine: All
AllowShortIfStatementsOnASingleLine: true
AllowShortLoopsOnASingleLine: true
AlwaysBreakAfterDefinitionReturnType: None
AlwaysBreakAfterReturnType: None
AlwaysBreakBeforeMultilineStrings: false
AlwaysBreakTemplateDeclarations: true
BinPackArguments: false
BinPackParameters: false
BraceWrapping:
  AfterClass:      true
  AfterControlStatement: true
  AfterEnum:       true
  AfterFunction:   true
  AfterNamespace:  true
  AfterObjCDeclaration: true
  AfterStruct:     true
  AfterUnion:      true
  AfterExternBlock: true
  BeforeCatch:     true
  BeforeElse:      true
  IndentBraces:    true
  SplitEmptyFunction: true
  SplitEmptyRecord: true
  SplitEmptyNamespace: true
BreakBeforeBinaryOperators: None
BreakBeforeBraces: Allman
BreakBeforeInheritanceComma: false
BreakBeforeTernaryOperators: true
BreakConstructorInitializersBeforeComma: false
BreakConstructorInitializers: BeforeColon
BreakAfterJavaFieldAnnotations: false
BreakStringLiterals: true
ColumnLimit:     120
CommentPragmas:  '^ IWYU pragma:'
CompactNamespaces: false
ConstructorInitializerAllOnOneLineOrOnePerLine: false
ConstructorInitializerIndentWidth: 4
ContinuationIndentWidth: 4
Cpp11BracedListStyle: true
DerivePointerAlignment: false
DisableFormat:   false
ExperimentalAutoDetectBinPacking: false
FixNamespaceComments: true
ForEachMacros:
  - foreach
  - Q_FOREACH
  - BOOST_FOREACH
IncludeBlocks:   Preserve
IncludeCategories:
  - Regex:           '^"(llvm|llvm-c|clang|clang-c)/'
    Priority:        2
  - Regex:           '^(<|"(gtest|gmock|isl|json)/)'
    Priority:        3
  - Regex:           '.*'
    Priority:        1
IncludeIsMainRegex: '(Test)?$'
IndentCaseLabels: true
IndentPPDirectives: None
IndentWidth:   4
IndentWrappedFunctionNames: false
JavaScriptQuotes: Leave
JavaScriptWrapImports: true
KeepEmptyLinesAtTheStartOfBlocks: true
MacroBlockBegin: ''
MacroBlockEnd:   ''
MaxEmptyLinesToKeep: 1
NamespaceIndentation: None
ObjCBlockIndentWidth: 2
ObjCSpaceAfterProperty: false
ObjCSpaceBeforeProtocolList: true
PenaltyBreakAssignment: 2
PenaltyBreakBeforeFirstCallParameter: 19
PenaltyBreakComment: 300
PenaltyBreakFirstLessLess: 120
PenaltyBreakString: 1000
PenaltyExcessCharacter: 1000000
PenaltyReturnTypeOnItsOwnLine: 60
PointerAlignment: Left
ReflowComments:  true
SortIncludes:    true
SortUsingDeclarations: true
SpaceAfterCStyleCast: false
SpaceAfterTemplateKeyword: true
SpaceBeforeAssignmentOperators: true
SpaceBeforeParens: ControlStatements
SpaceInEmptyParentheses: false
SpacesBeforeTrailingComments: 1
SpacesInAngles:  false
SpacesInContainerLiterals: true
SpacesInCStyleCastParentheses: false
SpacesInParentheses: false
SpacesInSquareBrackets: false
Standard:        Cpp11
TabWidth:        8
UseTab:          Never
...
```

You can search for all options here: (https://clang.llvm.org/docs/ClangFormatStyleOptions.html)

**Extra: I recommend to install C/C++ Clang Command Adapter Plugin.**
