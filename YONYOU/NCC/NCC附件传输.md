# 传输附件

## 本地文件保存和文件上传

```java
		//获取当前登录信息
		SessionContext usercontext = SessionContext.getInstance();
		String userid = usercontext.getClientInfo().getUserid();
		//根据用户userid(pk)信息查找对应用户信息
		UserVO user = userService.getUser(userid);

		if(user != null){
			//人员身份信息pk
			String pkPsndoc = user.getPk_psndoc();
			if(pkPsndoc != null){
				//取得人员全部信息，基本信息+任职表（主职信息）
				List<PsndocVO> psndocVOS = iPsndocPubService.queryPsndocAndMainJobByPks(new String[]{pkPsndoc});
```

