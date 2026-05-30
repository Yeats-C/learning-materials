# File 及 Java 常用 IO 类指的是什么

## 📌 [File 类](ca://s?q=Java_File类是什么)
- **File 类** 是 Java 提供的用于表示文件和目录路径的抽象类。  
- 特点：  
  - 可以创建、删除文件或目录。  
  - 提供文件属性访问（如大小、读写权限、修改时间）。  
  - 不直接读写文件内容，而是作为文件路径的抽象表示。  

---

## 📌 [Java 常用 IO 类](ca://s?q=Java常用IO类有哪些)
Java 提供了丰富的 IO 类来处理文件和数据流，主要分为 **字节流** 与 **字符流** 两大类：  

### **字节流**
- **InputStream / OutputStream**：所有字节流的抽象父类。  
- **FileInputStream / FileOutputStream**：用于文件的字节读取与写入。  
- **BufferedInputStream / BufferedOutputStream**：带缓冲区的字节流，提高读写效率。  
- **DataInputStream / DataOutputStream**：支持基本数据类型的读写。  

### **字符流**
- **Reader / Writer**：所有字符流的抽象父类。  
- **FileReader / FileWriter**：用于文件的字符读取与写入。  
- **BufferedReader / BufferedWriter**：带缓冲区的字符流，支持按行读取。  
- **PrintWriter**：便捷的字符输出类，常用于文本输出。  

### **高级 IO**
- **ObjectInputStream / ObjectOutputStream**：支持对象的序列化与反序列化。  
- **RandomAccessFile**：支持随机访问文件内容。  
- **NIO（New IO）**：提供更高性能的 IO 操作，支持通道（Channel）、缓冲区（Buffer）。  

---

## 🔎 核心特点

| 类别 | **[作用](ca://s?q=Java_IO作用)** | **常见类** |
|------|-------------------------------|------------|
| **File 类** | 文件与目录路径抽象 | File |
| **字节流** | 处理二进制数据 | InputStream、OutputStream、FileInputStream |
| **字符流** | 处理文本数据 | Reader、Writer、BufferedReader |
| **高级 IO** | 对象序列化、随机访问、高性能 IO | ObjectInputStream、RandomAccessFile、NIO |

---

## 💡 总结
- **File 类**：用于文件和目录的抽象表示，管理文件属性与路径。  
- **常用 IO 类**：分为字节流与字符流，分别处理二进制数据与文本数据。  
- **高级 IO**：支持对象序列化、随机访问和高性能操作。  
- Java IO 体系为开发者提供了 **灵活且高效的文件与数据处理能力**。  
