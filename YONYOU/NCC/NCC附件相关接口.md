# 附件相关接口

## 附件上传

```java
/**
 * 附件上传
 * @author xl
 *
 */
public class UpFileServlet extends HttpServlet{
    private static final long serialVersionUID = 2752936832927872870L;
    private static final String UTF8 = "text/json;charset=utf-8";


    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        JSONObject response = new JSONObject();
        JSONObject Response = new JSONObject();
        req.setCharacterEncoding("UTF-8");
        resp.setHeader("Content-Type", UTF8);
        PrintWriter out = resp.getWriter();
        ISecurityTokenCallback sc = NCLocator.getInstance().lookup(ISecurityTokenCallback.class);
        sc.token("NCSystem".getBytes(), "pfxx".getBytes());


        JSONObject rebillObject = CommonApiUtil.getJsonData(req);

        String  creator = rebillObject.getString("user");
        String  billid = rebillObject.getString("billid");
        String filename = rebillObject.getString("filename");
        String filetype = rebillObject.getString("filetype");
        String file = rebillObject.getString("file");




        JSONArray psnArray = new JSONArray();

        //附件的创建人
        String  user = null;
        try {
            //判断用户是否存在，不存在返回错误信息


            //判断单据是否存在
            String isentBill =  (String)NCLocator.getInstance().lookup(IUAPQueryBS.class).executeQuery("select pk_entryapply from  hi_entryapply where pk_entryapply='"+billid+"' ", new ColumnProcessor());
            if(null==isentBill || isentBill.isEmpty()){
                String isstaBill =  (String)NCLocator.getInstance().lookup(IUAPQueryBS.class).executeQuery("select pk_hi_stapply from  hi_stapply where pk_hi_stapply='"+billid+"' ", new ColumnProcessor());
                if(null == isstaBill || isstaBill.isEmpty()){
                    throw new BusinessException("单据不存在，无法上传附件！");
                }

            }


            byte[] destBuffer = new sun.misc.BASE64Decoder().decodeBuffer(file);
            InputStream stream = new ByteArrayInputStream(destBuffer);
            String fileName = filename+filetype;
            //调用附件上传服务
            FileStorageClient fsclient = FileStorageClient.getInstance();
            FileHeader header = fsclient.uploadFile( IAttachManageConst.RIAMoudleID, fileName, stream, false, new IFileStorageExt[0]);
            if (header != null) {
                String pk_doc = header.getPath();


                String name = header.getName();
                long size1 = header.getFileSize().longValue();
                NCFileVO attach = new NCFileVO();
                attach.setPath(name);
                attach.setCreator(user);
                attach.setIsdoc("z");
                attach.setFileLen(size1);
                attach.setPk_doc(pk_doc);
                IFileSystemService fsysService = NCLocator.getInstance().lookup(IFileSystemService.class);
                fsysService.createCloudFileNode(billid, user, attach);
                response.put("filepk", pk_doc);
            }


        } catch (Exception e) {
            // TODO Auto-generated catch block
            //			e.printStackTrace();

        }
        out.print(Response);
        out.flush();
        out.close();

    }

    /***
	 * 上传文件
	 * 
	 * @param fileName
	 * @param billpk
	 * @param input
	 * @throws BusinessException
	 */
    private void upLoad(String fileName, String billpk, InputStream input) throws BusinessException {
        //		Random random = new Random(6);
        //		String r = String.valueOf(random.nextInt()).replace("-", "");
        //		fileName = fileName.replace(".", "_".concat(r).concat("."));
        FileStorageClient fsclient = FileStorageClient.getInstance();
        FileHeader header = fsclient.uploadFile(IAttachManageConst.RIAMoudleID, fileName, input, false, new IFileStorageExt[0]);
        if (header != null) {
            String pk_doc = header.getPath();
            String name = header.getName();
            long size1 = header.getFileSize().longValue();
            NCFileVO attach = new NCFileVO();
            attach.setPath(name);
            attach.setCreator("");
            attach.setIsdoc("z");
            attach.setFileLen(size1);
            attach.setPk_doc(pk_doc);
            getService().createCloudFileNode(billpk + "/" + fileName, "", attach);
        }
    }

    private IFileSystemService service = null;
    private IFileSystemService getService() {
        if (service == null) {
            service = NCLocator.getInstance().lookup(IFileSystemService.class);
        }
        return service;
    }

}
```

