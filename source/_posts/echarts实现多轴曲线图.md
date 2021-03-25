---
title: echarts实现多轴曲线图
date: 2021-03-25 20:27:27
tags:
---

## 1. Echart实现多个x轴或y轴曲线图

效果图如下：

{% asset_img line.png This is an example image %}

<!-- more -->

### 1.1 配置option对象

```javascript
option:{
    // 设置 x 轴的样式
    xAxis:{},
    // 设置 y 轴的样式
    yAxis:[],
    // 设置每条曲线的数据和样式
    series:[],
    // 设置鼠标hover时的提示信息
    tooltip:{},
    // 调整表格两边的空白的区域
    grid:{},
    // 调整图样的名称
    legend:{}
    // 定义图样和每条曲线的颜色
    color:[]
}
```



+ yAixs 用来配置  y 轴的数据，样式和名称，当把 yAxis 设置为一个数组，且给数组添加多个对象时，就可以实现多个 y 轴的效果。

  + 但是这些 y 轴都是重叠的，我们可以通过每个 y 轴的 `offset` 属性来调整 y 轴使它们到达合适的位置
  + 通过 `axisTick`  和`axisLabel` 属性可以分别设置 y 轴上的刻度和刻度值的位置，是在左侧显示还是在右侧
  + 如果表格左右的留白空间不足以放下多个 y 轴，可以通过设置  `grid ` 属性来调整空白和图像的比例
  + 具体实现代码如下：

  ```javascript
  option = {
   yAxis: [
                {
                  name: '指标参数一(单位)',
                  type: 'value',
                  // max: 700,
                  // min: 0,
                  // 让表格的刻度向靠里侧显示
                  axisTick: {
                    inside: true
                  },
                  axisLabel: {
                    inside: true
                  },
                  // 设置刻度线的颜色等样式
                  axisLine: {
                    lineStyle: {
                      color: 'red',
                      width: 3
                    }
                  },
                  splitLine: {
                    show: true, //想要不显示网格线，改为false
                    lineStyle: {
                      // 设置网格为虚线
                      type: 'dashed'
                    }
                  }
                },
                {
                  name: '指标参数二(单位)',
                  // max: 800,
                  // min: 0,
                  type: 'value',
                  axisLine: {
                    lineStyle: {
                      color: 'orange',
                      width: 3
                    }
                  },
                  splitLine: {
                    show: false //想要不显示网格线，改为false
                  },
                  // 设置坐标轴偏移位置
                  offset: -1060
                },
                {
                  name: '指标参数三(单位)',
                  // max: 900,
                  // min: 0,
                  type: 'value',
                  axisLine: {
                    lineStyle: {
                      color: 'green',
                      width: 3
                    }
                  },
                  offset: -1160,
                  splitLine: {
                    show: false //想要不显示网格线，改为false
                  }
                },
                {
                  name: '指标参数四(单位)',
                  // max: 1000,
                  // min: 0,
                  type: 'value',
                  axisTick: {
                    inside: true
                  },
                  axisLabel: {
                    inside: true
                  },
                  axisLine: {
                    lineStyle: {
                      color: 'blue',
                      width: 3
                    }
                  },
                  splitLine: {
                    show: false //想要不显示网格线，改为false
                  }
                },
                {
                  name: '指标参数五(单位)',
                  // max: 1100,
                  // min: 0,
                  type: 'value',
                  offset: 100,
                  axisTick: {
                    inside: true
                  },
                  axisLabel: {
                    inside: true
                  },
                  axisLine: {
                    lineStyle: {
                      color: 'purple',
                      width: 3
                    }
                  },
                  splitLine: {
                    show: false //想要不显示网格线，改为false
                  }
                },
                {
                  name: '指标参数六(单位)',
                  // max: 700,
                  // min: 0,
                  type: 'value',
                  offset: 200,
                  axisTick: {
                    inside: true
                  },
                  axisLabel: {
                    inside: true
                  },
                  axisLine: {
                    lineStyle: {
                      color: 'pink',
                      width: 3
                    }
                  },
                  splitLine: {
                    show: false //想要不显示网格线，改为false
                  }
                }
              ],
      // 调整表格两边空白的区域
   grid: {
       		  // 左侧
                x: '20%',
                // 上部
                // y: 25,
                // 右侧
                x2: '20%'
                // 下部
                // y2: 35
              },
  }
  ```

+ xAxis，配置 x 轴数据、样式、名称，如果想设置多个 x 轴和上述  y 轴同理

+ 当 x 轴的 type 是 'time' 类型时，刻度值需要在`series` 属性中设置具体见下文

  ```javascript
  xAxis: {
                name: '时间',
                type: 'time',
                // boundaryGap: false, //x下标在刻度处显示
                splitLine: {
                  show: true, //想要不显示网格线，改为false
                  lineStyle: {
                    // 设置网格为虚线
                    type: 'dashed'
                  }
                },
                // splitArea: { show: true }, //保留网格区域
                // 设置刻度线的颜色等样式
                axisLine: {
                  lineStyle: {
                    color: 'black',
                    width: 3
                  }
                }
              }
  ```

