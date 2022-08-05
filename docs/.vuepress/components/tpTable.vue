<template>
    <div class="box">
        <el-table
            class="myTable"
            empty-text="暂无数据"
            ref="table"
            :data="tableList"
            border
            stripe
            fit
            highlight-current-row
            :max-height="tableHeight"
            :summary-method="getSummaries"
            show-summary
            :span-method="objectSpanMethod"
            @selection-change="selectionChange"
            @row-click="rowClick"
            @cell-click="cellClick"
        >
            <!-- 选择框 -->
            <el-table-column
                v-if="select"
                type="selection"
                fixed="left"
                width="55"
                align="center"
            />
            <template v-for="(itm, idx) in header">
                <!-- 特殊处理列 -->
                <el-table-column
                    v-if="itm.render"
                    :key="idx"
                    :prop="itm.prop ? itm.prop : null"
                    :label="itm.label ? itm.label : null"
                    :width="itm.width ? itm.width : null"
                    :sortable="itm.sortable ? itm.sortable : false"
                    :align="itm.align ? itm.align : 'center'"
                    :fixed="itm.fixed ? itm.fixed : null"
                    :show-overflow-tooltip="itm.tooltip"
                    min-width="50"
                >
                    <template slot-scope="scope">
                        <ex-slot
                            :render="itm.render"
                            :row="scope.row"
                            :index="scope.$index"
                            :column="itm"
                        />
                    </template>
                </el-table-column>
                <!-- 正常列 -->
                <el-table-column
                    v-else
                    :key="idx"
                    :prop="itm.prop ? itm.prop : null"
                    :label="itm.label ? itm.label : null"
                    :width="itm.width ? itm.width : null"
                    :sortable="itm.sortable ? itm.sortable : false"
                    :align="itm.align ? itm.align : 'center'"
                    :fixed="itm.fixed ? itm.fixed : null"
                    :formatter="itm.formatter"
                    :show-overflow-tooltip="itm.tooltip"
                    min-width="50"
                >
                    <el-table-column
                        v-if="
                            itm.children && Array.from(itm.children).length > 0
                        "
                        v-for="(itm2, idx2) in itm.children"
                        :key="idx2"
                        :prop="itm2.prop ? itm2.prop : null"
                        :label="itm2.label ? itm2.label : null"
                        :width="itm2.width ? itm2.width : null"
                        :sortable="itm2.sortable ? itm2.sortable : false"
                        :align="itm2.align ? itm2.align : 'center'"
                        :fixed="itm2.fixed ? itm2.fixed : null"
                        :formatter="itm2.formatter"
                        :show-overflow-tooltip="itm2.tooltip"
                    >
                        <el-table-column
                            v-if="
                                itm2.children &&
                                Array.from(itm2.children).length > 0
                            "
                            v-for="(itm3, idx3) in itm2.children"
                            :key="idx3"
                            :prop="itm3.prop ? itm3.prop : null"
                            :label="itm3.label ? itm3.label : null"
                            :width="itm3.width ? itm3.width : null"
                            :sortable="itm3.sortable ? itm3.sortable : false"
                            :align="itm3.align ? itm3.align : 'center'"
                            :fixed="itm3.fixed ? itm3.fixed : null"
                            :formatter="itm3.formatter"
                            :show-overflow-tooltip="itm3.tooltip"
                        />
                    </el-table-column>
                </el-table-column>
            </template>
        </el-table>
    </div>
</template>

<script>
// 自定义内容的组件
var exSlot = {
    functional: true,
    props: {
        row: Object,
        render: Function,
        index: Number,
        column: {
            type: Object,
            default: null,
        },
    },
    render: (h, context) => {
        const params = {
            row: context.props.row,
            index: context.props.index,
        };
        if (context.props.column) params.column = context.props.column;
        return context.props.render(h, params);
    },
};

