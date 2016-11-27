# Usage

Currently only simple `textRequest` method is available through this SDK

It looks like:

```javascript

const client = new ApiAi.Client('YOUR_ACCESS_TOKEN');
let promise = client.textRequest(longTextRequest);

promise
    .then(handleResponse)
    .catch(heandleError);

function handleResponse(serverResponse) {
        console.log(serverResponse);
}
function heandleError(serverError) {
        console.log(serverError);
}
```

And old (ported from V1 SDK) stream client (you can stream audio from mic with it):

```javascript


const client = new ApiAi.Client({accessToken: "ACCESS_TOKEN"});
const streamClient = client.createStreamClient(); // in that case some default settings will be applied
streamClient.onInit = function () {
    console.log("> ON INIT use direct assignment property");
    streamClient.open();
};

// or using 'options': 

const streamClient = client.createStreamClient({
    onInit: () => {
                console.log("> ON INIT use direct assignment property");
                streamClient.open();
            }
});

streamClient.init();
streamClient.startListening();
streamClient.stopListening();
```

Or in "old" style:

```javascript

const SERVER_PROTO = 'wss';
const SERVER_DOMAIN = 'api-ws.api.ai';
const SERVER_PORT = '4435';
const ACCESS_TOKEN = 'YOUR_ACCESS_TOKEN';

const config = {
    server: SERVER_PROTO + '://' + SERVER_DOMAIN + ':' + SERVER_PORT + '/api/ws/query',
    token: ACCESS_TOKEN,// Use Client access token there (see agent keys).
    sessionId: '123',
    lang: 'en',
    onInit: function () {
        console.log("> ON INIT use config");
    }
};

const streamClient = new ApiAi.StreamClient(config);

streamClient.onInit = function () {
    console.log("> ON INIT use direct assignment property");
    streamClient.open();
};

streamClient.init();
...

streamClient.startListening();
streamClient.stopListening();
```

# Development

* Checkout from this repository, do not forget to switch to "v2" branch
* run `$ npm install`
* run `$ webpack -w` or just `$ webpack-dev-server` (as an option for non globally installed dev-server - `$ ./node_modules/.bin/webpack-dev-server`)
* develop! (webpack will automatically compile SDK to ./target/ApiAi.js file on each change, just include it into some test HTML file (./demo/index.html will probably do the job) and test it).

# Building


* `$ webpack` compiles target/ApiAi.js
* `$ webpack --env.target=commonjs2` compiles target/ApiAi.commonjs2.js
* `$ webpack --env.compress=true` compiles target/ApiAi.min.js 
* `$ webpack --env.compress=true --env.target=commonjs2` compiles target/ApiAi.commonjs2.min.js
* `$ tsc -p tsconfig.es6src.json` compiles srces6 dir

# Testing

`$ npm test`
