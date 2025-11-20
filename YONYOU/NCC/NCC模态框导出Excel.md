# 模态框导出EXCEL

## 前端

```js
modalContext = () =>{
    return(
        <div>
            <div className="nc-bill-top-area he-dui">
                {this.props.cardTable.createCardTable('VerifyTable',{
                    onAfterEvent:this.onAfterEditFormlist,
                    lazyload:false,
                    hideSwitch:()=>{return false;},
                    showIndex: true,
                    showMaxBtn:true,
                    height:600,
                    onRowDoubleClick : this.checkRowClick,
                    // Added by Jiahui Li
                    tableHead: () => {
                        return (<NCButton onClick={ this.handleDtExport.bind(this)}>导出</NCButton>)
                    }
                })}
            </div>
		</div>
	)
}

handleDtExport(){
    // 构建tsMap:{主键: ts}，适配列表操作列优先从record中取值
    let tsMap = {};
    // 获取选中行
    let selectedRows = this.getCheckedDatas() || [];// 选中行
    selectedRows.forEach(row => Object.assign(tsMap, { [row.values['batch_head'].value]: row.values['ts'].value }));
    // tsMap = {selectedRows[0].values[FIELDS.PRIMARYKEY].value : selectedRows[0].values['ts'].value}
    formDownload({
        params: tsMap,
        url: URLS.exportexcelUrl,
        enctype: 2
    });
}
```

### 后端

#### ExportExcelAction.java

