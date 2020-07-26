## 1、POI 和 easyExcel 讲解

> 常用进程

1、将用户信息导出为excel表格（导出数据....）

2、将Excel表中的信息录入到网站数据库（习题上传....）

开发中经常会设计到excel的处理，如导出Excel，导入Excel到数据库中！

操作Excel目前比较流行的就是 **Apache POI** 和 阿里巴巴的 **easyExcel** ！

> Apache POI

Apache POI 官网：https://poi.apache.org/

![image-20200421153707641](.\POI和easyExcel.assets\image-20200421153707641.png)  

![image-20200421153734332](.\POI和easyExcel.assets\image-20200421153734332.png)  



> easyExcel

easyExcel 官网地址：https://github.com/alibaba/easyexcel



![image-20200421154301766](.\POI和easyExcel.assets\image-20200421154301766.png)  



EasyExcel 是阿里巴巴开源的一个excel处理框架，**以使用简单、节省内存著称**。

EasyExcel 能大大减少占用内存的主要原因是在解析 Excel 时没有将文件数据一次性全部加载到内存中，而是从磁盘上一行行读取数据，逐个解析。

下图是 EasyExcel 和 POI 在解析Excel时的对比图。

![img](.\POI和easyExcel.assets\e3a3500014c95f7118d8c200a51acab4.png) 

官方文档：https://www.yuque.com/easyexcel/doc/easyexcel



## 2、POI-Excel写

> 创建新项目

1、建立一个空项目 POI-easyExcel，创建普通Maven的Moudle  poi-easyExcel

2、引入pom依赖

```xml
<dependencies>
    <!--xls(03)-->
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>3.9</version>
    </dependency>

    <!--xlsx(07)-->
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>3.9</version>
    </dependency>
    
    <!--日期格式化工具-->
    <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time</artifactId>
        <version>2.10.1</version>
    </dependency>

    <!--test-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```

> 日期格式化工具的使用`String date = new DateTime().toString("yyyy-MM-dd HH:mm:ss");`
>
> 03 | 07 版本的写，就是对象不同，方法一样的！

==需要注意：2003 版本和 2007 版本存在兼容性的问题！03最多只有 65535 行！==

![image-20200421231630486](.\POI和easyExcel.assets\image-20200421231630486.png) 

1、工作簿：  2、工作表：  3、行： 4、列：

### 2.1、03版本：

```java
// 存放路径
String Path = "D:\\IDEA2019\\IdeaProjects\\POI-easyExcel\\";

@Test
public void testWrite03() throws IOException {
    // 1、创建一个工作簿(面向接口编程，比较方便维护)
    Workbook workbook = new HSSFWorkbook();
    // 2、创建一个工作表
    Sheet sheet = workbook.createSheet();
    // 3、创建一个行
    Row row1 = sheet.createRow(0);
    // 4、创建一个单元格(1,1)
    Cell cell11 = row1.createCell(0);
    cell11.setCellValue("姓名");
    // (1,2)
    Cell cell12 = row1.createCell(1);
    cell12.setCellValue("晓江");

    // 第二行
    Row row2 = sheet.createRow(1);
    Cell cell21 = row2.createCell(0);
    cell21.setCellValue("时间");
    // (2,2)
    Cell cell22 = row2.createCell(1);
    String date = new DateTime().toString("yyyy-MM-dd HH:mm:ss");
    cell22.setCellValue(date);

    // 生成一张表（IO流）  03边的就是使用xls结尾
    FileOutputStream fileOutputStream = new FileOutputStream(Path + "用户.xls");
    // 输出
    workbook.write(fileOutputStream);
    // 关闭流
    fileOutputStream.close();
    System.out.println("生成完毕！");
}
```

数据批量导入！

> 大文件写HSSF

缺点：最多只能处理65536行，否则会抛出异常

```bash
java.lang.IllegalArgumentException: Invalid row number (65536) outside allowable range (0..65535)
```

