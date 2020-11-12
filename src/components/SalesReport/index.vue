<template>
    <div class="sales-report">
        <div class="header">电商商品销售趋势图</div>
        <div class="content">
            <div class="content-title-wrapper">月销售增长率</div>
            <div class="content-index-wrapper">
                <span class="arrow-up">
                  <svg t="1605067465192" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2924"><path d="M904.533333 674.133333l-362.666666-362.666666c-17.066667-17.066667-42.666667-17.066667-59.733334 0l-362.666666 362.666666c-17.066667 17.066667-17.066667 42.666667 0 59.733334 17.066667 17.066667 42.666667 17.066667 59.733333 0L512 401.066667l332.8 332.8c8.533333 8.533333 19.2 12.8 29.866667 12.8s21.333333-4.266667 29.866666-12.8c17.066667-17.066667 17.066667-42.666667 0-59.733334z" p-id="2925" fill="#3AC079"></path></svg>                
                </span>
                <sapn class="percentage">34<span>%</span></sapn>
                <span class="text">+14,000</span>
            </div>
            <div id="content-chart" /> 
            <div class="content-circle-wrapper">
                <!-- class绑定数组 -->
                <div 
                :class="['circle',selectedIndex === index ? 'selected' : '']" 
                v-for="(item,index) in circle" 
                :key="index"
                @click="change(index)" 
                />
            </div>
            <div class="content-bottom-wrapper">销售趋势</div>
        </div>
        <div class="footer">
            <div class="footer-wrapper">
                <div class="left">
                    <div class="footer-title">订单销售额</div>
                    <div class="footer-sub-title">3月累计销售额</div>
                </div>
                <div class="right">
                    <small>￥</small> 300,254.00
                </div>
            </div>
            <div class="progress-wrapper">
                <div class="progress-bg">
                    <div class="progress-current" :style="{'width':`${process*100}%`}"></div>
                </div>
            </div>
            <div class="footer-text">
                <div>销售增长率</div>
                <div>34%</div>
            </div>
        </div>
    </div>
</template>

<script>
import ECharts from 'echarts'
export default {
    data(){
        return{
            circle: new Array(3),
            selectedIndex: 0,
            process: 0.35,
        }
    },
    mounted(){
        this.genChart();
        this.task = setInterval(() => {
            let index = this.selectedIndex
            index++
            if(index >= this.circle.length){
                index = 0
            }
            this.change(index)
        },3000)
    },
    //每次修改保存后都会挂载setInterval方法，需要判断并销毁
    destroyed(){
        if(this.task){
            clearInterval(this.task)
        }
    },
    methods:{
        change(index){
            this.selectedIndex = index;
            this.genChart();
        },
        genChart(){
            //获取数据源
            const mockData = []
            for (let i = 0; i < 10; i++) {
                //随机生成模拟数据
                mockData.push(Math.floor(Math.random() * 100)+200)
            }
            console.log(mockData)
            //获取chart对应的Dom
            const chartDom = document.getElementById('content-chart')
            //初始化echarts对象
            const chart = ECharts.init(chartDom)
            //生成渲染参数
            const options = {
                xAxis:{
                    type: 'category',//默认为value，线性直线
                    show: false //隐藏x轴
                },
                yAxis:{
                    min: 0,
                    max: 350,
                    show: false
                },
                series:{
                    data: mockData,
                    type: 'line',
                    smooth: true,
                    //区域样式
                    areaStyle:{
                        color: 'rgba(75,192,252,.5)'
                    },
                    //折线样式
                    lineStyle:{
                        width: 4,
                        color: 'rgba(75,192,252,1)'
                    },
                    itemStyle:{
                        borderWidth:8,
                        color: 'rgba(75,192,252,1)'
                    }
                },
                grid:{
                    top: 0,
                     bottom: 0,
                     left: -30,
                     right: -30,   
                },
                tooltip:{
                    trigger: 'axis',
                    axisPointer:{
                        type: 'cross',
                        label:{
                            backgroundColor: '#6a7985'
                        }
                    }
                }

            }
            //渲染图表
            chart.setOption(options)
        }
    }
}
</script>
<style lang="scss">
.sales-report{
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    background: #ffffff;
    box-shadow: 0 2px 8px rgba(4, 9, 20, .03),0 2px 8px rgba(4, 9, 20, .03),0 2px 8px rgba(4, 9, 20, .03),0 2px 8px rgba(4, 9, 20, .03);
    .header{
        display: flex;
        align-items: center;
        width: 100%;
        height: 50px;
        border: 1px solid #eee;
        box-sizing: border-box;
        padding-left: 20px;
    }
    .content{
        flex: 1;
        display: flex;
        flex-direction: column;
        width: 100%;
        padding: 0 28px;
        box-sizing: border-box;
        .content-title-wrapper{
            padding-top: 28px;
            color: rgb(108,117,125);
            font-size: 13px;
        }
        .content-index-wrapper{
            display: flex;
            align-items: center;
            margin-top: 15px;
            .arrow-up{
                svg{
                    width: 25px;
                }
            }
            .percentage{
                font-size: 35px;
                font-weight: 700;
                color: #333;
                margin-left: 15px;
                span{
                    font-size: 28px;
                    font-weight: 400;
                    color: #999;
                    margin-left: 2px;
                }
            }
            .text{
                margin-left: 15px;
                color: rgb(58, 196, 125);
            }
        }
        #content-chart{
            flex: 1;
            width: 100%;
        }
        .content-circle-wrapper{
            display: flex;
            align-items: center;
            justify-content: center;
            margin-top: 20px;
            .circle{
                width: 10px;
                height: 10px;
                background: #fff;
                border: 3px solid rgb(63, 106, 216);
                border-radius: 50%;
                margin: 0 5px;
                &.selected{
                    width: 12px;
                    height: 12px;
                    border: 5px solid rgb(63, 106, 216);
                }
            }
        }
        .content-bottom-wrapper{
            margin: 10px 0;
            color: #999;
        }
    }
    .footer{
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 120px;
        border: 1px solid #eee;
        box-sizing: border-box;
        .footer-wrapper{
            display: flex;
            padding: 14px 14px 0;
            .left{
                .footer-title{
                    font-size: 13px;
                    font-weight: 700;
                    color: #333;
                }
                .footer-sub-title{
                    font-size: 13px;
                    color: #999;
                }
            }
            .right{
                flex: 1;
                text-align: right;
                color: rgb(58, 196, 125);
                font-size: 25px;
                font-weight: 700;
                small{
                    font-weight: 400;
                }
            }
        }
        .progress-wrapper{
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 14px;
            .progress-bg{
                width: 100%;
                height: 7px;
                position: relative;
                background: rgb(233, 236, 239);
                border-radius: 3.5px;
                .progress-current{
                    position: absolute;
                    left: 0;
                    top: 0;
                    height: 7px;
                    background: #3f6ad8;
                    border-radius: 3.5px;
                    //实现蒙层
                    &::after{
                        content: "";
                        position: absolute;
                        top: 0;
                        left: 0;
                        right: 0;
                        bottom: 0;
                        width: 100%;
                        background: #ffffff;
                        opacity: 0;
                        animation: process-active 2s ease infinite ;
                    }
                    //动画过渡
                    @keyframes process-active {
                        from{
                            width: 0;
                            opacity: 0;
                        }
                        to{
                            width: 100%;
                            opacity: .3;
                        }
                    }
                }
            }
        }
        .footer-text{
            display: flex;
            justify-content: space-between;
            padding: 0 14px 14px;
            font-size: 13px;
            color: #999;
        }
    }
}
</style>