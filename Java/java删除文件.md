  # java删除文件操作
  
  ## java删除文件
  ```
  File myObj = new File("filename.txt");

if (myObj.delete()) {
System.out.println("Deleted the file: " + myObj.getName());

} else {
System.out.println("Failed to delete the file.");

}

  ```
  * delete()除了能删除文件，也可以删除文件夹。但是，删除的文件夹必须为空

## 删除某个目录及目录下的所有子目录和文件
*删除某个目录及目录下的所有子目录和文件。File.delete()只能删除某个文件或者空目录，要想要删除某个目录及其所有子文件和子目录，要使用递归进行删除。

```
import java.io.File;

/**

* 删除某个目录及目录下的所有子目录和文件

*/

public class DelFiles {
/**

* 递归删除

* 删除某个目录及目录下的所有子目录和文件

* @param file 文件或目录

* @return 删除结果

*/

public static boolean delFiles(File file){
boolean result = false;

//目录

if(file.isDirectory()){
File[] childrenFiles = file.listFiles();

for (File childFile:childrenFiles){
result = delFiles(childFile);

if(!result){
return result;

}

}

}

//删除 文件、空目录

result = file.delete();

return result;

}

public static void main(String[] args) {
File file = new File("E:\\temp");

System.out.println("result："+delFiles(file));

}

}
```
