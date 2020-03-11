<template>
  <div id="app">
    <div class="header-bar">
      <div class="header-bar__title">Brightness & Contrast Developer Test</div>
      <div class="header-bar__subtitle">01 Jun, 2018 – 31 Dec, 2019</div>
    </div>

    <div class="header">
      <div class="header__img"></div>
      <div class="header__avatar">
        <img src="./assets/IMG_0105.jpeg" />
      </div>
    </div>

    <FilterPage />
  </div>
</template>

<script>
import FilterPage from './components/FilterPage.vue'

export default {
  name: 'App',
  components: {
    FilterPage
  }
}
</script>

<style>
@font-face {
  font-family: 'Graphik';
  src:  url('./assets/fonts/Graphik-Medium.woff2') format('woff2'),
        url('./assets/fonts/Graphik-Medium.woff') format('woff');
  font-weight: 500;
  font-style: normal;
}

@font-face {
  font-family: 'Graphik';
  src:  url('./assets/fonts/Graphik-Regular.woff2') format('woff2'),
        url('./assets/fonts/Graphik-Regular.woff') format('woff');
  font-weight: 400;
  font-style: normal;
}

body {
  margin: 0px;
}

#app {
  font-family: Graphik, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.header-bar {
  height: 70px;
  background-color: #7344C0;
  color: #FFFFFF;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.header-bar__title {
  font-weight: 500;
  font-size: 15px;
  line-height: 25px;
}

.header-bar__subtitle {
  font-size: 13px;
  line-height: 20px;
}

.header {
  height: 195px;
  margin-bottom: 20px;
}

.header__img {
  height: 100%;
  width: auto;
  background-size: cover;
  background-image: linear-gradient( rgba(255, 255, 255, 0) 80%, rgba(102, 102, 102, 1) 100%) , url('./assets/beyond-the-valley.png');
  background-repeat: no-repeat;
  background-position: center;
}

.header__avatar {
  position: absolute;
  margin-top: -50px;
  width: 100%;
}

.header__avatar img {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 50%;
  border: 2px solid #FFFFFF;
}
</style>
