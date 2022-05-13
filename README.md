# Sending HTTP Requests

[Vue - The Complete Guide, Section 12](https://www.udemy.com/course/vuejs-2-the-complete-guide/)

## How to (not) send http requests

Without `.prevent` modifier / `event.preventDefault()`, a http request will be sent when we submit form. The browser's default behavoir is to send a http request to server when a form is submitted.

Send http requests(either fetch data from server or send data to server) from code to server with `axios` or use browser build in methods `fetch`.

Both `axios` or `fetch` send behind the scenes http requests, which means we won't reload the page, our application won't be killed and restarted, instead the vue app continue running, and the requests is just sent behind the scenes to the backend.

If we want to manage data locally in vue app (that is, no backend, user data is temporary), we need to prevent default, like we did in previous courses.
If we want to handle the submitted data first in vue (eg. validate it) and then send them to a external server, we also need to prevent default, because the default behavor is to send requests to the same address that serving this page (vue application) rather than our backend server. Typically, the backend server that handles http request is not the same one that serves our Vue application (that is, one server for delivering pages to user, another server for handling user data). If they are the same server in some rare case, the browser default might be fine.

## Send a POST request to store data

The `fetch` version is

```js
fetch('https://vue-http-demo-85e9e.firebaseio.com/surveys.json', {
  method: 'POST', // send data to server to store
  headers: {
    'Content-Type': 'application/json', // data is sent in json format
  },
  // body's value is the data we want to send
  body: JSON.stringify({
    // turn js object into json format
    name: this.enteredName,
    rating: this.chosenRating,
  }),
});
```

The `axios` version is

```js
import axios from 'axios'; // at the start of your <script> tag, before you "export default ..."
...
axios.post('https://vue-http-demo-85e9e.firebaseio.com/surveys.json', {
  name: this.enteredName,
  rating: this.chosenRating,
});

```

## Get request & transform response data

```js
fetch(
  'https://send-http-request-vue-default-rtdb.firebaseio.com/learning-survey.json'
)
  .then((response) => {
    if (response.ok) {
      return response.json(); // turn json into js object
    }
  })
  .then((data) => {
    // handle data...
  });
```

当我们发出的 GET request 得到 response 后(promise is fulfilled)，执行第一个 then 里面的函数，然后返回的数据成为第二个 then 里函数的参数

## Handle technical/browser-side error

like the url is wrong
add cathch method to fetch

## Handle errors stemmed from server side

In this case, you do get a regular response, but the response simply tells you something went wrong(that is, status code is 400 instead of 200). This case would by default not be handled by the catch method. However, we can check if response is ok in then method, if not, we throw a new error, this error will automatically go to the catch method.

```js
fetch('...')
  .then((response) => {
    if (!response.ok) {
      throw new Error('Cannot save data.');
    }
  })
  .catch((error) => {
    console.log(error);
    this.error =
      error.message || 'Sorry, something went wrong. Please try again later.';
  });
```
