# NCC消息通知

```java
//消息发送
private void sendMessage(String subject, String content, String userID) {
    String nc_sender ="NC_USER0000000000000";
    try {
        NCCMessageVO nccMessageVO = new NCCNoticeMessageVO();
        //构建消息VO
        nccMessageVO.setSubject(subject);//主题
        nccMessageVO.setContent(content);//消息内容
        if(content!=null && content.length()>4000) {
            nccMessageVO.setContent(content.substring(0,4000));
        }
        nccMessageVO.setMsgsourcetype("notice");//消息类型
        nccMessageVO.setSendtime(new UFDateTime(new Date()));
        nccMessageVO.setSender("NC_USER0000000000000");

        nccMessageVO.setReceiver(userID);//接受者
        nccMessageVO.setPk_group("0001DA10000000000GZJ");//集团

        NCCMessage nccMessage = new NCCMessage();
        nccMessage.setMessage(nccMessageVO);//消息内容
        nccMessage.setMessageType("notice");//消息类型
        String msg = MessageCenter.sendMessage(nccMessage, new NullMessageSendMonitor());
    }
    catch (Exception e) {
        Logger.error("向消息中心发送消息时发生错误" + e.getMessage(), e);
    }
}
```

