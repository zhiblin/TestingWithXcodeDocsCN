本章由CocoaChina翻译小组成员dada翻译 [github地址](https://github.com/sherlockdan)

#测试基础#
测试是指检验你的应用程序代码和库代码能否成功运行的过程，用于衡量预期结果。通过执行一些操作，测试可在执行一些操作后检查一个对象的实例变量的状态，以确定你的代码在受到边界条件变化时是否会抛出一个特定异常等。

##定义测试范围
所有的软件都是组合体，也就是说，小组件组合到一起形成较大的、功能更强的高级组件，直到符合项目的需求。良好的测试需要涵盖该组合的所有功能。其中单元测试通常处理该项目功能级的小组件。而XCTest允许你为任何层次结构的各个级别的组件编写相应的测试。

测试组件由什么构成取决于你自己，可以是一个类中的一个方法，也可以是完成一个基本目的的一组方法。例如，可以是一个算数运算，就像[Quick Start](https://developer.apple.com/library/ios/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_1_quick_start/testing_1_quick_start.html#//apple_ref/doc/uid/TP40014132-CH2-SW1)一章中的计算器应用的例子。处理TableView的内容间和代码数据结构中持有的列表名称间的交互有不同的方法。每个方法和操作都意味着应用程序功能的组成部分和对其的测试。一个测试组件的行为应该是完全确定的，无论测试成功或者失败。
 
把你的应用程序的行为划分为越多的组件，就越能有效地测试你的代码是否能满足参考标准的各种细节，尤其是当项目不断增长和变化时。对于有很多组件的大型项目来说，你需要运行很多测试来彻底检测整个项目。如果可能的话，测试应该快速运行，但有一些测试确实很大，所以运行的可能会慢些。当有一些故障问题时，可以经常运行一些小型的快速的测试，这样可以轻松诊断和修复问题。

为项目组件设计测试是测试驱动开发（test-drivendevelopment，TDD）的基础，也是一种在编写代码测试之前先编写测试逻辑的编码方式。这种开发方法可以让你在实施之前确定代码需求和边界情况。编写测试后，你要开发旨在通过测试的算法。在代码通过测试后，你才有了提高代码的基础，才有信心在下一次运行这些测试时能鉴定一些预期行为（导致你的产品产生bug）的变化。

甚至在你不使用测试驱动开发时，测试还可以降低修改代码时引入的bug数量，从而帮你提高代码的特性和功能。在一款运行的应用程序中进行测试以确保未来的更改不会改变应用的行为。当你修复这些bug后，你需要添加测试以确认该bug已经被修复。测试还应该检测你的代码，所以有成功和失败两种预期，以覆盖所有边界条件。

>注意：为一个没有考虑到测试的项目添加测试可能会要求重构部分代码来使测试更加简单。“[Writing Testable Code](https://developer.apple.com/library/ios/documentation/DeveloperTools/Conceptual/testing_with_xcode/A1-guidelines_writing_testable_code/A1-guidelines_writing_testable_code.html#//apple_ref/doc/uid/TP40014132-CH8-SW1)”包含有用的编写可测试代码的简单指南。

组件可以包括你的应用程序的各部分之间的交互。由于有些类型的测试可能需要很长时间，所以你可能希望定期或者只在一台服务器上运行它们。下面一节你将看到，你可以组织自己的测试并以多种方式运行它们，以满足不同的需求。

##应用程序测试和库测试
Xcode提供两种类型的测试：应用程序测试和库测试。

应用程序测试。应用程序测试可以检查app的代码组件，比如计算器的算术运算的例子。你可以利用应用程序测试来确保你的UI控件保持原有位置，并且你的控件和控制器对象能够和对象模型正确地工作。

库测试。库测试可检查独立代码（不在应用程序中运行的代码）的行为是否正确。利用库测试，你可以将整个库的组件放在一起进行测试，通常是测试库的对象和方法。你也可以使用库测试来执行代码的压力测试，以确保它在极端的情况下也能正确执行（不大可能出现在一个运行的app中）。这些测试可以帮助你生成“强壮的”代码，即使在没有预料的情况下也可以正常运行。

##XCTest—Xcode测试框架
XCTest是Xcode 5及以上版本中使用的测试框架。如果你以前用过Xcode OCUnit测试，你可能会发现XCTest和OCUnit有些相似。XCTest是OCUnit的更现代化的替代，可以更好的与Xcode集成，为将来Xcode测试功能的改进奠定了基础。Xcode把XCTest.框架并入你的项目，而不是SenTestingKit.框架。该框架提供可以让你设计测试并在代码中运行的API。
>注意：Xcode包括一个迁移助手，可用来转换包含OCUnit测试的项目。更多OCUnit向XCTest转换的详细信息，请参阅“[Transitioning from OCUnit to XCTest](https://developer.apple.com/library/ios/documentation/DeveloperTools/Conceptual/testing_with_xcode/A3-transitioning_ocunit_to_xctest/A3-transitioning_ocunit_to_xctest.html#//apple_ref/doc/uid/TP40014132-CH10-SW1)”

##测试从哪里入手
当你开始创建测试时，记住下面的方法：

专注于测试你的代码的最基础的功能，模型类和方法，它们与控制器相交互。

应用程序的高级框图很可能会包含Model、View和Controller类别，对于每个使用Cocoa和Cocoa Touch的开发者来说，这是一个熟悉的设计模式。当你编写要覆盖所有Model类别的测试时，你必须要知道app的基础已经进行了良好的测试，而且要在编写Controller classes测试之前--它将带你接触你应用程序更复杂的部分。

作为一个可选的起始点，如果你正在编写一个框架或库，你可能想从API的接口开始。从那里，你可以按照你的方式进入内部类。该文档其它部分你可以学到如何创建、运行并使用Xcode提供的工具来调试开发项目的测试。
