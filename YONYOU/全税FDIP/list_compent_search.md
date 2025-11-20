



```js
import React, { Component } from 'react';
import { actions } from 'mirrorx';
import { Col, Row, FormControl, Label, Checkbox, Switch } from 'tinper-bee';
import Select from 'bee-select';
import SearchPanel from 'components/SearchPanel';
import { addSearchKey, deepClone, getValidateFieldsTrim } from 'utils';
import FormList from 'components/FormList';
import FormError from 'components/FormError';
import SelectMonth from 'components/SelectMonth';
import InputNumber from 'bee-input-number';
import moment from 'moment';
import RefCommon from 'components/RefCommon';
import DatePicker from 'bee-datepicker';
import { handleEntity } from 'utils/tools';
import PreCode from 'components/PreCode';
import Radio from 'bee-radio';

import './index.less';

const FormItem = FormList.Item;
const { Option } = Select;
const { RangePicker } = DatePicker; // 日期范围选择组件

class SearchArea extends Component {
    constructor(props) {
        super(props);
        this.state = {
            rowData: {},
        };
    }

    /** 
     * 查询数据
     * @param {*} error 校验是否成功
     * @param {Object} values 表单数据
     */
    search = () => {
        this.props.form.validateFields((err, _values) => {
            let values = getValidateFieldsTrim(_values);
            // 获取默认请求的 分页信息
            if (!err) {
                // 日期按范围选择 START
                if (values.busi_date.length > 0) {
                    let busi_dateStart = moment(values.busi_date[0]).format('YYYY-MM-DD');
                    let busi_dateEnd = moment(values.busi_date[1]).format('YYYY-MM-DD');
                    values.busi_dateStart = busi_dateStart;
                    values.busi_dateEnd = busi_dateEnd;
                    delete values.busi_date;
                } else {
                    delete values.busi_dateStart;
                    delete values.busi_dateEnd;
                    delete values.busi_date;
                }
                // 日期按范围选择 END

                let { pageSize } = this.props.adj_billObj;
                let queryParam = deepClone(this.props.queryParam);
                let { pageParams } = queryParam;
                pageParams.pageIndex = 0;
                pageParams.pageSize = pageSize;

                const arrayNew = this.getSearchPanel(values); //对搜索条件拼接
                queryParam.whereParams = arrayNew;

                actions.masterDetailOneadj_bill.updateState({ cacheFilter: arrayNew }); // 保存查询条件
                actions.masterDetailOneadj_bill.loadList(queryParam);
                this.props.clearRowFilter();
            }
        });
    };

    /**
     *
     * @param values search 表单值
     * @returns {Array}
     */
    getSearchPanel = values => {
        const list = [];

        let queryconfition = {
            pk_org: 'IN',
            pk_year: 'EQ',
            tax_period: 'EQ',
            pk_item: 'EQ',
            // 日期按范围选择 START
            busi_date: "EQ",
            busi_dateStart: "GTEQ",
            busi_dateEnd: "LTEQ",
            // 日期按范围选择 END
        };

        // 日期按范围选择 START
        if (values.busi_date) {
            values.busi_date = moment(values.busi_date).format("YYYY-MM-DD");
        } 
        // 处理记账日期起
        if (values.busi_dateStart) {
            values.busi_dateStart = moment(values.busi_dateStart).format("YYYY-MM-DD");
        }   
        // 处理记账日期止
        if (values.busi_dateEnd) {
            values.busi_dateEnd = moment(values.busi_dateEnd).format("YYYY-MM-DD");
        }
        // 日期按范围选择 END

        values = handleEntity(values);

        for (let key in values) {
            if (values[key]) {
                let condition = queryconfition[key];
                list.push({ key, value: values[key], condition });
            }
        }
        return list;
    };

    reset = () => {
        this.props.form.resetFields();
    };

    render() {
        const { form, onCallback, orgList } = this.props;
        const { getFieldProps } = form;
        const _this = this;
        let { rowData } = this.state;
        let paramSearch = (
            <div className="headSearch">
            <FormItem label={'机构'}>
            <RefCommon
rowData={typeof rowData !== 'undefined' && rowData}
btnFlag={typeof btnFlag !== 'undefined' && btnFlag}
disabled={typeof btnFlag !== 'undefined' && btnFlag == 2}
type={1}
multiple={true}
title={'机构'}
refPath={`${GROBAL_HTTP_CTX}//orgcommon-ref/`}
displayField={'{name}'}
param={{
       refCode: 'organizationtreeRef',
       // clientParam: { id: orgList.join(',') },
      }}
      {...getFieldProps('pk_org', {
          initialValue: JSON.stringify({
              refname:
              (typeof rowData !== 'undefined' && rowData['pk_orgref']) || '',
              refpk: (typeof rowData !== 'undefined' && rowData['pk_org']) || '',
          }),
      })}
