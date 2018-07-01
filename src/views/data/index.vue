<template>
    <div>
        <el-table :data="tableData" border style="width: 99%">
            <el-table-column label="近日概况">
                <el-table-column prop="target" label="指标" align='center'>
                </el-table-column>
                <el-table-column prop="newUser" label="新增用户" align='center'>
                </el-table-column>
                <el-table-column prop="useUser" label="使用用户" align='center'>
                </el-table-column>
                <el-table-column prop="newUserP" label="新用户占比" align='center'>
                </el-table-column>
                <el-table-column prop="sessionTime" label="会话次数" align='center'>
                </el-table-column>
                <el-table-column prop="pSessionTime" label="人均会话次数" align='center'>
                </el-table-column>
            </el-table-column>
        </el-table>
        <el-table :data="tableData2" border style="width: 44%;display: inline-block;margin-top: 20px;">
            <el-table-column label="总体摘要">
                <el-table-column prop="head" show-header='false' align='center'>
                </el-table-column>
                <el-table-column prop="content" show-header='false' align='center'>
                </el-table-column>
            </el-table-column>
        </el-table>
        <el-table :data="tableData3" border style="width: 44%;display: inline-block;margin-left: 11%;margin-top: 20px;">
            <el-table-column label="使用概况">
                <el-table-column prop="head" show-header='false' align='center'>
                </el-table-column>
                <el-table-column prop="content" show-header='false' align='center'>
                </el-table-column>
            </el-table-column>
        </el-table>
        <el-row>
            <el-col :span="24">
                <b>30天趋势分析</b>
                <el-radio-group v-model="radio3" size='mini' class='buttonFloat' @change='radioButtonChange'>
                    <el-radio-button label="累计用户"></el-radio-button>
                    <el-radio-button label="新增用户"></el-radio-button>
                    <el-radio-button label="使用用户"></el-radio-button>
                    <el-radio-button label="会话次数"></el-radio-button>
                </el-radio-group>
            </el-col>
            <el-col :span='24'>
                <div id="echartsDemo"></div>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="24">
                <b>访问来源饼状图</b>
            </el-col>
            <el-col :span='24'>
                <div id="echartsDemo2"></div>
            </el-col>
        </el-row>
    </div>
