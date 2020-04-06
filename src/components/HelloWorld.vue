<template>
  <div id="main">
    <button>BTN</button>
    <div id="myChart" ref="myChart"></div>
  </div>
</template>
<script>
import echarts from "echarts";

export default {
  name: "echarts",
  data() {
    return {
      resData: "",
      echartsOption: {
        title: {
          text: "指數",
          left: 0
        },
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross"
          }
        },
        legend: {
          data: ["日K", "MA5", "MA10", "MA20", "MA30"]
        },
        grid: {
          left: "10%",
          right: "10%",
          bottom: "15%"
        },
        xAxis: {
          type: "category",
          data: [],
          scale: true,
          boundaryGap: false,
          axisLine: { onZero: false },
          splitLine: { show: false },
          splitNumber: 20,
          min: "dataMin",
          max: "dataMax"
        },
        yAxis: {
          scale: true,
          splitArea: {
            show: true
          }
        },
        dataZoom: [
          {
            type: "inside",
            start: 30,
            end: 70
          },
          {
            show: true,
            type: "slider",
            y: "90%",
            start: 0,
            end: 75
          }
        ],
        series: [
          {
            name: "日K",
            type: "k",
            data: [],
            itemStyle: {
              normal: {
                color: "#D75442",
                color0: "#6BA583",
                borderColor: "#8A0000",
                borderColor0: "#008F28"
              }
            },
            markPoint: {
              label: {
                normal: {
                  formatter: function(param) {
                    return param != null ? Math.round(param.value) : "";
                  }
                }
              },
              data: [
                {
                  name: "highest value",
                  type: "max",
                  valueDim: "highest"
                },
                {
                  name: "lowest value",
                  type: "min",
                  valueDim: "lowest"
                },
                {
                  name: "average value on close",
                  type: "average",
                  valueDim: "close"
                }
              ],
              tooltip: {
                formatter: function(param) {
                  return param.name + "<br>" + (param.data.coord || "");
                }
              }
            },
            markLine: {
              symbol: ["none", "none"],
              data: [
                [
                  {
                    name: "from lowest to highest",
                    type: "min",
                    valueDim: "lowest",
                    symbol: "circle",
                    symbolSize: 10,
                    label: {
                      normal: { show: false },
                      emphasis: { show: false }
                    }
                  },
                  {
                    type: "max",
                    valueDim: "highest",
                    symbol: "circle",
                    symbolSize: 10,
                    label: {
                      normal: { show: false },
                      emphasis: { show: false }
                    }
                  }
                ]
              ]
            }
          }
        ]
      }
    };
  },
  created() {
    // this.setEchartOption()
  },
  mounted() {
    this.setEchartOption();
    this.myChart = echarts.init(document.getElementById("myChart"));
    this.myChart.setOption(this.echartsOption);
  },
  methods: {
    setEchartOption() {
      // data ：open，close，lowest，highest
      this.resData = splitData([
        ["2020/1/24", 12320.26, 12320.26, 12287.3, 12362.94],
        ["2020/1/25", 12300, 12291.3, 12288.26, 12308.38],
        ["2020/1/28", 12295.35, 12346.5, 12295.35, 12346.92],
        ["2020/1/29", 12347.22, 12358.98, 12337.35, 12363.8],
        ["2020/1/30", 12360.75, 12382.48, 12347.89, 12383.76],
        ["2020/1/31", 12383.43, 12385.42, 12371.23, 12391.82],
        ["2020/2/01", 12320.26, 12320.26, 12287.3, 12362.94]
      ]);
      this.echartsOption.xAxis.data = this.resData.categoryData;
      this.echartsOption.series[0].data = this.resData.values;
      console.log(this.echartsOption.xAxis.data);
      console.log(this.echartsOption.series[0].data);

      function splitData(rawData) {
        var categoryData = [];
        var values = [];
        for (var i = 0; i < rawData.length; i++) {
          categoryData.push(rawData[i].splice(0, 1)[0]);
          values.push(rawData[i]);
          console.log(categoryData);
          console.log(values);
        }
        return {
          categoryData: categoryData,
          values: values
        };
      }
    }
  }
};
</script>
<style scoped>
#myChart {
  width: 70%;
  height: 500px;
  margin: 0 auto;
}
</style>
