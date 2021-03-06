
# Language Guide (proto3)

# 语言指南（proto3）

- Defining A Message Type
- 定义消息类型

- Scalar Value Types
- 标量值类型

- Default Values
- 默认值

- Enumerations
- 枚举

- Using Other Message Types
- 使用其他消息类型

- Nested Types
- 嵌套类型

- Updating A Message Type
- 更新消息类型

- Unknown Fields
- 未知字段

- Any
- Any

- Oneof
- Oneof

- Maps
- 映射

- Packages
- 包

- Defining Services
- 定义服务

- JSON Mapping
- JSON 映射

- Options
- 选项

- Generating Your Classes
- 生成你的类

This guide describes how to use the protocol buffer language to structure your protocol buffer data, including .proto file syntax and how to generate data access classes from your .proto files. It covers the proto3 version of the protocol buffers language: for information on the proto2 syntax, see the [Proto2 Language Guide](https://developers.google.com/protocol-buffers/docs/proto).

本指南描述了如何使用 protocol buffer 语言来组织你的 protocol buffer 数据，包括 .proto 文件句法以及如何从你的 .proto 文件生成数据访问类。它适用于 protocol buffers 语言的 **proto3** 版本：想要了解 **proto2** 的句法，请查看 [Proto2 语言指南](https://developers.google.com/protocol-buffers/docs/proto)。

This is a reference guide – for a step by step example that uses many of the features described in this document, see the tutorial for your chosen language (currently proto2 only; more proto3 documentation is coming soon).

这是一个参考指南——如果想要获取一个使用本篇文档描述的诸多功能的、按步骤来的示例，请查看你选择的语言的[教程](https://developers.google.com/protocol-buffers/docs/tutorials)（目前仅有 proto2；更多 proto3 的文档很快到来）。



