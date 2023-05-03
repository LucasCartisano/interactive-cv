<template>
  <div class="inner-container" :style="{width: width + 'px', height: height + 'px'}">
    <apexcharts 
      ref="chartComponent" 
      class="chart-component" 
      :class="{ 'padding-inner-container': horizontal }"
      :options="options" 
      :series="series">
    </apexcharts>
  </div>
</template>

<script>
import VueApexCharts from "vue3-apexcharts";

export default {
  name: 'MainChart',
  components: {
    apexcharts: VueApexCharts
  },

  props: {
    chartData: {
      type: Array,
      required: true,
    },
    width: {
      type: Number,
      required: true,
    },
    height: {
      type: Number,
      required: true,
    },
  },

  data() {
    const technologies = this.chartData.map(item => item.technology);
    const experience = this.chartData.map(item => item.realUseInYears);
    const horizontal = this.height > this.width ? true: false;

    return {
      options: {
        plotOptions: {
          bar: {
            horizontal: horizontal,
            dataLabels: {
              position: 'top',
              orientation: 'horizontal',
            }
          }
        },
        chart: {
          type: 'bar',
          id: 'experience-chart',
          background: '#fff',
          foreColor: '#333',
          offsetX: horizontal ? '-10': '0',
        },
        xaxis: {
          categories: technologies,
          position: 'bottom',
          labels: {
            show: true,
            rotate: -90,
            maxHeight: 165,
            style: {
              fontSize: '16px',
              fontFamily: 'Helvetica, Arial, sans-serif',
            },
          },
        },
        yaxis: {
          title: {
            text: 'Years',
            style: {
              fontSize: '16px',
              fontFamily: 'Montserrat, sans-serif'
            }
          }
        },
        dataLabels: {
          enabled: !horizontal,
          offsetY: 0,
        },
        colors: ['#3099ca']
      },
      series: [{
        name: 'Experience',
        data: experience
      }],
      horizontal: horizontal,
    }
  },

  mounted(){
    this.updateSize();
  },

  methods: {
    updateChart() {
      const technologies = this.chartData.map(item => item.technology);
      const experience = this.chartData.map(item => item.realUseInYears);

      this.series = [{
        name: 'Experience',
        data: experience
      }];

      this.$refs.chartComponent.updateOptions({ 
        xaxis: {categories: technologies},
      })
    },

    updateSize(){
      let newSize = '16px';
      if( this.width < 600 ){
        newSize = '12px';
      }else if(this.width < 1000 ){
        newSize = '14px';
      }

      const horizontal = this.height > this.width ? true: false;
      this.horizontal = horizontal;

      if( this.$refs.chartComponent ){
        setTimeout(() => {
          this.$refs.chartComponent.updateOptions({ 
            xaxis: {
              labels: {
                style: {
                  fontSize: newSize,
                },
                offsetY: horizontal ? -3: 0,
              }
            },
            yaxis: {
              title: {
                text: horizontal ? '': 'Years',
                style: {
                  fontSize: newSize,
                  fontFamily: 'Montserrat, sans-serif'
                }
              },
            },
            chart: {
              height: this.height,
              offsetX: horizontal ? '-10': '0'
            },
            plotOptions: { 
              bar: { 
                horizontal: horizontal 
              }
            },
            dataLabels: {
              enabled: !horizontal
            },
          })
        }, 10);
      }
    },
  },


  watch: {
    chartData: function () {
      this.updateChart();
    },
    width: function () {
      this.updateSize();
    },
    height: function () {
      this.updateSize();
    },
  }
}
</script>

<style scoped>
  .inner-container, .chart-component{
    min-height: 65vh;
  }
  .padding-inner-container{
    padding:1.2em 0 0;
  }
</style>