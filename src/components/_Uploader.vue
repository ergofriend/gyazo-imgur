<template>
  <div>
    <img :src="logo" />
    {{ logo }}
    <input v-show="!image_url" type="file" @change="onFileChange" :value="image_url" class="imgur" accept="image/*" required/>
    <button v-show="image_url" type="button" @click="removeImage">取り消す</button>
    <br>
    <div>
        logo: {{info.logo}}
        url: {{info.url}}
        token: {{token}}
    </div>
    <p>{{ image_url }}</p>
    <br>
    <img :src="image_thum" id="image_thum" height="50px"/>
  </div>
</template>

<script>
export default {
  props: {
    logo: String,
    url: String,
    token: String,
    info: JSON
  },
  data() {
    return {
      image_thum_sample:
        'data:image/png;base64,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',
      image_thum:
        'data:image/png;base64,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',
      image_url: null
    };
  },
  methods: {
    onFileChange: function(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length) {
        return;
      }

      const apiUrl = this.url;
      const apiKey = this.token;

      this.createImage(files[0]);
      var data = new FormData();
      data.append('image', files[0]);

      data.append('access_token', apiKey);

      fetch(apiUrl, {
        method: 'POST',
        headers: {
          Authorization: 'Client-ID ' + apiKey,
          Accept: 'application/json'
        },
        body: data,
        mimeType: 'multipart/form-data'
      })
        .then(
          response => response.json() // if the response is a JSON object
        )
        .then(
          success => console.log(success) // Handle the success response object
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
      this.image_thum = this.image_thum_sample;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