/>
</FormItem>
</div>
);
return (
    <SearchPanel
    // className="small"
    className="search-form"
    form={form}
searchOpen={true} //默认收起
reset={this.reset}
head={paramSearch}
onCallback={onCallback}
search={this.search}
>
    <FormList size="sm">
        {/* <FormItem label={'业务编码'}>
                        <FormControl
                            disabled={typeof btnFlag !== 'undefined' && btnFlag == 2}
                            placeholder={'请输入业务编码'}
                            {...getFieldProps('busi_code', {
                                initialValue: '',
                            })}
                        />
                    </FormItem> */}
{/* <FormItem  label={"业务日期"} >
                        <DatePicker 
                            className='form-item'
                            placeholder={"请选择业务日期"}
                            disabled={typeof btnFlag !== 'undefined' && btnFlag == 2}
                            format={'YYYY-MM-DD'}
                            {
                                ...getFieldProps('busi_date', {
                                    initialValue: '',
                                })
                            }
                        />
                    </FormItem> */}
{/* <FormItem label={'业务日期'}>
                        <RangePicker
                            className="form-item"
                            disabled={typeof btnFlag !== 'undefined' && btnFlag == 2}
                            placeholder={'业务日期开始 ~ 业务日期结束'}
                            dateInputPlaceholder={['开始', '结束']}
                            showClear={true}
                            showClose={true}
                            {...getFieldProps('busi_date', {
                                initialValue: '',
                            })}
                        />
                    </FormItem> */}
{/* <FormItem label={'科目名称'}>
                        <RefCommon
                            rowData={typeof rowData !== 'undefined' && rowData}
                            btnFlag={typeof btnFlag !== 'undefined' && btnFlag}
                            disabled={typeof btnFlag !== 'undefined' && btnFlag == 2}
                            placeholder={'请选择科目'}
                            type={2}
                            title={'科目名称'}
                            refPath={`${GROBAL_HTTP_CTX}//common-ref/`}
                            displayField={'{code}'}
                            param={{
                                refCode: 'subjectRef',
                            }}
                            {...getFieldProps('pk_subject', {
                                initialValue: JSON.stringify({
                                    refname:
                                        (typeof rowData !== 'undefined' &&
                                            rowData['pk_subjectref']) ||
                                        '',
                                    refpk:
                                        (typeof rowData !== 'undefined' && rowData['pk_subject']) ||
                                        '',
                                }),
                            })}
                        />
                    </FormItem> */}
<FormItem label={'记账日期'}>
    <RangePicker
className="form-item"
disabled={typeof btnFlag !== 'undefined' && btnFlag == 2}
placeholder={'记账日期起 ~ 记账日期止'}
dateInputPlaceholder={['开始日期', '结束日期']}
showClear={true}
showClose={true}
{...getFieldProps('busi_date', {
    initialValue: '',
})}
/>
</FormItem>


</FormList>
</SearchPanel>
);
}
}

export default FormList.createForm()(SearchArea);

```