</template>
<script>
    import echarts from 'echarts'

    export default {
        data() {
            return {
                chartDemo: null,
                chartDemo2: null,
                radio3: '会话次数',
                tableData: [{
                        target: '昨日',
                        newUser: 0,
                        useUser: 1,
                        newUserP: '0.00%',
                        sessionTime: 9,
                        pSessionTime: '9.00'
                    },
                    {
                        target: '预计今日',
                        newUser: 0,
                        useUser: 1,
                        newUserP: '0.00%',
                        sessionTime: 9,
                        pSessionTime: '9.00'
                    }
                ],
                tableData2: [{
                        head: "累计用户总数",
                        content: 4
                    },
                    {
                        head: "一次性用户(%)",
                        content: '0(0.00 %)'
                    },
                    {
                        head: "会话次数累计(人均)",
                        content: '2,701(675.25)'
                    }
                ],
                tableData3: [{
                        head: "首日留存率",
                        content: '0%'
                    },
                    {
                        head: "月活跃",
                        content: 1
                    },
                    {
                        head: "月活跃率",
                        content: '50%'
                    }
                ]
            }
        },
        mounted() {
            this.initChartLine()
            this.initChartPie()
        },
        activeated() {
            if (this.chartDemo) {
                this.chartDemo.resize()
            }
            if (this.chartDemo2) {
                this.chartDemo2.resize()
            }
        },
        methods: {
            initChartLine() {
                var chartData = [
                    ["2000-06-05", 116],
                    ["2000-06-06", 129],
                    ["2000-06-07", 135],
                    ["2000-06-08", 86],
                    ["2000-06-09", 73],
                    ["2000-06-10", 85],
                    ["2000-06-11", 73],
                    ["2000-06-12", 68],
                    ["2000-06-13", 92],
                    ["2000-06-14", 130],
                    ["2000-06-15", 245],
                    ["2000-06-16", 139],
                    ["2000-06-17", 115],
                    ["2000-06-18", 111],
                    ["2000-06-19", 309],
                    ["2000-06-20", 206],
                    ["2000-06-21", 137],
                    ["2000-06-22", 128],
                    ["2000-06-23", 85],
                    ["2000-06-24", 94],
                    ["2000-06-25", 71],
                    ["2000-06-26", 106],
                    ["2000-06-27", 84],
                    ["2000-06-28", 93],
                    ["2000-06-29", 85],
                    ["2000-06-30", 73],
                    ["2000-07-01", 83],
                    ["2000-07-02", 125],
                    ["2000-07-03", 107],
                    ["2000-07-04", 82],
                    ["2000-07-05", 44]
                ]
                var option = {
                    'title': {
                        'text': ''
                    },
                    'tooltip': {
                        'trigger': 'axis'
                    },
                    'grid': {
                        'left': '3%',
                        'right': '4%',
                        'bottom': '3%',
                        'containLabel': true
                    },
                    'toolbox': {
                        'feature': {
                            'saveAsImage': {}
                        },
                        'right': 30
                    },
                    'xAxis': {
                        'data': chartData.map(function (item) {
                            return item[0];
                        })
                    },
                    'yAxis': {
                        'type': 'value'
                    },
                    'series': {
                        'type': 'line',
                        'data': chartData.map(function (item) {
                            return item[1];
                        }),
                        areaStyle: {}
                    }
                }
                this.chartDemo = echarts.init(document.getElementById('echartsDemo'))
                this.chartDemo.setOption(option)
                window.addEventListener('resize', () => {
                    this.chartDemo.resize()
                })
            },
            initChartLine2() {
                var chartData = [
                    ["2000-06-05", 444],
                    ["2000-06-06", 346],
                    ["2000-06-07", 390],
                    ["2000-06-08", 86],
                    ["2000-06-09", 42],
                    ["2000-06-10", 78],
                    ["2000-06-11", 23],
                    ["2000-06-12", 73],
                    ["2000-06-13", 123],
                    ["2000-06-14", 11],
                    ["2000-06-15", 33],
                    ["2000-06-16", 88],
                    ["2000-06-17", 111],
                    ["2000-06-18", 44],
                    ["2000-06-19", 45],
                    ["2000-06-20", 345],
                    ["2000-06-21", 45],
                    ["2000-06-22", 128],
                    ["2000-06-23", 49],
                    ["2000-06-24", 94],
                    ["2000-06-25", 111],
                    ["2000-06-26", 55],
                    ["2000-06-27", 123],
                    ["2000-06-28", 93],
                    ["2000-06-29", 85],
                    ["2000-06-30", 222],
                    ["2000-07-01", 142],
                    ["2000-07-02", 99],
                    ["2000-07-03", 107],
                    ["2000-07-04", 82],
                    ["2000-07-05", 44]
                ]
                var option = {
                    'title': {
                        'text': ''
                    },
                    'tooltip': {
                        'trigger': 'axis'
                    },
                    'grid': {
                        'left': '3%',
                        'right': '4%',
                        'bottom': '3%',
                        'containLabel': true
                    },
                    'toolbox': {
                        'feature': {
                            'saveAsImage': {}
                        },
                        'right': 30
                    },
                    'xAxis': {
                        'data': chartData.map(function (item) {
                            return item[0];
                        })
                    },
                    'yAxis': {
                        'type': 'value'
                    },
                    'series': {
                        'type': 'line',
                        'data': chartData.map(function (item) {
                            return item[1];
                        }),
                        areaStyle: {}
                    }
                }
                this.chartDemo = echarts.init(document.getElementById('echartsDemo'))
                this.chartDemo.setOption(option)
                window.addEventListener('resize', () => {
                    this.chartDemo.resize()
                })
            },
            radioButtonChange(e) {
                console.log(e);
                if (e == '会话次数') {
                    this.initChartLine()
                }
                if (e == '累计用户') {
                    this.initChartLine2()
                }
            },
            initChartPie() {
                var option = {
                    backgroundColor: '#2c343c',
                    title: {
                        text: '',
                        left: 'center',
                        top: 20,
                        textStyle: {
                            color: '#ccc'
                        }
                    },
                    tooltip: {
                        trigger: 'item',
                        formatter: '{a} <br/>{b} : {c} ({d}%)'
                    },
                    visualMap: {
                        show: false,
                        min: 80,
                        max: 600,
                        inRange: {
                            colorLightness: [0, 1]
                        }
                    },
                    series: [{
                        name: '访问来源',
                        type: 'pie',
                        radius: '90%',
                        center: ['50%', '50%'],
                        data: [{
                                value: 335,
                                name: '直接访问'
                            },
                            {
                                value: 310,
                                name: '邮件营销'
                            },
                            {
                                value: 274,
                                name: '联盟广告'
                            },
                            {
                                value: 235,
                                name: '视频广告'
                            },
                            {
                                value: 400,
                                name: '搜索引擎'
                            }
                        ].sort(function (a, b) {
                            return a.value - b.value
                        }),
                        roseType: 'radius',
                        label: {
                            normal: {
                                textStyle: {
                                    color: 'rgba(255, 255, 255, 0.3)'
                                }
                            }
                        },
                        labelLine: {
                            normal: {
                                lineStyle: {
                                    color: 'rgba(255, 255, 255, 0.3)'
                                },
                                smooth: 0.2,
                                length: 10,
                                length2: 20
                            }
                        },
                        itemStyle: {
                            normal: {
                                color: '#c23531',
                                shadowBlur: 200,
                                shadowColor: 'rgba(0, 0, 0, 0.5)'
                            }
                        },
                        animationType: 'scale',
                        animationEasing: 'elasticOut',
                        animationDelay: function (idx) {
                            return Math.random() * 200
                        }
                    }]
                }
                this.chartDemo2 = echarts.init(document.getElementById('echartsDemo2'))
                this.chartDemo2.setOption(option)
                window.addEventListener('resize', () => {
                    this.chartDemo2.resize()
                })
            }
        }
    }
</script>
<style lang="scss">
    .el-row {
        margin-top: 20px;
        color: #606266;
    }

    .el-col {
        border: solid 1px #ebeef5;
        &:first-child {
            border-bottom: none;
            padding: 10px 10px 10px 10px;
            >b {
                font-size: 20px;
            }
        }
        &:last-child {
            min-height: 400px;
            >div {
                min-height: 400px;
                width: 100%;
            }
        }
    }

    .buttonFloat {
        float: right;
    }
</style>
