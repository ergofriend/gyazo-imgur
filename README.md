# gyazo-imgur
Qiita article
https://qiita.com/ErgoFriend/items/acb61dda0121acbed531

```
yarn install
yarn serve
```

Add `.env.local` to project root

```
VUE_APP_GYAZO='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
VUE_APP_IMGUR='zzzzzzzzzzzz'
```

## Based API

```javascript
// https://api.imgur.com/3/image
// https://upload.gyazo.com/api/upload
const apiUrl = '';
// https://imgur.com/account/settings/apps
// https://gyazo.com/oauth/applications
const apiKey = '';

// Common
let data = new FormData();
let content = {
  method: 'POST',
  headers: {
    Accept: 'application/json'
  },
  body: data,
  mimeType: 'multipart/form-data'
};

// Gyazo
data.append('imagedata', files[0]);
data.append('access_token', apiKey);
// Imgur
data.append('image', files[0]);
content.headers.Authorization = 'Client-ID ' + apiKey; // Don't include gyazo api

fetch(apiUrl, content)
  .then(
    response => response.json() // if the response is a JSON object
  )
  .then(success => {
    console.log(success);
    console.log(success.url); // Gyazo API
    console.log(success.data.link); // Imgur API
  })
  .catch(
    error => console.log(error) // Handle the error response object
  );
```
