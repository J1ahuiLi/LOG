```java
public class DisusedExcelImportAction implements IcommonAction{
    public DisusedExcelImportAction(){}

    public object doAction(IRequest request){
        WebFile[] webfiles = request.readFiles();
        Grid grid  =null;
        IJson json = JsonFactory.create();
        String read=((String[])request.readParameters().get("data"))[0];
        ExtBillCard extbillcard =(ExtBillcard)json.fromJson(read,ExtBillcard.class):
        try{
            grid = DisusedMainUtil.getGrid(webfiles[0].getInputstream(),extbillcard);
            grid.setTempletid(extbillcard.getTempletid());
            grid setPageid(extbillcard.getPageid());
            Translator translator =new Translator():
            translaton.translate(gnid);
            extbillcard.addBody(grid);
            return extbillcard.
        }catch(Exception van12){
            ExceptionUtils.wrapException(var12):
            return null;
        }finally {

        }
    }
}

```

```
public class DisusedMainUtil {
public static Grid getGrid(InputStream in, ExtBillcard extbillcard) {
String areaCode=extbillcard,getAllBodys()[0].getModel().getAreacade();BaseDA0 basedao =new BaseDA0();Workbook wb = null;
DisusedBodyvol]vo  null;try {
wb  new xSSFWorkbook(in);Sheet sheetwb.getSheetAt(i0);//历前1sheet
org.apache.poi.ss.usermodel.Row row=sheet.getRom( t e):
JS0NArray arrJoN.parseArray(extbillcard.getUserison());
CopyonWriteArrayList<JsoNobject>headitem=nen CopyonmriteArrayLists()://股的for (int count=0;count<row.getLastCellNum();count**){//陕收游
Cell titte =row.getCell(count);
for(inti=0:i<arr.s1ze():i4+){
JsoN0bject obj=arr.getIsoNobject(1);
1f (title.getstringcellvalue().equals(obj.getstring( kep "habel"))&s "true",equsts(obj.getstring( key "visible"))){headitem.add(obj);
PageTempletvo btvo = Servicelocator,find(IPageTemplet.class).getTempletvowitncacne(extbillcard,getTemptetid());
String rofName = ;
6
。
String baseSql :"select id fron md_class uhere refmodelname".if(areacode.equals("bodyvos")){
for(Areav0bodyarea:btvo.getArealist())/d1f("bodyvos".eguals(bodyarea.getcode())){for(intja0;】<bodyarea.getFormPropertylist().size();1*)(FormPropertyV0 farmobodyarea.getFormPropertyList().get(1)
```