优点：过程中写入缓存，不操作磁盘，最后一次性写入磁盘，速度快

```java
@Test
public void testWrite03BigData() throws IOException {
    // 时间
    long begin = System.currentTimeMillis();

    // 创建一个薄
    Workbook workbook = new HSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();
    // 写入数据
    for (int rowNum = 0; rowNum < 65537; rowNum++) {
        Row row = sheet.createRow(rowNum);
        for (int cellNum = 0; cellNum < 10 ; cellNum++) {
            Cell cell = row.createCell(cellNum);
            cell.setCellValue(cellNum);
        }
    }
    System.out.println("over");
    FileOutputStream outputStream = new FileOutputStream(Path + "testWrite03BigData.xls");
    workbook.write(outputStream);
    outputStream.close();
    long end = System.currentTimeMillis();
    System.out.println((double) (end-begin)/1000);
}
```





### 2.2、07版本：

```java
// 存放路径
String Path = "D:\\IDEA2019\\IdeaProjects\\POI-easyExcel\\";

@Test
public void testWrite07() throws IOException {
    // 1、创建一个工作簿(面向接口编程，比较方便维护)
    Workbook workbook = new XSSFWorkbook();
    // 2、创建一个工作表
    Sheet sheet = workbook.createSheet();
    // 3、创建一个行
    Row row1 = sheet.createRow(0);
    // 4、创建一个单元格(1,1)
    Cell cell11 = row1.createCell(0);
    cell11.setCellValue("姓名");
    // (1,2)
    Cell cell12 = row1.createCell(1);
    cell12.setCellValue("晓江");

    // 第二行
    Row row2 = sheet.createRow(1);
    Cell cell21 = row2.createCell(0);
    cell21.setCellValue("时间");
    // (2,2)
    Cell cell22 = row2.createCell(1);
    String date = new DateTime().toString("yyyy-MM-dd HH:mm:ss");
    cell22.setCellValue(date);

    // 生成一张表（IO流）  03边的就是使用xls结尾
    FileOutputStream fileOutputStream = new FileOutputStream(Path + "用户.xlsx");
    // 输出
    workbook.write(fileOutputStream);
    // 关闭流
    fileOutputStream.close();
    System.out.println("生成完毕！");
}
```

注意对象的一个区别，文件后缀！

> 大文件写XSSF

缺点：写数据时速度非常慢，非常耗内存，也会发生内存溢出，如100万条

优点：可以写较大的数据量，如20万条

```java
@Test
public void testWrite07BigData() throws IOException {
    // 时间
    long begin = System.currentTimeMillis();

    // 创建一个薄
    Workbook workbook = new XSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();
    // 写入数据
    for (int rowNum = 0; rowNum < 100000; rowNum++) {
        Row row = sheet.createRow(rowNum);
        for (int cellNum = 0; cellNum < 10 ; cellNum++) {
            Cell cell = row.createCell(cellNum);
            cell.setCellValue(cellNum);
        }
    }
    System.out.println("over");
    FileOutputStream outputStream = new FileOutputStream(Path + "testWrite07BigData.xlsx");
    workbook.write(outputStream);
    outputStream.close();
    long end = System.currentTimeMillis();
    System.out.println((double) (end-begin)/1000);
}
```



> 大文件写SXSSF

优点：可以写非常大的数据量，如100万条甚至更多条，写数据速度快，占用更少的内存

**注意：**

过程中会产生临时文件，需要清理临时文件

默认由100条记录被保存在内存中，如果超过这数量，则最前面的数据被写入临时文件

如果想自定义内存中数据的数量，可以使用new SXSSFWorkbook ( 数量 )

