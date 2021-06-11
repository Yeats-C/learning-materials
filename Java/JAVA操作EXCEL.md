# JAVA操作EXCEL

## 准备
引入包
```
<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi</artifactId>
			<version>4.0.0</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml</artifactId>
			<version>4.0.0</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-scratchpad</artifactId>
			<version>4.0.0</version>
		</dependency>
```

## 测试 
```
public static void main(String[] args) throws FileNotFoundException {
        POIFSFileSystem fs;

        try {
            fs = new POIFSFileSystem(new FileInputStream("test.xls"));

            HSSFWorkbook wb = new HSSFWorkbook(fs);

            HSSFSheet sheet = wb.getSheet("输入数据");

            HSSFRow row = sheet.getRow((short) 1);

            HSSFCell cell = row.getCell((short) 2);

            cell.setCellValue((short) 5);

            cell = row.getCell((short) 3);

            cell.setCellValue((short) 40);

            HSSFCell cell1 = row.getCell((short)1);

//            if (2==cell1.getCellType().getCode()) {
//            //取得公式单元格的公式,重新设置
//
//                cell1.setCellFormula(cell1.getCellFormula());
//            }



            // 刷新公式
            wb.setForceFormulaRecalculation(true);

            //使用evaluateFormulaCell对函数单元格进行强行更新计算
            wb.getCreationHelper().createFormulaEvaluator().evaluateAll();


            FileOutputStream fileOut = new FileOutputStream("test.xls");

            wb.write(fileOut);

            fileOut.close();

        } catch (IOException e) {
            e.printStackTrace();

        }

    }
```

## 知识点
> 单个单元格刷新计算公式
```
cell1.setCellFormula(cell1.getCellFormula()); 
```
> 整个Excel刷新公式
```
// 刷新公式
wb.setForceFormulaRecalculation(true);

//使用evaluateFormulaCell对函数单元格进行强行更新计算
wb.getCreationHelper().createFormulaEvaluator().evaluateAll();
```
