### 1.组件使用

```vue
<tpTable
	:table-list="tableData"
    :header="header"
    :select="true"
    :sumData="sumData"
    :mergeColumns="mergeFields"
    @row-click="rowClick"
    @cell-click="cellClick"
    @selectionChange="selectionChange"
 />

//引用地址
import tpTable from "@/components/tp_table/index.vue"
```

#### 1.1组件参数

| 参数名称         | 参数说明               | 是否必填 | 数据类型 | 备注                                         |
| ---------------- | ---------------------- | -------- | -------- | -------------------------------------------- |
| table-list       | 表格数据               | 是       | Array    |                                              |
| header           | 表格字段对象           | 是       | Array    | 详见下面说明                                 |
| select           | 表格是否带有复选框     | 否       | Boolean  |                                              |
| sumData          | 合计对象               | 否       | Object   | 详见下面说明                                 |
| mergeFields      | 需要合并单元格字段对象 | 否       | Array    | 详见下面说明                                 |
| rowClick         | 表格行点击事件         | 否       | function | 三个参数，分别为行对象、列对象、事件         |
| cellClick        | 表格单元格点击事件     | 否       | function | 四个参数，分别为行对象、列对象、单元格、事件 |
| selectionChange  | 表格复选框勾选事件     | 否       | function | 一个参数，为勾选的行数据                     |
| objectSpanMethod | 表格行列合并方法       | 否       | function | 详见下面说明                                 |

#### 1.2表格字段参数

```json
// 示例
header:[
    { prop:"name",label:"名称" },
    {
        prop:"age",
        label:"年龄",
        formatter:(row)=>{
            return row.age+1;
        }
    },
    {
        label:"地址",
        children:[
            {
                label:"省份",
                children:[
                    label:"市区",
                    prop:"adress"
                ]
            }
        ]
    },
    {
        label:"操作",
        render:(h,data)=>{
            return (
                <el-button type="primary" onClick={()=>this.edit(data.row)}>编辑</el-button>
            )
        }
    }
]
```

| 参数名称  | 参数说明       | 是否必填 | 数据类型                                | 备注                                             |
| --------- | -------------- | -------- | --------------------------------------- | ------------------------------------------------ |
| prop      | 字段存储值     | 否       | String                                  |                                                  |
| label     | 字段显示值     | 是       | String                                  |                                                  |
| formatter | 用来格式化内容 | 否       | Function(row, column, cellValue, index) | 四个参数，分别为行对象、列对象、单元格数据及下标 |
| children  | 多级表头对象   | 否       | Array                                   | 可以多级嵌套，目前支持三级表头                   |
| render    | render构造器   | 否       | Function                                | 使用jsx语法实现自定义列                          |



#### 1.3字段合计对象

```vue
//示例
sumData:{
	type:"sum",
    sumField:['d']
};
// type:表尾类型，目前支持合计  sumField：需要计算的字段
```



#### 1.4字段合并对象

```vue
//示例
mergeFields:[
	{name:"w",index:1},
    {name:"x",index:2},
    {name:"d",index:3}
],
//数组对象数据格式，name：字段 index:当前列字段的下标(注意：若表格含有勾选框，此时下标应为包含勾选框后列下标)

/**
*传数组对象组件默认合并相同行与列，但会有某个单元格与相邻列单元格相同并且也和相邻行单元格相同情况，此时不能使用组件默认合并方法，否则会出现表格样式错乱情况
*组件提供合并方法，带有四个参数，分别是行对象、列对象、行下标、列下标，使用此方法时，mergeFields对象须为空，否则会走组件默认合并方法
*/
```

