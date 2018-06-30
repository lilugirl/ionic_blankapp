# ionic_blankapp
npm init 
npm install mosca --save
touch index.js
```

Index.js
```
var mosca = require("mosca");

var server = new mosca.Server({
  http: {
    port: 3000,
    bundle: true,
    static: './'
  }
});

server.on('ready', function(){
    console.log('mqtt server started');
});

server.on('published', function(packet, client){
    console.log('Published: ', packet.payload);
})

server.on('subscribed', function(topic, client){
     console.log('subscribed: ', topic);
});

server.on('unSubscribed', function(topic, client){
     console.log('unSubscribed: ', topic);
})

server.on('clientConnected', function(client){
    console.log('client connected: ', client.id);
});

server.on('clientDisConnected', function(client){
    console.log('client disConnected: ' + client.id + " userNumber:" + usermap.keys.length);
});
```

> mosca node客户端代码

```
var mqtt    = require('mqtt');
var client  = mqtt.connect('mqtt://127.0.0.1:1883');

client.on('connect', function () {
  client.subscribe('presence');
  client.publish('presence', 'Hello mqtt');
});

client.on('message', function (topic, message) {
  // message is Buffer
  console.log(message.toString());
  client.end();
});
```

> mosca html客户端代码

1、生成browserMqtt.js ，拷贝出来，放到和index.html目录下面
```
cd node_modules/mqtt
npm install .
 webpack mqtt.js ./browserMqtt.js --output-library mqtt
```
2、创建index.html
* 注意：html客户端的地址是ws://localhost:3000，这个ws开头表示是websocket的地址，端口号是3000，不是

```
<html>
  <head>
    <script src="./browserMqtt.js"></script>
  </head>
  <body>
    <script>
        console.log('hello 0 ');
      var client = mqtt.connect("ws://10.8.5.79:3000");
      console.log('hello 1');
      client.subscribe("mqtt/demo");
      console.log('hello 12');
      client.on("message", function(topic, payload) {
        alert([topic, payload].join(": "));
        client.end();
      });

      client.publish("mqtt/demo", "hello world!");
    </script>
  </body>
</html>
```