+ series，用于配置每条曲线的名称、曲线中点的值，和 x 轴的时间刻度值

  + series 为一个数组时，里面的每一个对象都是一条曲线，所以可以配置多条曲线。
  + 通过  `yAxisIndex` 可以设置每个曲线对应的上面的 yAxis 中的 y 轴的索引，把曲线和不同的 y 轴匹配起来
  + `smooth` 为 true 时，则可以折线变成曲线

  ```javascript
  series: [
                {
                  // 曲线数据配置
                  data: [
                    {
                      // value[0] 为时间  value[1] 为值
                      value: ['1997-10-1', 300]
                    },
                    {
                      value: ['1997-10-2', 200]
                    },
                    {
                      value: ['1997-10-3', 500]
                    },
                    {
                      value: ['1997-10-4', 600]
                    },
                    {
                      value: ['1997-10-5', 400]
                    },
                    {
                      value: ['1997-10-6', 200]
                    }
                  ],
                  // 曲线名
                  name: '参数1',
                  // 设置参数对应的y坐标轴的索引
                  type: 'line',
                  // 曲线平滑设置
                  smooth: true
                },
                {
                  data: [
                    {
                      value: ['1997-10-1', 180]
                    },
                    {
                      value: ['1997-10-2', 250]
                    },
                    {
                      value: ['1997-10-3', 50]
                    },
                    {
                      value: ['1997-10-4', 450]
                    },
                    {
                      value: ['1997-10-5', 400]
                    },
                    {
                      value: ['1997-10-6', 200]
                    }
                  ],
                  // 曲线名
                  name: '参数2',
                  // 设置所在曲线对应的y坐标轴的索引
                  yAxisIndex: 1,
                  type: 'line',
                  // 曲线平滑设置
                  smooth: true
                },
                {
                  data: [
                    {
                      value: ['1997-10-1', 100]
                    },
                    {
                      value: ['1997-10-2', 800]
                    },
                    {
                      value: ['1997-10-3', 250]
                    },
                    {
                      value: ['1997-10-4', 350]
                    },
                    {
                      value: ['1997-10-5', 360]
                    },
                    {
                      value: ['1997-10-6', 500]
                    }
                  ],
                  name: '参数3',
                  // 设置参数对应的y坐标轴的索引
                  yAxisIndex: 2,
                  type: 'line',
                  // 曲线平滑设置
                  smooth: true
                },
                {
                  data: [
                    {
                      value: ['1997-10-1', 200]
                    },
                    {
                      value: ['1997-10-2', 400]
                    },
                    {
                      value: ['1997-10-3', 600]
                    },
                    {
                      value: ['1997-10-4', 800]
                    },
                    {
                      value: ['1997-10-5', 1000]
                    },
                    {
                      value: ['1997-10-6', 1100]
                    }
                  ],
                  name: '参数4',
                  // 设置参数对应的y坐标轴的索引
                  yAxisIndex: 3,
                  type: 'line',
                  // 曲线平滑设置
                  smooth: true
                },
                {
                  data: [
                    {
                      value: ['1997-10-1', 1100]
                    },
                    {
                      value: ['1997-10-2', 800]
                    },
                    {
                      value: ['1997-10-3', 600]
                    },
                    {
                      value: ['1997-10-4', 500]
                    },
                    {
                      value: ['1997-10-5', 400]
                    },
                    {
                      value: ['1997-10-6', 200]
                    }
                  ],
                  name: '参数5',
                  // 设置参数对应的y坐标轴的索引
                  yAxisIndex: 4,
                  type: 'line',
                  // 曲线平滑设置
                  smooth: true
                },
                {
                  data: [
                    {
                      value: ['1997-10-1', 700]
                    },
                    {
                      value: ['1997-10-2', 600]
                    },
                    {
                      value: ['1997-10-3', 500]
                    },
                    {
                      value: ['1997-10-4', 400]
                    },
                    {
                      value: ['1997-10-5', 300]
                    },
                    {
                      value: ['1997-10-6', 200]
                    }
                  ],
                  name: '参数6',
                  // 设置参数对应的y坐标轴的索引
                  yAxisIndex: 5,
                  type: 'line',
                  // 曲线平滑设置
                  smooth: true
                }
              ]
  ```

+ tooltip 属性用于配置鼠标放到曲线图上展示的数据展示样式

  

{% asset_img tip.png tip %}

```javascript
tooltip: {
              trigger: 'axis', // 有3个属性值 axis   item   none
              axisPointer: {
                type: 'cross',
                label: {
                  backgroundColor: '#6a7985' //配置展示方块的背景颜色
                }
              }
            }
```



+ 通过 legend 属性配置图样的文字信息和样式

 ```javascript
legend: {     // 调整图样文字
              data: ['参数1', '参数2', '参数3', '参数4', '参数5', '参数6']
        }
 ```



+ 通过 color 属性定义图样和曲线的颜色

```javascript
 // 定义图例和曲线颜色
 color: ['red', 'orange', 'green', 'blue', 'purple', 'pink']
```



### 1.2 创建HTML元素

设置要放入曲线的div的宽度和高度

```html
// 曲线图显示在这个div中，
<div id="myChart"></div>
// 这里设置的是高为600px ，宽为1600px
<style>
#myChart {
  width: 1600px;
  height: 600px;
}
</style>
```

### 1.3 渲染曲线图

```javascript
let myChart = echarts.init(document.getElementById('myChart'))
myChart.setOption(this.option)
```

