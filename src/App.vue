<template>
  <div class="app-container">
    <div class="header">
      <div class="branding-space">
        <h1 class="title"><span>Welcome to my</span> <span>interactive CV</span></h1>
        <h3 class="owner"><a href="https://www.linkedin.com/in/lucas-cartisano/" target="_blank">By Lucas Cartisano</a></h3>
      </div>
      <div class="menu-container">
        <div class="menu-header">
          <h4>Pick a career to see my related experience</h4>
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
    <Transition>
      <div class="contact" v-show="showContact">
        <a href="https://www.linkedin.com/in/lucas-cartisano/" target="_blank">Contact me on LinkedIn</a>
      </div>
    </Transition>
  </div>
</template>

<script>
import MainMenu from './components/MainMenu.vue';
import MainChart from './components/MainChart.vue';
import experience from "/experience.json";

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
      showContact: false,
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

    setTimeout(() => {
      this.showContact = true
    }, 3000)
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

<style>
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
  margin:0 !important;
  padding:0 !important;;
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
  padding-right:0.5em;
  flex-direction: column;
  justify-content: flex-start;
}

.menu-header h4{
  font-family: 'Montserrat', sans-serif;
  margin:0;
  padding:0.5em;
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
.branding-space{
  margin: 0.7em 0.7em 0.7em 1.8em;
  min-width:150px;
}

.title {
  font-family: 'Oswald', sans-serif;
  justify-self: start;
  padding-left:0;
  font-size: 2em;
  margin: 0;
  padding: 0;
}

.title span{
  white-space: nowrap;
}

.owner{
  font-size:1.2em;
  font-weight:bolder;
  color: #2b7efa;
  font-family: 'Oswald', sans-serif;
  margin: 0;
  padding: 0 0 0 0.2em;
}

.contact{
  font-size:1.2em;
  font-weight:bolder;
  background-color: #1e4681;
  color: #fff;
  margin:0;
  padding:0;
  font-family: 'Helvetica', sans-serif;
  text-align:right;
  display:inline-block;
  position:fixed;
  bottom:0;
  right:0;
}
.contact a{
  display:block;
  padding:0.4em 0.7em;
  margin:0;
}

.owner a, .contact a{
  color:inherit;
  font-size: inherit;
  font-weight: inherit;
  text-decoration:none;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 1s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

@media screen and (max-width: 1200px) {
  .title {
    font-size: 1.8em;
  }
  .contact{
    display:block;
    position:relative;
    bottom:auto;
    right:auto;
    text-align:center;
  }
  .contact a{
    padding:0.6em 1em;
  }
}

@media screen and (max-width: 1024px) {
  .title {
    font-size: 1.5em;
  }
  .page-container{
    padding: 0.3em 0;
  }
  .contact{
    font-size:1em;
  }
}

@media screen and (max-width: 900px) {
  .title {
    font-size: 1.3em;
  }
  .owner{
    font-size:1em;
  }
}

@media screen and (max-width: 750px) {
  .title {
    font-size: 1.1em;
  }
  .menu-header h4{
    font-weight:normal;
  }
}

@media screen and (max-width: 650px) {
  .owner{
    font-weight:normal;
  }
  .menu-header h4{
    font-size: 0.9em;
  }
}

@media screen and (max-width: 480px) {
  .branding-space{
    margin: 0.5em;
    min-width:150px;
  }
}
</style>


