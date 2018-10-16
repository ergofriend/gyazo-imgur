<template>
  <div id="app">
    <HelloWorld msg="Gyazo&Imgur API"/>
    <br>
    <br>
    <Uploader class="api" :api="gyazo"/>
    <div class="api bar" />
    <Uploader class="api" :api="imgur"/>
  </div>
</template>

<script>
import Uploader from './components/Uploader.vue';
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'app',
  components: {
    HelloWorld,
    Uploader
  },
  data() {
    return {
      imgur: {
        service: 'imgur',
        url: 'https://api.imgur.com/3/image',
        token: process.env.VUE_APP_IMGUR,
        logo:
          'data:image/jpeg;base64,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'
      },
      gyazo: {
        service: 'gyazo',
        url: 'https://upload.gyazo.com/api/upload',
        token: process.env.VUE_APP_GYAZO,
        logo: 'https://i.imgur.com/ylzMVIk.png'
      }
    };
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.api {
  display: inline-block;
}

.bar {
  width: 20px;
  background-color: #2c3e50;
}
</style>
