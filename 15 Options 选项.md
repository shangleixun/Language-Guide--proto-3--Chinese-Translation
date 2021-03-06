
# Options

# Options（选项） 

Individual declarations in a .proto file can be annotated with a number of options. Options do not change the overall meaning of a declaration, but may affect the way it is handled in a particular context. The complete list of available options is defined in google/protobuf/descriptor.proto.

在 `.proto` 文件中，单独的声明可添加若干选项。选项并不改变声明的整体含义，但可能会影响它在特定的上下文中被处理的方式。完整的可用选项列表定义在 `google/protobuf/descriptor.proto` 中（**按**位置已在 3.12.0 中更改为相应语言版本根目录下的 `GPBDescriptor` 中）。

Some options are file-level options, meaning they should be written at the top-level scope, not inside any message, enum, or service definition. Some options are message-level options, meaning they should be written inside message definitions. Some options are field-level options, meaning they should be written inside field definitions. Options can also be written on enum types, enum values, oneof fields, service types, and service methods; however, no useful options currently exist for any of these.

一些选项是文件级（file-level）的选项，意味着它们应当被编写在最顶层的作用域中，而不是任一消息，枚举，或服务定义的内部。一些选项是消息级（message-level）的选项，意味着它们应当被编写在消息定义的内部。一些选项是字段级（field-level）的选项，意味着它们应当被编写在字段定义的内部。选项也可以被编写在枚举类型，枚举值，oneof 字段，服务类型，以及服务方法上；然而，对于（论及到的）这几个中的任何一个而言，目前并不存在有用的选项。

Here are a few of the most commonly used options:

这里是一些最常用到的选项：

* `java_package` (file option): The package you want to use for your generated Java classes. If no explicit `java_package` option is given in the `.proto` file, then by default the proto package (specified using the "package" keyword in the .proto file) will be used. However, proto packages generally do not make good Java packages since proto packages are not expected to start with reverse domain names. If not generating Java code, this option has no effect.

* `java_package`（文件选项）：你想为你的生成的 Java 类而使用的包。如果在 `.proto` 文件中没有给定显式的 `java_package` 选项，那么默认就会使用 proto 包（在 .proto 文件中使用“package”关键字指定的）。然而，通常 proto 包并不能制作良好的 Java 包，因为 proto 包预期并不以反向域名开始。如果不生成 Java 代码，此选项就没有作用。

    ```proto
    option java_package = "com.example.foo";
    ```


* `java_multiple_files` (file option): Causes top-level messages, enums, and services to be defined at the package level, rather than inside an outer class named after the `.proto` file.

* `java_multiple_files`（文件选项）：会使最顶层的消息，枚举和服务以包的层级来定义，而不是在以 `.proto` 文件命名的一个外部类的内部。

    ```proto
    option java_multiple_files = true;
    ```

* `java_outer_classname` (file option): The class name for the outermost Java class (and hence the file name) you want to generate. If no explicit `java_outer_classname` is specified in the `.proto` file, the class name will be constructed by converting the `.proto` file name to camel-case (so `foo_bar.proto` becomes `FooBar.java`). If not generating Java code, this option has no effect.

* `java_outer_classname`（文件选项）：你想生成的最外层的 Java 类（因此也是文件名）的类名。如果在 `.proto` 文件中没有给定显式的 `java_outer_classname` 选项，类名会通过将 `.proto` 文件名转换为驼峰命名的方式（所以 `foo_bar.proto` 变成了 `FooBar.java`）来构建。

    ```proto
    option java_outer_classname = "Ponycopter";
    ```

* `optimize_for` (file option): Can be set to SPEED, CODE_SIZE, or LITE_RUNTIME. This affects the C++ and Java code generators (and possibly third-party generators) in the following ways:

