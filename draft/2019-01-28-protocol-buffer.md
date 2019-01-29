How do I start?
1. [Download](https://github.com/protocolbuffers/protobuf) and install the protocol buffer compiler.
2. Read the [overview](https://developers.google.com/protocol-buffers/docs/overview).
3. Try the [tutorial](https://developers.google.com/protocol-buffers/docs/tutorials) for your chosen language.

---

To install protobuf, you need to install the protocol compiler (used to compile `.proto` files) and the protobuf runtime for your chosen programming language.

The simplest way to install the protoc compiler is to download pre-compiled binaries for your operating system (`protoc-<version>-<os>.zip`) from here: https://github.com/google/protobuf/releases

* Unzip this file.
* Update the environment variable PATH to include the path to the protoc binary file.

---

使用 Protocol Buffer 的第一步是定义要序列化的数据的结构，在一个 .proto 的普通文本文件中定义。Protocol Buffer 数据被结构化为 `message`，其中每个 message 是包含一系列称为 field 的 name-value 对的信息的小逻辑记录。示例如下：

```
message Person {
  string name = 1;
  int32 id = 2;
  bool has_ponycopter = 3;
}
```