export default {
    components: { exSlot },
    name: "tpTable",
    props: {
        // tableList: {
        //     type: Array,
        //     default: () => [],
        // },
        // header: {
        //     type: Array,
        //     default: () => [],
        // },
        select: {
            type: Boolean,
            default: () => false,
        },
        // sumData: {
        //     type: Object,
        //     default: () => {},
        // },
        // mergeColumns: {
        //     type: Array,
        //     default: () => [],
        // },
    },
    data() {
        return {
            //表格高度
            tableHeight: `${document.documentElement.clientHeight}` - 225,

            sumData:{
                type:"sum",
                sumField:['d']
            },

            mergeColumns:[
                {name:"w",index:1},
                {name:"x",index:2},
                {name:"d",index:3}
            ],

            header:[
                { prop: "w", label: "w" },
                {
                    prop: "x",
                    label: "x",
                    formatter: (row) => {
                        return row.x.toFixed(2);
                    },
                },
                {
                    prop: "d",
                    label: "d",
                    formatter: (row) => {
                        return row.d.toFixed(2);
                    },
                },
                {
                    label: "一级表头",
                    children:[
                        {
                            label:"二级表头1",
                            children:[
                                {
                                    prop: "a",
                                    label: "A",
                                    formatter: (row) => {
                                        return row.a.toFixed(3);
                                    },
                                }
                            ],
                        },
                        {
                            label:"二级表头2",
                            prop:"b"
                        }
                    ],
                },
                {
                    label: "操作",
                    render: (h, data) => {
                        return (
                            <el-button
                                type="primary"
                                onClick={() => {
                                    this.handleClick(data.row);
                                }}
                            >
                                点我获取行数据
                            </el-button>
                        );
                    },
                },
            ],

            tableList:[
                { w: 99.25123, x: 94.25123, d: 3,a:5.22665,b:"56po" },
                { w: 99.25123, x: 255.6666, d: 4,a:5.22665,b:"56po" },
                { w: 1, x: 99.25123, d: 3,a:5.22665,b:"56po" },
                { w: 2, x: 255.6666, d: 4,a:5.22665,b:"56po" },
                { w: 3, x: 99.25123, d: 3,a:5.22665,b:"56po" },
                { w: 1, x: 255.6666, d: 4,a:5.22665,b:"56po" },
                { w: 1, x: 99.25123, d: 4,a:5.22665,b:"56po" },
                { w: 1, x: 255.6666, d: 255.6666,a:5.22665,b:"56po" },
                { w: 1, x: 99.25123, d: 3,a:5.22665,b:"56po" },
                { w: 7, x: 255.6666, d: 255.6666,a:5.22665,b:"56po" },
                { w: 1, x: 99.25123, d: 3,a:5.22665,b:"56po" },
                { w: 1, x: 255.6666, d: 4,a:5.22665,b:"56po" }
            ],

            tableMergeData: []
        };
    },
    created() {
        //该阶段可以接收父组件的传递参数
        this.inTableHeight = this.height;
    },
    mounted() {
        window.addEventListener("resize", this.tableHight); // 添加监听数据 引用-> method 的 tableHight
        if (this.mergeColumns.length > 0) {
            this.$nextTick(() => {
                this.generateTableMergeData()
            })
        }
    },
    destroyed() {
        window.removeEventListener("resize", this.tableHight); // 组件注销移除监听
    },
    watch: {},
    methods: {
        /**
         * 选择框选择后更改,事件方法
         */
        selectionChange(selection) {
            this.$emit("selectionChange", selection);
        },
        /**
         * 行点击事件方法
         */
        rowClick(row, column, event) {
            this.$emit("row-click", row, column, event);
        },
        /**
         * 单元格点击事件方法
         */
        cellClick(row, column, cell, event){
            this.$emit("cell-click", row, column,cell, event);
        },

        // 主要以监听窗口变化，让table出现滚动条
        tableHight() {
            if (document.documentElement.clientHeight - 225 > 73)
                this.tableHeight =
                    `${document.documentElement.clientHeight}` - 225;
            else {
                this.tableHeight = 73;
            }
        },

        /**
         * 表尾合计方法
         */
        getSummaries(param) {
            if (this.sumData) {
                const { columns, data } = param;
                const sums = [];
                //如果是合计
                if (this.sumData.type === "sum") {
                    columns.forEach((column, index) => {
                        if (index === 0) {
                            sums[index] = "合计";
                            return;
                        }
                        if (this.sumData.sumField.includes(column.property)) {
                            const values = data.map((item) =>
                                Number(item[column.property])
                            );
                            if (!values.every((value) => isNaN(value))) {
                                sums[index] = values.reduce((prev, curr) => {
                                    const value = Number(curr);
                                    if (!isNaN(value)) {
                                        return prev + curr;
                                    } else {
                                        return prev;
                                    }
                                }, 0);
                            } else {
                                sums[index] = "N/A";
                            }
                        } else {
                            sums[index] = "N/A";
                        }
                    });
                    return sums;
                }
            }
        },
        
        /**
         * element表格合并方法
         * @param rowIndex 行坐标
         * @param columnIndex 列坐标
         * @returns {*} 单元格坐标信息
         */
        objectSpanMethod({ row, column, rowIndex, columnIndex }){
            if(this.mergeColumns && this.mergeColumns.length > 0){
                const key = columnIndex + '_' + rowIndex
                if (this.tableMergeData[key]) {
                    return this.tableMergeData[key]
                }
            }else{
                this.$emit('objectSpanMethod',{row, column, rowIndex, columnIndex})
            }
        },

        /**
         * 生成合并表格信息
         */
        generateTableMergeData() {
            // 遍历表格中需要合并的所有单元格
            for (let i = 0; i < this.tableList.length; i++) {
                for (let j = 0; j < this.mergeColumns.length; j++) {
                    // 初始化行、列坐标信息
                    let rowIndex = 1
                    let columnIndex = 1
                    // 比较横坐标左方的第一个元素
                    if (j > 0 && this.tableList[i][this.mergeColumns[j]['name']] === this.tableList[i][this.mergeColumns[j - 1]['name']]) {
                        columnIndex = 0
                    }
                    // 比较纵坐标上方的第一个元素
                    if (i > 0 && this.tableList[i][this.mergeColumns[j]['name']] === this.tableList[i - 1][this.mergeColumns[j]['name']]) {
                        rowIndex = 0
                    }
                    // 比较横坐标右方元素
                    if (columnIndex > 0) {
                        columnIndex = this.calculateColumnIndex(this.tableList[i], j, j + 1, 1, this.mergeColumns.length)
                    }
                    // 比较纵坐标下方元素
                    if (rowIndex > 0) {
                        rowIndex = this.calculateRowIndex(this.tableList, i, i + 1, 1, this.mergeColumns[j]['name'])
                    }
                    const key = this.mergeColumns[j]['index'] + '_' + i
                    this.tableMergeData[key] = [rowIndex, columnIndex]
                }
            }
        },
        /**
         * 计算列坐标信息
         * @param data 单元格所在行数据
         * @param index 当前下标
         * @param nextIndex 下一个元素坐标
         * @param count 相同内容的数量
         * @param maxLength 当前行的列总数
         * @returns {*}
         */
        calculateColumnIndex(data, index, nextIndex, count, maxLength) {
            // 比较当前单元格中的数据与同一行之后的单元格是否相同
            if (nextIndex < maxLength && data[this.mergeColumns[index]['name']] === data[this.mergeColumns[nextIndex]['name']]) {
                return this.calculateColumnIndex(data, index, ++nextIndex, ++count, maxLength)
            }
            return count
        },
        /**
         * 计算行坐标信息
         * @param data 单元格所在行数据
         * @param index 当前下标
         * @param nextIndex 下一个元素坐标
         * @param count 相同内容的数量
         * @param name 数据的key
         * @returns {*}
         */
        calculateRowIndex(data, index, nextIndex, count, name) {
            // 比较当前单元格中的数据与同一列之后的单元格是否相同
            if (nextIndex < data.length && data[index][name] === data[nextIndex][name]) {
                return this.calculateRowIndex(data, index, ++nextIndex, ++count, name)
            }
                return count
        }
    },
};
</script>
<style scoped>
.myTable{
    width: 80%;
}
.myTable ::-webkit-scrollbar {
    width: 10px;
    height: 10px;
}
.myTable ::-webkit-scrollbar-track {
    background-color: #ffffff;
}
.myTable ::-webkit-scrollbar-thumb {
    background-color: #bfbfbf;
    border-radius: 5px;
    border: 1px solid #f1f1f1;
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}
.myTable ::-webkit-scrollbar-thumb:hover {
    background-color: #a8a8a8;
}
.myTable ::-webkit-scrollbar-thumb:active {
    background-color: #787878;
}
.myTable ::-webkit-scrollbar-corner {
    background-color: #ffffff;
}
</style>
