<template>
  <div class="app-container">
    <div class="header">
      <h1 class="title">Welcome to my <span>interactive CV</span></h1>
      <div class="menu-container">
        <div class="menu-header">
          <h4>Pick a subject to see my related experience</h4>
        </div>
        <div class="menu-items">
          <MainMenu :categories="categories" @pick-category="pickCategory" />
        </div>
      </div>
    </div>
    <div class="page-container">
      <div class="chart-container" ref="chartContainer">
        <MainChart :chartData="chartData" :width="chartContainerWidth" :height="chartContainerHeight" class="chart" />
      </div>
    </div>
  </div>
</template>

<script>
import MainMenu from './components/MainMenu.vue';
import MainChart from './components/MainChart.vue';
import experience from "/data.json";

const sortedExperience = experience.sort((a, b) => b.realUseInYears - a.realUseInYears);

export default {
  name: "App",
  components: {
    MainMenu,
    MainChart
  },

  data() {
    return {
      CHARTRATIO: 1.6,
      chartContainer: null,
      chartContainerWidth: 400,
      chartContainerHeight: 300,
      jsonData: sortedExperience,
      categories: Array.from(new Set(sortedExperience.flatMap(item => item.category))),
      chartData: sortedExperience,
      filter: 'all',
    };
  },

  mounted() {
    this.chartContainer = this.$refs.chartContainer;

    this.adjustChartSize();
    window.addEventListener('resize', this.adjustChartSize);
  },

  beforeUnmount() {
    window.removeEventListener('resize', this.adjustChartSize);
  },

  methods: {
    pickCategory(category) {
      this.filter = category
      if(this.filter === 'all' ){
        this.chartData = sortedExperience;
      }else{
        this.chartData = this.jsonData.filter(item => item.category.includes(this.filter));
      }
      // console.log(this.chartData)
    },

    adjustChartSize() {
      if (this.chartContainer) {
        this.chartContainerHeight = this.chartContainer.clientHeight;
        this.chartContainerWidth = Math.min(this.chartContainer.clientWidth, this.chartContainerHeight * this.CHARTRATIO);
        // console.log(this.chartContainerWidth, this.chartContainerHeight);
      }
    },
  },
}
</script>

<style scoped>
@font-face {
  font-family: 'Oswald';
  src: url('~@/assets/fonts/Oswald/Oswald-VariableFont_wght.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}

@font-face {
  font-family: 'Montserrat';
  src: url('~@/assets/fonts/Montserrat/Montserrat-VariableFont_wght.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}

html, body{
  margin:0;
  padding:0;
  font-family: 'Oswald', sans-serif;
}

.app-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  margin:0;
  padding:0;
}

.menu-container {
  display: flex;
  height: 8vh; /* 5% del viewport height */
  flex-direction: column;
  justify-content: flex-start;
}

.menu-header h4{
  font-family: 'Montserrat', sans-serif;
  margin:0;
  padding:0 0 0 0.5em;
}

.menu-items {
  padding: 0;
}

.page-container{
  flex: 1 1 auto;
  padding:1em;
}

.chart-container {
  height: 85vh;
  max-height: 85vh;
  max-width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}
.chart{
  width: 100%;
  height: 100%;
  aspect-ratio: 1.62;
}

.header {
  flex: 0 0 auto;
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: center;
  position: sticky;
  top: 0;
  left: 0;
  right: 0;
  background-color: #fff;
  z-index:999;
}
.title {
  font-family: 'Oswald', sans-serif;
  justify-self: start;
  padding-left:1.5em;
  font-size: 2em;
}

.title span{
  white-space: nowrap;
}

@media screen and (max-width: 1200px) {
  .title {
    font-size: 1.8em;
  }
}

@media screen and (max-width: 1024px) {
  .title {
    font-size: 1.5em;
  }
  .page-container{
    padding: 0.3em 0;
  }
}

@media screen and (max-width: 900px) {
  .title {
    font-size: 1.3em;
  }
}

</style>


