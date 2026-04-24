# Read a Exel file
    - Apache poi is a library is use to read a exel file need to integrate with the selenium 
    - We need to add a dependancy to run the projects

### There are 4 classes in a Apache poi
    1. XSSFWorkBook
    2. XSSFSheet
    3. XSSFRow
    4. XSSFCell

    `// Exel file -> WorkBook -> Sheet -> cell`     

#### File Reading in a Apache POI
```
FileInputStream fis = new FileInputStream("data.xlsx");
Workbook wb = WorkbookFactory.create(fis);
Sheet sheet = wb.getSheetAt(0);

for(int i = 0 ; i <= sheet.getLastRowNum(); i++){
    Row row = sheet.getRow();
    for(int j = 0; j < row.getLastCellNum(); j++){
        row.getCell(j).getStringCellValue()
    }
System.out.println();
}

```
#### File Writeing in Apache POI
```
        XSSFWorkbook workbook = new XSSFWorkbook();
        XSSFSheet sheet = workbook.createSheet("Results");

        XSSFRow row = sheet.createRow(0);
        row.createCell(0).setCellValue("TestCase");
        row.createCell(1).setCellValue("Status");

        FileOutputStream fos = new FileOutputStream("result.xlsx");
        workbook.write(fos);

        workbook.close();
        fos.close();
```