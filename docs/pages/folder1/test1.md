### 1.组件使用

```vue
<page-tp
   :header-field="headerField"
   :table-data="tableData"
   :page="fyData"
   order-number
   check-box
   :buttonListRight="topButtonList"
   :buttonListRow="rowFixedButtonList"
   :signRule="signRule"
   :operationHide="operationHide"
   @customEventHook="customEventHook"
   @pageEventHook="pageEventHook"
   @search = "search"
 />
```



| 参数名称        | 参数说明                                                     | 是否必填 | 数据类型 | 备注                            |
| --------------- | ------------------------------------------------------------ | -------- | -------- | ------------------------------- |
| headerField     | 分页表头字段                                                 | 是       | Array    | 详见下面说明                    |
| tableData       | 分页数据                                                     | 是       | Array    |                                 |
| page            | 分页对象 ![image-20220627150642643](C:\Users\杨洋\AppData\Roaming\Typora\typora-user-images\image-20220627150642643.png) | 是       | Object   |                                 |
| orderNumber     | 自增序号                                                     | 否       | Boolean  |                                 |
| checkBox        | 分页中是否需要带上勾选框                                     | 否       | Array    |                                 |
| buttonListRight | 分页右上方按钮                                               | 否       | Array    | 详见下面说明                    |
| buttonListRow   | 分页行内按钮                                                 | 否       | Array    | 详见下面说明                    |
| signRule        | 格式化字段                                                   | 否       | Object   | 只有类型 pageType = sign 才有效 |
| operationHide   | 操作栏隐藏条件                                               | 否       | Object   |                                 |
| customEventHook | 按钮自定义动作                                               | 否       | function | 详见下方方法说明                |
| pageEventHook   | 翻页动作                                                     | 否       | function | 详见下方方法说明                |
| search          | 搜索动作                                                     | 否       | function | 详见下方方法说明                |
| eventField      | 字段点击动作 只有字段pageType= link才会触发                  | 否       | function | 详见下方方法说明                |
| sumField        | 分页列合计                                                   | 否       | Array    |                                 |
| customColumn    | 显示列定制，默认不显示                                       | 否       | Boolean  |                                 |

```json
signRule:{
    name:[
        // 1等于 2包含 3大于 4小于
        type:1,
        comparison:"小明,
        css:{
        	color:"red"
        }
    ]
}
备注：name分页中需要格式化的字段名， type 类型，comparison 比对的值， css 需要改变的样式
```

```
operationHide:{
	edit_tp:{
		condition:[1,2],
		field:"id"
	}
}
备注：edit_tp为需要隐藏的行内按钮action名，condition操作栏隐藏条件，类型为数组 field为隐藏条件字段
```

```
sumField:{
	sum:["age","total"]
}
备注：分页中 age字段和total字段需要合计
```



### 2.分页字段说明

```json
headerField:[
    { title: '名称', field: 'name', width: "120px", search: true, type: 1 ,fixed:"left"},
    { title: '年龄', field: 'age', width: "120px", search: true, type: 1 ,fixed:"left",pageType:"link"}
]
```

| 参数名称   | 参数说明       | 是否必填             | 数据类型 | 备注                                                        |
| ---------- | -------------- | -------------------- | -------- | ----------------------------------------------------------- |
| title      | 字段显示值     | 是                   | String   |                                                             |
| field      | 字段存储值     | 是                   | String   |                                                             |
| width      | 字段宽度       | 否                   | String   | 所有字段都没有宽度时，字段过多不会显示横向滚动条，会显示... |
| search     | 是否为搜索条件 | 否                   | Boolean  |                                                             |
| type       | 搜索类型       | 当search为true是必填 | Number   | 可选项:1单行文本\4下拉框\6系统按钮\7日期                    |
| fixed      | 列冻结         | 否                   | String   | 可选项:"left"\"right"                                       |
| pageType   | 字段类型       | 否                   | String   | 可选项:"link"\“switch”\“sign\definedColumn”                 |
| pageIsHide | 字段是否显示   | 否                   | Boolean  |                                                             |
| slotName   | 自定义插槽名   | 否                   | String   | 只有当pageType为definedColumn时才会生效                     |



### 3.按钮说明

```json
topButtonList:[
    {
        action:"add", //按钮动作名
        type:"primary", //按钮类型
        icon:"el-icon-document-add", //图标样式
        visible:false, //是否显示 默认显示
        title:"新增" //按钮名称
    },
    {
        action:"delete", //按钮动作名
        type:"primary", //按钮类型
        icon:"el-icon-document-del", //图标样式
        visible:false, //是否显示 默认显示
        title:"删除", //按钮名称
        tickData:true //勾选数据
    }
],
rowFixedButtonList:[
    {
        action:"edit_tp", //按钮动作名
        type:"primary", //按钮类型
        icon:"el-icon-document-edit", //图标样式
        visible:false, //是否显示 默认显示
        title:"编辑" //按钮名称
    }
]
```

### 4.方法说明

#### 4.1 按钮方法

```vue
// 按钮动作 右上角与行内按钮都可用此方法
customEventHook(event){
	// event 为按钮中数组对象
}
```

#### 4.2 翻页动作

```vue
pageEventHook(event){
	// event为一个对象，其中包括当前页及当前一页多少行
}
```

#### 4.3搜索动作

```vue
search(event){
	// event为搜索表单对象
}
```

#### 4.4字段点击动作

```vue
eventField(event){
	//event为字段对象
}
```

