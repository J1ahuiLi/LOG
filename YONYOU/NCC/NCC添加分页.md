# NCC添加分页

## 前端

创建state模型下，相关模块区域下添加

```java
handleModal: {
    showPagination: true,// 显示分页器// 页器操作的回调函数handlePageInfoChange: (props, config, pks, total) => {
    	// 加载表格数据  -> 将数据设置到表格上  -> 更新按钮状态this.onHandle(null, 'handleModal');
    	this.setPageStatus(EDITMODE_EDIT, this.updateBtnStatus);
	}
},

onHandle = (param = {}, modalType) => {
    let pageInfo = this.props.editTable.getTablePageInfo(this.state.handleModal.area);// 页面分页信息ajax({
    	url: '',
    	data: { pageInfo, queryTreeFormatVO: { ...queryInfo, pageCode: this.config.pagecode } },
    	success: (res) => {
        	if (res.data != null) {
            	this.props.editTable.setTableData(handleModal.area, res.data.data['cit_daily_credTable']);
        	}
    	}
	})
}
```

## 后端

```java
// json数据转换
public Object doAction(IRequest request, RequstParamWapper paramWapper) throws Throwable {
    RequestDTO param = VOTransform.toVO(paramWapper.requestString, RequestDTO.class);
    QueryTreeFormatVO queryVo = param.getQueryTreeFormatVO();
    ITService service = ServiceLocator.find(ITService.class);
    List<T> result = service.handleModal(queryVo);
    List<String> allpksList = new ArrayList<String>();
    for (T resultItem : result) {
        allpksList.add(resultItem.getDef2());
    }
    if (allpksList == null || allpksList.size() < 1) {
        return null;
    }
    // 获取结果pk
    String[] allpks = (String[]) allpksList.toArray(new String[allpksList.size()]);
    // 传入全部数据
    param.setAllpks(allpks);
    // 获取页面信息PageInfo pageInfo = param.getPageInfo();
    // 根据分页拿到当前分页的pk
    String[] curPagePks = pageInfo == null ? allpks : paramWapper.pageResult(pageInfo, allpks);
    // 查询当前分页数据
    List<T> finalResult = result.stream().filter(m -> Arrays.stream(curPagePks).anyMatch(def2 -> def2 == m.getDef2())).collect(Collectors.toList());
    // 构造返回结果return buildResult(param, false, null, finalResult.toArray());
}

```

## 数据库

```sql
SELECT * FROM pub_area WHERE code = '';
UPDATE pub_area SET pagination = 'true' WHERE code = '';
```