```java
@Test
public void testWrite07BigDataS() throws IOException {
    // 时间
    long begin = System.currentTimeMillis();

    // 创建一个薄
    Workbook workbook = new SXSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();
    // 写入数据
    for (int rowNum = 0; rowNum < 100000; rowNum++) {
        Row row = sheet.createRow(rowNum);
        for (int cellNum = 0; cellNum < 10 ; cellNum++) {
            Cell cell = row.createCell(cellNum);
            cell.setCellValue(cellNum);
        }
    }
    System.out.println("over");
    FileOutputStream outputStream = new FileOutputStream(Path + "testWrite07BigDataS.xlsx");
    workbook.write(outputStream);
    outputStream.close();
    // 清除临时文件！
    ((SXSSFWorkbook) workbook).dispose();
    long end = System.currentTimeMillis();
    System.out.println((double) (end-begin)/1000);
}
```

SXSSFWorkbook-来至官方的解释：实现“BigGridDemo”策略的流式XSSFWorkbook版本。这允许写入非常大的文件而不会耗尽内存，因为任何时候只有可配置的行部分被保存在内存中。

请注意，仍然可能会消耗大量内存，这些内存基于您正在使用的功能，例如合并区域，注释......仍然只存储在内存中，因此如果广泛使用，可能需要大量内存。

再使用  POI的时候！内存问题 Jprofile！





## 3、POI-Excel读

### 3.1、03版本

```java
@Test
public void testRead03() throws Exception {
    // 获取文件流
    FileInputStream inputStream = new FileInputStream(Path + "用户.xls");

    // 1、创建一个工作簿。 使用excel能操作的这边他都可以操作！
    Workbook workbook = new HSSFWorkbook(inputStream);
    // 2、得到表
    Sheet sheet = workbook.getSheetAt(0);
    // 3、得到行
    Row row = sheet.getRow(0);
    // 4、得到列
    Cell cell = row.getCell(1);

    // 读取值的时候，一定需要注意类型！
    // getStringCellValue 字符串类型
    System.out.println(cell.getStringCellValue());
    inputStream.close();
}
```



### 3.2、07版本

```Java
@Test
public void testRead07() throws Exception {

    // 获取文件流
    FileInputStream inputStream = new FileInputStream(Path + "用户.xlsx");

    // 1、创建一个工作簿。 使用excel能操作的这边他都可以操作！
    Workbook workbook = new XSSFWorkbook(inputStream);
    // 2、得到表
    Sheet sheet = workbook.getSheetAt(0);
    // 3、得到行
    Row row = sheet.getRow(0);
    // 4、得到列
    Cell cell = row.getCell(1);

    // 读取值的时候，一定需要注意类型！
    // getStringCellValue 字符串类型
    System.out.println(cell.getStringCellValue());
    inputStream.close();
}
```

==注意获取值的类型即可==



### 3.3、读取不同的数据类型

（最麻烦的就是这里了！）

