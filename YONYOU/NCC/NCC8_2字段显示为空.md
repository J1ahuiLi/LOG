```java
BaseDAO baseDAO = new BaseDAO();
		ResultWapper object = (ResultWapper) buildResult(param, true, null, vo);
		if (object != null) {
			ExtBillCard extBillCard = (ExtBillCard) object.getData();
			if (extBillCard != null) {
				Form form = extBillCard.getHead();
				GridModel formModel = form.getModel();
				if (formModel != null && StringUtils.isNotBlank(formModel.getAreacode())
						&& "bd_urancekind_form".equals(formModel.getAreacode())) {
					Row[] rows = formModel.getRows();
					if (rows != null && rows.length > 0) {
						Row row = rows[0];
						Cell pkUrancetype = row.getCell("pk_urancetype");
						if (pkUrancetype != null && pkUrancetype.getValue() != null) {
							String value = (String) pkUrancetype.getValue();
							if (StringUtils.isNotBlank(value)) {
								String sql = "SELECT URANCETYPENAME FROM ins_bd_urancetype WHERE PK_URANCETYPE = '" + value + "'";
								Object result = baseDAO.executeQuery(sql, new ColumnProcessor());
								if(result != null) {
									pkUrancetype.setDisplay((String) result);
								}
							}
						}
					}
				}
			}
		}
```

