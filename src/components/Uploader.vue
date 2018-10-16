<template>
  <div class="api_container">
    <div style="height: 160px;">
      <img :src="api.logo" style="max-width: 300px;"/>
    </div>
    <br>
    <input v-show="!image_url" type="file" id="image_file" @change="onFileChange"  class="imgur" accept="image/*" required/>
    <button v-show="image_url" type="button" @click="removeImage">取り消す</button>
    <br>
    <p>URL: {{ image_url }}</p>
    <br>
    <div style="height: 300px;">
      <img :src="image_thum" id="image_thum" style="max-width: 300px;"/>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    api: {
      type: Object,
      logo: String,
      url: String,
      token: String,
      service: String
    }
  },
  data() {
    return {
      loading_gif:
        'https://cdn-images-1.medium.com/max/1600/1*9EBHIOzhE1XfMYoKz1JcsQ.gif',
      image_thum_sample:
        'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTaE2sVZUD0TFLL7ix68tjo2PG0cgxb_NFR65Thfr_VuC45yGfT',
      image_thum:
        'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTaE2sVZUD0TFLL7ix68tjo2PG0cgxb_NFR65Thfr_VuC45yGfT',
      image_url: null
    };
  },
  methods: {
    onFileChange: function(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length) {
        return;
      }

      const apiUrl = this.api.url;
      const apiKey = this.api.token;

      this.image_thum = this.loading_gif;
      this.image_url = ' アップロードしています...';
      var data = new FormData();
      if (this.api.service == 'gyazo') {
        console.debug('gyazo');
        data.append('imagedata', files[0]);
        data.append('access_token', apiKey);
      } else {
        console.debug('imgur');
        data.append('image', files[0]);
      }

      let content = {
        method: 'POST',
        headers: {
          //Authorization: 'Client-ID ' + apiKey,
          Accept: 'application/json'
        },
        body: data,
        mimeType: 'multipart/form-data'
      };
      if (this.api.service == 'imgur') {
        content.headers.Authorization = 'Client-ID ' + apiKey;
      }

      fetch(apiUrl, content)
        .then(
          response => response.json() // if the response is a JSON object
        )
        .then(
          success => {
            console.log(success);
            if (this.api.service == 'gyazo') {
              console.log(success.url);
              this.image_url = success.url;
            } else {
              console.log(success.data.link);
              this.image_url = success.data.link;
            }
            this.createImage(files[0]);
          } // Handle the success response object
        )
        .catch(
          error => console.log(error) // Handle the error response object
        );
    },
    createImage(file) {
      let reader = new FileReader();
      reader.onload = e => {
        this.image_thum = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    removeImage() {
      this.image_url = '';
      document.getElementById('image_file').value = '';
      this.image_thum = this.image_thum_sample;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