```java
@Test
public void testCellType() throws Exception {
    // 获取文件流
    FileInputStream inputStream = new FileInputStream(Path + "明细表.xls");
    // 创建一个工作簿。 使用excel能操作的这边他都可以操作！
    Workbook workbook = new HSSFWorkbook(inputStream);
    Sheet sheet = workbook.getSheetAt(0);
    // 获取标题内容
    Row rowTitle = sheet.getRow(0);
    if (rowTitle != null) {
        // 一定要掌握
        // getPhysicalNumberOfCells 获取总列数
        int cellCount = rowTitle.getPhysicalNumberOfCells();
        for (int cellNum = 0; cellNum < cellCount; cellNum++) {
            Cell cell = rowTitle.getCell(cellNum);
            if (cell != null){
                String cellValue = cell.getStringCellValue();
                System.out.print(cellValue + " | ");
            }
        }
        System.out.println();
    }

    // 获取表中的内容
    // getPhysicalNumberOfRows获取总行数
    int rowCount = sheet.getPhysicalNumberOfRows();
    for (int rowNum = 1; rowNum < rowCount ; rowNum++) {
        Row rowData = sheet.getRow(rowNum);
        if (rowData != null){
            // 读取列
            int cellCount = rowTitle.getPhysicalNumberOfCells();
            for (int cellNum = 0; cellNum < cellCount ; cellNum++) {
                System.out.print("[" +(rowNum+1) + "-" + (cellNum+1) + "]");

                Cell cell = rowData.getCell(cellNum);
                // 匹配列的数据类型
                if (cell!=null) {
                    int cellType = cell.getCellType();
                    String cellValue = "";

                    switch (cellType) {
                        case HSSFCell.CELL_TYPE_STRING: // 字符串
                            System.out.print("【String】");
                            cellValue = cell.getStringCellValue();
                            break;
                        case HSSFCell.CELL_TYPE_BOOLEAN: // 布尔
                            System.out.print("【BOOLEAN】");
                            cellValue = String.valueOf(cell.getBooleanCellValue());
                            break;
                        case HSSFCell.CELL_TYPE_BLANK: // 空
                            System.out.print("【BLANK】");
                            break;
                        case HSSFCell.CELL_TYPE_NUMERIC: // 数字（日期、普通数字）
                            System.out.print("【NUMERIC】");
                            if (HSSFDateUtil.isCellDateFormatted(cell)){ // 日期
                                System.out.print("【日期】");
                                Date date = cell.getDateCellValue();
                                cellValue = new DateTime(date).toString("yyyy-MM-dd");
                            }else {
                                // 不是日期格式，防止数字过长！
                                System.out.print("【转换为字符串输出】");
                                cell.setCellType(HSSFCell.CELL_TYPE_STRING);
                                cellValue = cell.toString();
                            }
                            break;
                        case HSSFCell.CELL_TYPE_ERROR:
                            System.out.print("【数据类型错误】");
                            break;
                    }
                    System.out.print(cellValue);
                }
            }
        }
        System.out.println();
    }
    inputStream.close();
}
```

注意，类型转换问题；





### 3.4、计算公式 

（了解即可！）

```java
@Test
public void testFormula() throws Exception {
    FileInputStream inputStream = new FileInputStream(Path + "公式.xls");
    Workbook workbook = new HSSFWorkbook(inputStream);
    Sheet sheet = workbook.getSheetAt(0);

    Row row = sheet.getRow(4);
    Cell cell = row.getCell(0);

    // 拿到计算公司 eval
    FormulaEvaluator FormulaEvaluator = new HSSFFormulaEvaluator((HSSFWorkbook)workbook);

    // 输出单元格的内容
    int cellType = cell.getCellType();
    switch (cellType){
        case Cell.CELL_TYPE_FORMULA: // 公式
            String formula = cell.getCellFormula();
            System.out.println(formula);

            // 计算
            CellValue evaluate = FormulaEvaluator.evaluate(cell);
            String cellValue = evaluate.formatAsString();
            System.out.println(cellValue);
            break;
    }

}
```



## 4、EasyExcel操作

> 导入依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>easyexcel</artifactId>
    <version>2.2.0-beta2</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