* `optimize_for`（文件选项）：可被设为 `SPEED`、`CODE_SIZE` 或 `LITE_RUNTIME`。此选项会通过下面几种方式来影响 C++ 和 Java 的代码生成器（第三方生成器也可能会影响到）：

    * SPEED (default): The protocol buffer compiler will generate code for serializing, parsing, and performing other common operations on your message types. This code is highly optimized.

    * `SPEED`（默认）：protocol buffer 编译器会在你的消息类型上生成用于序列化、解析以及执行其他常用操作的代码。此代码是高度优化的。

    * CODE_SIZE: The protocol buffer compiler will generate minimal classes and will rely on shared, reflection-based code to implement serialialization, parsing, and various other operations. The generated code will thus be much smaller than with SPEED, but operations will be slower. Classes will still implement exactly the same public API as they do in SPEED mode. This mode is most useful in apps that contain a very large number .proto files and do not need all of them to be blindingly fast.

    * `CODE_SIZE`：protocol buffer 编译器会生成最少的类，并且会依赖共享的、基于反射机制的代码，来实现（implement）序列化、解析和其他各种操作。生成的代码量因此会比用 `SPEED` 选项还要小得多，但运行速度会慢一点。类仍然会像它们在 `SPEED` 模式中所做的那样，精确地实现相同的公开 API。在那种包含了极大数量的 `.proto` 文件，但并不需要它们所有都眩目般快的 app 中，这个模式最有用。

    * LITE_RUNTIME: The protocol buffer compiler will generate classes that depend only on the "lite" runtime library (libprotobuf-lite instead of libprotobuf). The lite runtime is much smaller than the full library (around an order of magnitude smaller) but omits certain features like descriptors and reflection. This is particularly useful for apps running on constrained platforms like mobile phones. The compiler will still generate fast implementations of all methods as it does in SPEED mode. Generated classes will only implement the MessageLite interface in each language, which provides only a subset of the methods of the full Message interface.

    * `LITE_RUNTIME`：protocol buffer 编译器仅会依靠“lite”版的运行时库（`libprotobuf-lite`，而非 `libprotobuf`）来生成类。轻量运行时比全量的库小很多（大约小一个数量级），但删减了确定的（certain）一些功能，像描述器和反射机制。这点对运行在受限的平台像手机上的 app 是尤其有用的。编译器仍会像它在 `SPEED` 模式中所做的那样，生成所有方法的快速实现代码。每种语言里，生成的类只会实现 `MessageLite` 接口，这个接口只提供完整的 `Message` 接口的方法的一个子集。

    ```proto
    option optimize_for = CODE_SIZE;
    ```

* `cc_enable_arenas` (file option): Enables arena allocation for C++ generated code.

* `cc_enable_arenas`（文件选项）：为 C++ 的生成代码启用 [arena allocation](https://developers.google.com/protocol-buffers/docs/reference/arenas)。

* `objc_class_prefix` (file option): Sets the Objective-C class prefix which is prepended to all Objective-C generated classes and enums from this .proto. There is no default. You should use prefixes that are between 3-5 uppercase characters as recommended by Apple. Note that all 2 letter prefixes are reserved by Apple.

* `objc_class_prefix`（文件选项）：设置 Objective-C 类的前缀，此前缀会置于从这个 .proto 生成的所有 Objective-C 的类和枚举的前面。无默认前缀。你应当使用由 [Apple 推荐的](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Conventions/Conventions.html#//apple_ref/doc/uid/TP40011210-CH10-SW4) 3-5 位的大写字母。请注意，所有两位字母的前缀已由 Apple 保留（使用权）。

* `deprecated` (field option): If set to `true`, indicates that the field is deprecated and should not be used by new code. In most languages this has no actual effect. In Java, this becomes a @Deprecated annotation. In the future, other language-specific code generators may generate deprecation annotations on the field's accessors, which will in turn cause a warning to be emitted when compiling code which attempts to use the field. If the field is not used by anyone and you want to prevent new users from using it, consider replacing the field declaration with a reserved statement.

* `deprecated`（字段选项）：如设为 `true`，则表明本字段已弃用（deprecated），并且新代码不该再使用了。在大多数语言中，这个选项没有实际的作用。在 Java 中，它会变成一个 `@Deprecated` 的注释。未来，其他的、特定语言的代码生成器可能会在这个字段的访问器处生成弃用注释，这样，在编译试图使用这个字段的代码时，反而会使编译器弹出来一条警告。如果这个字段没有人用了，而且你想阻止新的用户使用它，考虑用一条[保留](https://developers.google.com/protocol-buffers/docs/proto3#reserved)语句来替换字段的声明。

    ```proto
    int32 old_field = 6 [deprecated = true];
    ```

## Custom Options

## 自定义选项

Protocol Buffers also allows you to define and use your own options. This is an advanced feature which most people don't need. If you do think you need to create your own options, see the Proto2 Language Guide for details. Note that creating custom options uses extensions, which are permitted only for custom options in proto3.

Protocol Buffers 也允许你定义和使用你自己的选项。这是一个**高阶功能**——大多数人用不到。如果你确实认为你需要创建你自己的选项，请查看 [Proto2 语言指南](https://developers.google.com/protocol-buffers/docs/proto#customoptions)来获取更多细节。注意，请使用[扩展](https://developers.google.com/protocol-buffers/docs/proto#extensions)来创建自定义选项——扩展只允许在 proto3 的自定义选项中使用。