```java
public class ExportExcelZiDownBatchVOAction extends AbstractFormDecodeAction {
    @Override
    public Object excute(String param, IRequest request) {
        Map<String, String> tsMap = JSONObject.parseObject(param, Map.class);
        if (tsMap == null || tsMap.size() == 0) {
            throw new BusinessException("通过getTsMap()获取pk与ts组成的Map集合作为参数时未获取到，请传递tsMap");
        }
        List<String> listPk = new ArrayList<String>();
        tsMap.forEach((key, value) -> {
            listPk.add(key);
        });
        if (listPk == null || listPk.size() <= 0) {
            throw new BusinessException("请选择一条数据！");
        }

        // 获取参数String pk = listPk.get(0);
        if (StringUtil.isEmpty(pk)) {
            throw new BusinessException("主批次号不能为空！");
        }
        ICollDownBatchManagerService service = ServiceLocator.find(ICollDownBatchManagerService.class);
        List<ZifiCheckTopVO> list = new ArrayList<ZifiCheckTopVO>();
        try {
            list = service.check(new String[] { pk });
        } catch (nc.vo.pub.BusinessException e1) {
            // TODO Auto-generated catch block
            e1.printStackTrace();
        }

        String filename = "日结单";
        File onetempfile = null;
        WebFile webfile = null;
        try {
            onetempfile = File.createTempFile(filename, ".tmp");
            UFOEFileUtil.setFileAllPerm(onetempfile);
            this.exportExcel(filename, list, onetempfile);
            InputStream is = new FileInputStream(onetempfile);
            webfile = new WebFile(filename + ".xlsx", is);
        } catch (Exception e) {
            ExceptionUtils.wrapException(e);
        } finally {
            if (onetempfile != null) {
                onetempfile.deleteOnExit();
            }
        }
        return webfile;
    }

    public void exportExcel(String title, List<ZifiCheckTopVO> datas, File file) throws Exception {
        if (datas == null) {
            return;
        }
        UFOEFileUtil.setFileAllPerm(file);
        XSSFWorkbook workbook = new XSSFWorkbook();
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        try {
            // 生成一个表格
            XSSFSheet sheet = workbook.createSheet(title);
            // 第一行样式
            XSSFCellStyle headerStyle = workbook.createCellStyle();
            // 设置这些样式
            XSSFColor headerColor = new XSSFColor();
            headerColor.setRGB(intToByteArray(getIntFromColor(221, 221, 221)));

            headerStyle.setFillForegroundColor(headerColor);
            headerStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND);
            headerStyle.setBorderBottom(BorderStyle.THIN);
            headerStyle.setBorderLeft(BorderStyle.THIN);
            headerStyle.setBorderRight(BorderStyle.THIN);
            headerStyle.setBorderTop(BorderStyle.THIN);
            headerStyle.setAlignment(HorizontalAlignment.CENTER);
            headerStyle.setVerticalAlignment(VerticalAlignment.CENTER);
            // 生成一个字体
            XSSFFont headerFont = workbook.createFont();
            headerFont.setFontHeightInPoints((short) 10);
            headerFont.setBold(true);
            // 把字体应用到当前的样式
            headerStyle.setFont(headerFont);

            // 普通单元格样式
            XSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setFillForegroundColor(IndexedColors.WHITE.index);
            cellStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND);
            cellStyle.setBorderBottom(BorderStyle.THIN);
            cellStyle.setBorderLeft(BorderStyle.THIN);
            cellStyle.setBorderRight(BorderStyle.THIN);
            cellStyle.setBorderTop(BorderStyle.THIN);
            cellStyle.setAlignment(HorizontalAlignment.LEFT);
            cellStyle.setVerticalAlignment(VerticalAlignment.CENTER);
            // 生成另一个字体
            XSSFFont cellFont = workbook.createFont();
            cellFont.setFontHeightInPoints((short) 10);
            // 把字体应用到当前的样式
            cellStyle.setFont(cellFont);

            // 表体黄色合计样式
            XSSFColor firstColor = new XSSFColor();
            firstColor.setRGB(intToByteArray(getIntFromColor(255, 253, 191)));
            XSSFCellStyle firstStyle = workbook.createCellStyle();
            firstStyle.setFillForegroundColor(firstColor);
            firstStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND);
            firstStyle.setBorderBottom(BorderStyle.THIN);
            firstStyle.setBorderLeft(BorderStyle.THIN);
            firstStyle.setBorderRight(BorderStyle.THIN);
            firstStyle.setBorderTop(BorderStyle.THIN);
            firstStyle.setAlignment(HorizontalAlignment.LEFT);
            firstStyle.setVerticalAlignment(VerticalAlignment.CENTER);
            XSSFFont firstFont = workbook.createFont();
            firstFont.setFontHeightInPoints((short) 10);
            firstFont.setBold(true);
            firstStyle.setFont(firstFont);

            // 遍历集合数据，产生数据行
            XSSFRow row = null;
            // 默认宽度
            int maxLen = 20;
            List<String> keys = Arrays.asList("batch_head", "prcty", "data_src", "rep_des", "take_des", "count",
                                              "currency", "moneynum");// 列属性// 创建表头
            List<String> headNamelist = Arrays.asList("序号", "下载批次", "利润中心", "方式", "汇总项目", "收付方式", "单证张数", "币别", "金额");
            row = sheet.createRow(0);
            for (int i = 0; i < headNamelist.size(); i++) {
                XSSFCell cell = row.createCell(i);
                cell.setCellStyle(headerStyle);
                cell.setCellValue(headNamelist.get(i));
            }
            // 根据数据创建表体
            for (int i = 0; i < datas.size(); i++) {
                ZifiCheckTopVO data = datas.get(i);
                Map<String, String> describe = BeanUtils.describe(data);
                if (data == null) {
                    continue;
                }
                row = sheet.createRow(i + 1);
                Object value = i + 1;
                String valStr = value == null ? "" : value.toString();
                XSSFCell cell0 = row.createCell(0);
                // 不同填充色
                if (data.getData_src().contains("合计")) {
                    cell0.setCellStyle(firstStyle);
                } else {
                    cell0.setCellStyle(cellStyle);
                }
                cell0.setCellValue(valStr);
                for (int j = 0; j < keys.size(); j++) {
                    value = describe.get(keys.get(j));
                    valStr = value == null ? "" : value.toString();
                    XSSFCell cell = row.createCell(j + 1);
                    // 不同填充色
                    if (data.getData_src().contains("合计")) {
                        cell.setCellStyle(firstStyle);
                    } else {
                        cell.setCellStyle(cellStyle);
                    }
                    cell.setCellValue(valStr);
                }
            }
            sheet.setDefaultColumnWidth(maxLen);
            byte[] bytes = null;
            workbook.write(outputStream);
            outputStream.flush();
            bytes = outputStream.toByteArray();
            try (FileOutputStream stream = new FileOutputStream(file)) {
                stream.write(bytes);
                stream.flush();
            }
        } catch (Exception e) {
            throw new Exception(e);
        } finally {
            if (workbook != null) {
                workbook.close();
            }
            if (outputStream != null) {
                outputStream.close();
            }
        }
        return;
    }
   /**
  	* rgb转int
  	*/
    private static int getIntFromColor(int Red, int Green, int Blue) {
        Red = (Red << 16) & 0x00FF0000;
        Green = (Green << 8) & 0x0000FF00;
        Blue = Blue & 0x000000FF;
        return 0xFF000000 | Red | Green | Blue;
    }

   /**
  	* int转byte[]
  	*/
    public static byte[] intToByteArray(int i) {
        byte[] result = new byte[4];
        result[0] = (byte) ((i >> 24) & 0xFF);
        result[1] = (byte) ((i >> 16) & 0xFF);
        result[2] = (byte) ((i >> 8) & 0xFF);
        result[3] = (byte) (i & 0xFF);
        return result;
    }
}
```

## 