<!--test-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```



> 写入测试

https://www.yuque.com/easyexcel/doc/read	

> 读取测试

https://www.yuque.com/easyexcel/doc/read



固定套路：

1、写入，固定类格式进行写入

2、读取，根据监听器设置的规则进行读取！



## 5、学习方式

1. 写数据一般步骤：创建工作簿、创建工作表、创建一行、创建一列、通过文件输出流保存
2. 读数据一般步骤：通过文件输入流读取工作簿、得到工作表、得到行、得到列、取出数值（注意类型）
3. 了解，面向对象的思想，学会面向接口编程！
4. 理解使用测试API！

5. 把**EasyExcel** 的所有api都测试一下！



## 6、总结：

- 创建表格

1. 创建03版的工作簿

   ```java
   Workbook workbook = new HSSFWorkbook();
   ```

2. 创建07版的工作簿

   ```java
   Workbook workbook = new XSSFWorkbook();
   ```

3. 创建07版的升级版

   ```java
   Workbook workbook = new SXSSFWorkbook();
   ```

   - 这个需要清除临时文件！

     ```java
     ((SXSSFWorkbook) workbook).dispose();
     ```

4. 创建工作表

   ```java
   Sheet sheet = workbook.createSheet();
   ```

5. 创建行

   ```java
   Row row1 = sheet.createRow(0); 
   ```

   创建列

   ```java
   Cell cell11 = row1.createCell(0);
   ```

6. 设置值

   ```java
   cell11.setCellValue("姓名");
   ```

   

- 读取表格

1. 通过文件输入流获取工作簿 `Workbook workbook = new HSSFWorkbook(inputStream);`

2. 获取工作簿 `Sheet sheet = workbook.getSheetAt(0);`

3. 获取行 `Row rowTitle= sheet.getRow(0);`

4. 获取总列数 `int cellCount = rowTitle.getPhysicalNumberOfCells();`

5. 获取列 `Cell cell = rowTitle.getCell(0);`

6. 获取单元格类型 `int cellType = cell.getCellType();`

   - `case HSSFCell.CELL_TYPE_STRING:`

   ```java
   int CELL_TYPE_NUMERIC = 0;		// 返回0为数字
   int CELL_TYPE_STRING = 1;		// 返回1为字符串
   int CELL_TYPE_FORMULA = 2;		// 返回2为公式
   int CELL_TYPE_BLANK = 3;		// 返回3为空
   int CELL_TYPE_BOOLEAN = 4;		// 返回4为布尔
   int CELL_TYPE_ERROR = 5;		// 返回5为错误
   ```

7. 读取公式

   1. ```java
      // 拿到计算公式 eval
      FormulaEvaluator FormulaEvaluator = new HSSFFormulaEvaluator((HSSFWorkbook)workbook);
      ```

   2. ```java
      // 输出公式
      String formula = cell.getCellFormula();
      System.out.println(formula);
      ```

   3. ```java
      // 输出计算值
      CellValue evaluate = FormulaEvaluator.evaluate(cell);
      String cellValue = evaluate.formatAsString();
      System.out.println(cellValue);
      ```



- 使用easyExcel

1. 需要一个实体类DemoData
2. 一个监听器DemoDataListener extends AnalysisEventListener<DemoData>
3. 一个数据访问类DemoDAO
4. 测试类EasyTest

```
* 最简单的读
* <p>1. 创建excel对应的实体对象 参照{@link DemoData}
* <p>2. 由于默认一行行的读取excel，所以需要创建excel一行一行的回调监听器，参照{@link DemoDataListener}
* <p>3. 直接读即可
```

https://www.yuque.com/easyexcel/doc



## 7、合并Excel工作簿

> 以后收集成绩等信息就可以这样使用了

1. 在当前目录的下创建一个空的Excel表格

   ![image-20200531144705016](.\POI和easyExcel.assets\image-20200531144705016.png)

2. 开启开发工具

   ![image-20200531143418272](.\POI和easyExcel.assets\image-20200531143418272.png)

   ![image-20200531143500387](.\POI和easyExcel.assets\image-20200531143500387.png)

3. 打开编译界面

   ![image-20200531143549893](.\POI和easyExcel.assets\image-20200531143549893.png)

4. 写上如下程序（左侧那里记得选中需要合并到的表中）

   ![image-20200531143625465](.\POI和easyExcel.assets\image-20200531143625465.png)

   - **注意：下面的第7行代码`MN = Dir(MP & "\" & "*.xlsx")` 用于设置读取的是所有xlxs（07版）的Excel文件**
   - **如果读取的文件为03版，请设置为`MN = Dir(MP & "\" & "*.xls")` **
   - **读取全部文件则设置为MN = Dir(MP & "\" & "*")**
   
   ```vb
   Sub 合并目录所有工作簿全部工作表()
       Dim MP, MN, AW, Wbn, wn
       Dim Wb As Workbook
       Dim i, a, b, d, c, e
       Application.ScreenUpdating = False
       MP = ActiveWorkbook.Path
       MN = Dir(MP & "\" & "*.xlsx")
       AW = ActiveWorkbook.Name
       Num = 0
       e = 1
       Do While MN <> ""
           If MN <> AW Then
               Set Wb = Workbooks.Open(MP & "\" & MN)
                   a = a + 1
                   With Workbooks(1).ActiveSheet
                   For i = 1 To Sheets.Count
                   If Sheets(i).Range("a1") <> "" Then
                       Wb.Sheets(i).Range("a1").Resize(1, Sheets(i).UsedRange.Columns.Count).Copy .Cells(1, 1)
                       d = Wb.Sheets(i).UsedRange.Columns.Count
                       c = Wb.Sheets(i).UsedRange.Rows.Count - 1
                       wn = Wb.Sheets(i).Name
                       .Cells(1, d + 1) = "表名"
                       .Cells(e + 1, d + 1).Resize(c, 1) = MN & wn
                       e = e + c
                       Wb.Sheets(i).Range("a2").Resize(c, d).Copy .Cells(.Range("a1048576").End(xlUp).Row + 1, 1)
           End If
       Next
       Wbn = Wbn & Chr(13) & Wb.Name
       Wb.Close False
   End With
   End If
   MN = Dir
   Loop
   Range("a1").Select
   Application.ScreenUpdating = True
   MsgBox "共合并了" & a & "个工作薄下全部工作表。如下：" & Chr(13) & Wbn, vbInformation, "提示"
   End Sub
   ```

5. 然后点回原来的Excel中，选择宏

   ![image-20200531143729862](.\POI和easyExcel.assets\image-20200531143729862.png)
   
6. 单击执行，然后等待一会就可以了

   ![image-20200531143758067](.\POI和easyExcel.assets\image-20200531143758067.png)

7. 对代码的解释

   ```vb
   Sub 合并目录所有工作簿全部工作表()
   
       ' 定义变量
       Dim MP, MN, AW, Wbn, wn
       Dim Wb As Workbook
       Dim i, a, b, d, c, e
       Application.ScreenUpdating = False
       ' 获取路径MP
       MP = ActiveWorkbook.Path
       ' 获取所有xlsx文件MN
       MN = Dir(MP & "\" & "*.xlsx")
       ' 获取工作簿的名字AW
       AW = ActiveWorkbook.Name
       Num = 0
       e = 1
       ' 遍历所有文件，如果文件名不为空
       Do While MN <> ""
          ' 如果文件名不等于当前工作簿的名字
           If MN <> AW Then
               ' 打开该文件
               Set Wb = Workbooks.Open(MP & "\" & MN)
                   a = a + 1
                   ' 访问一个工作簿
                   With Workbooks(1).ActiveSheet
                   ' 遍历所有的表
                   For i = 1 To Sheets.Count
                   ' 如果第一个单元格不为空（A1不为空）
                   If Sheets(i).Range("a1") <> "" Then
                       ' Resize 从第一格到最后一格复制
                       ' 下面这一行设置只读一次，a1表示第一行
                       Wb.Sheets(i).Range("a1").Resize(1, Sheets(i).UsedRange.Columns.Count).Copy .Cells(1, 1)
                       
                       d = Wb.Sheets(i).UsedRange.Columns.Count
                       c = Wb.Sheets(i).UsedRange.Rows.Count - 1
                       ' wn就是表名 MN是文件名 Wb是路径加文件名
                       wn = Wb.Sheets(i).Name
                       .Cells(1, d + 1) = "表名"
                       .Cells(e + 1, d + 1).Resize(c, 1) = MN & wn
                       ' 上面这部分可省略
                       ' e初始为1
                       e = e + c
                       Wb.Sheets(i).Range("a2").Resize(c, d).Copy .Cells(.Range("a1048576").End(xlUp).Row + 1, 1)
           End If
       Next
       Wbn = Wbn & Chr(13) & Wb.Name
       Wb.Close False
   End With
   End If
   MN = Dir
   Loop
   ' Wihle循环结束
   Range("a1").Select
   ' 提示部分
   Application.ScreenUpdating = True
   MsgBox "共合并了" & a & "个工作薄下全部工作表。如下：" & Chr(13) & Wbn, vbInformation, "提示"
   End Sub
   ```

   平时就需要改这两处地方，上面部分表示只读取一次（比如每个表格都有的标题信息，就不用重复读取了）

   ![image-20200726115411054](.\POI和easyExcel.assets\image-20200726115411054.png)

   对于成绩表这样的多行标题的页面可以这样设置

   ![image-20200726115613177](.\POI和easyExcel.assets\image-20200726115613177.png)

   ![image-20200726115626562](.\POI和easyExcel.assets\image-20200726115626562.png)







## 8、合并成绩表

   - 通过修改` a1 `和 `Cells(1, 1) `可以设置某一行只读取一遍

     ```vb
     Wb.Sheets(i).Range("a1").Resize(1, Sheets(i).UsedRange.Columns.Count).Copy .Cells(1, 1)
     ```

- 通过修改`Sheets.Count`的值可以指定读取哪几张表

  ```vb
  For i = 1 To Sheets.Count
  ```

   ```vb
Sub 合并目录所有工作簿全部工作表()
    Dim MP, MN, AW, Wbn, wn
    Dim Wb As Workbook
    Dim i, a, b, d, c, e
    Application.ScreenUpdating = False
    MP = ActiveWorkbook.Path
    MN = Dir(MP & "\" & "*.xlsx")
    AW = ActiveWorkbook.Name
    Num = 0
    e = 1
    Do While MN <> ""
        If MN <> AW Then
            Set Wb = Workbooks.Open(MP & "\" & MN)
                a = a + 1
                With Workbooks(1).ActiveSheet
                For i = 1 To Sheets.Count
                If Sheets(i).Range("a1") <> "" Then
                    Wb.Sheets(i).Range("a1").Resize(1, Sheets(i).UsedRange.Columns.Count).Copy .Cells(1, 1)
                    Wb.Sheets(i).Range("a2").Resize(1, Sheets(i).UsedRange.Columns.Count).Copy .Cells(2, 1)
                    Wb.Sheets(i).Range("a3").Resize(1, Sheets(i).UsedRange.Columns.Count).Copy .Cells(3, 1)
                    Wb.Sheets(i).Range("a4").Resize(1, Sheets(i).UsedRange.Columns.Count).Copy .Cells(4, 1)
                    d = Wb.Sheets(i).UsedRange.Columns.Count
                    c = Wb.Sheets(i).UsedRange.Rows.Count - 1
                    e = e + c
                    Wb.Sheets(i).Range("a5").Resize(c, d).Copy .Cells(.Range("a1048576").End(xlUp).Row + 1, 1)
        End If
    Next
    Wbn = Wbn & Chr(13) & Wb.Name
    Wb.Close False
End With
End If
MN = Dir
Loop
Range("a1").Select
Application.ScreenUpdating = True
MsgBox "共合并了" & a & "个工作薄下全部工作表。如下：" & Chr(13) & Wbn, vbInformation, "提示"
End Sub
   ```

   

6. 然后直接返回Excel表格中，选择宏

   ![image-20200531143729862](.\POI和easyExcel.assets\image-20200531143729862.png)

7. 单击执行

   ![image-20200531143758067](.\POI和easyExcel.assets\image-20200531143758067.png)















## 9、可能遇到的异常

![image-20200530225635909](.\POI和easyExcel.assets\image-20200530225635909.png)



解决方法：将jdk版本改为java8即可

![image-20200530225805284](.\POI和easyExcel.assets\image-20200530225805284.png)