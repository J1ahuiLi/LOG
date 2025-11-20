# 导入导出

## 导入字段修改

1. 导入导出xml文件中相关字段下添加翻译器ID

```xml
<translator>1001ZZ1LCP000005XP1Y</translator>
```

2. 数据库添加相关字段

```sql
SELECT PK_TRANSLATOR FROM EXCEL_TRANSLATOR WHERE MODULE ='taxm' ORDER BY ts DESC ;
```

## 修改导入导出自定义翻译类

```java
public class AggPit_tax_ledger_hTranslator extends AbstractRefTranslator {

    private Logger logger = LoggerFactory.getLogger(AggTaxRateConfigVOTranslator.class);

    @SuppressWarnings("unchecked")
    @Override
    public String translateExToNC(String srcValue, String metaDataID, ITranslateContext translateContext)
        throws PfxxException {
        //TODO
        // if (translateContext.getProperty().getLabel().equals("县支公司")) {
        // 	 String countryBranchSql = "SELECT cccode, pk_costcenter FROM resa_costcenter";
        //   String val = "";
        //   try {
        //     Map<String, String> countryBranchMap = (Map<String, String>) new BaseDAO()
        //       .executeQuery(countryBranchSql, new KeyValueMapProcessor<String, String>());
        //     val = countryBranchMap.get(srcValue);
        //   } catch (Exception e) {
        //     logger.error(e.getMessage());
        //     return null;
        //   }
        //     return val;
        // }
        return null;
    }
}
```

## 导出字段修改

```java
@SuppressWarnings("unchecked")
@Override
protected ExportDataInfo getValue(Object[] vos, List<InputItem> exportItems, BillDefination billdefination) throws BusinessException {
    ExtendedAggregatedValueObject[] aggvos = getConvertorForTemp(new DefRefPropertyProcess()).convertDataFromEditorData(billdefination, vos, exportItems);
    //TODO
    // String strMapSql = "SELECT riskcode, RISKCODE_YW FROM "
    //   + "( SELECT RISKCODE_YW, RISKCODE_NAME, RISKCODE, PK_ZTRANS_RCCONVERT, row_number() over(PARTITION BY RISKCODE_YW,RISKCODE_NAME,RISKCODE ORDER BY RISKCODE_YW) AS rk FROM "
    //   + "( SELECT trim(RISKCODE_YW) RISKCODE_YW, trim(RISKCODE_NAME) RISKCODE_NAME, trim(RISKCODE) RISKCODE, trim(PK_ZTRANS_RCCONVERT) PK_ZTRANS_RCCONVERT FROM ztrans_rcconvert UNION SELECT trim(RISKCODE_YW) RISKCODE_YW, '_', trim(RISKCODE) RISKCODE, PK_ZCIS_RCCONVERT  FROM zcis_rcconvert ) ) WHERE rk =1";
    // Map<String, String> ywCodeMap = (Map<String, String>) new BaseDAO().executeQuery(strMapSql.toString(), new KeyValueMapProcessor<String, String>());
    // try {
    //     for (ExtendedAggregatedValueObject aggvo : aggvos) {
    //         aggvo.getParentVO().setAttributeValue("pk_taxcode", ywCodeMap.get(aggvo.getParentVO().getAttributeValue("pk_taxcode")));
    //     }
    // } catch (Exception e) {
    //     Logger.error(e);
    // }

    return new ExportDataInfo(aggvos);
}
```

## 导入字段赋值

```java
@Override
public AggTaxRateConfigVO[] saveAggTaxRateConfigVO(AggTaxRateConfigVO vo) throws BusinessException {
    //TODO
    // vo.getParentVO().setPk_type1name(type1Name.get(vo.getParentVO().getPk_type1()));
    if (StringUtils.isEmpty(pk)) {
        return dao.insert(vo);// 插入
    } else {
        return dao.update(vo);// 更新
    }
}
```