## 附件查询下载

```java
/**
 * 附件查询 
 * @author xuelin
 *
 */
public class FileQueryServlet extends HttpServlet{
    private static final long serialVersionUID = 2752936832927872870L;
    private static final String UTF8 = "text/json;charset=utf-8";


    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        JSONObject response = new JSONObject();
        JSONObject Response = new JSONObject();
        req.setCharacterEncoding("UTF-8");
        resp.setHeader("Content-Type", UTF8);
        PrintWriter out = resp.getWriter();
        ISecurityTokenCallback sc = NCLocator.getInstance().lookup(ISecurityTokenCallback.class);
        sc.token("NCSystem".getBytes(), "pfxx".getBytes());

        IUAPQueryBS bs = NCLocator.getInstance().lookup(IUAPQueryBS.class);

        JSONObject rebillObject = CommonApiUtil.getJsonData(req);

        String  pk = rebillObject.getString("billid");        //单据主键
        String name = rebillObject.getString("billtype"); //单据类型
        String creator = rebillObject.getString("user");//附件创建人

        try {

            if(null==pk || null==name || null==creator){

                throw new BusinessException("附件信息为空，请确认！");
            }

            StringBuffer fileSql =new StringBuffer();
            fileSql.append(" select substr(bap_fs_header.name,0,instr(bap_fs_header.name,'.')-1) as name, substr(bap_fs_header.name,instr(bap_fs_header.name,'.')+1) as type , ");
            fileSql.append(" sm_pub_filesystem.pk_doc, sm_user.user_name,bap_fs_header.filesize ");
            fileSql.append(" from  sm_pub_filesystem left join bap_fs_header on sm_pub_filesystem.pk_doc=bap_fs_header.path ");
            fileSql.append(" left join sm_user on bap_fs_header.creator=sm_user.cuserid");
            fileSql.append(" where sm_pub_filesystem.filepath like '%"+pk+"%' and sm_pub_filesystem.pk_doc is not null");
            //			if(null !=creator){
            if(!creator.isEmpty()){
                //				fileSql.append(" and bap_fs_header.creator='"+creator+"' ");
                fileSql.append(" and bap_fs_header.creator=(select  cuserid from sm_user where user_code='"+creator+"') ");

            }

            List<Map<String,String>> file = (List<Map<String,String>>)bs.executeQuery(fileSql.toString(), new MapListProcessor());

            JSONArray fileArray = new JSONArray();
            if(null!=file && file.size()>0){
                // 返回信息
                //				（1）附件名称
                //				（2）附件类型
                //				（3）附件主键
                //				（4）单据主键
                //				（5）文件的所有者
                //				（6）文件大小（精确字节数，企微预览下载使用）
                for(Map<String,String> map : file){

                    JSONObject fileObj = new JSONObject();
                    fileObj.put("billid", pk);
                    fileObj.put("filename", map.get("name"));
                    fileObj.put("filetype", map.get("type"));
                    fileObj.put("pk_doc", map.get("pk_doc"));
                    //					fileObj.put("user_name", map.get("user_name"));
                    fileObj.put("filesize", map.get("filesize"));
                    fileObj.put("creator", creator);

                    //附件GET下载的地址
                    String fileUrl = null;
                    try{

                        FileStorageClient fileClient =FileStorageClient.getInstance();
                        //						fileUrl = fileClient.getDownloadURL("uapattachroot", map.get("pk_doc"), "http://10.32.224.112:8086/");
                        fileUrl = fileClient.getDownloadURL("uapattachroot", map.get("pk_doc"));
                    }catch(Exception e ){
                        throw new BusinessException(e.getMessage());
                        //						e.printStackTrace();

                    }
                    fileObj.put("fileUrl", fileUrl);
                    fileArray.add(fileObj);
                }


            }

        } catch (Exception e) {
            // TODO Auto-generated catch block
            //			e.printStackTrace();

        }
        out.print(Response);
        out.flush();
        out.close();

    }


}
```

