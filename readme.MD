<h1>I forked this and now Imma bork it.</h1>

<hr>

:comet: An example of using the [CppComet](https://github.com/CppComet/comet-server) server to create a chat. For more information about CppComet, see [documentation](http://comet-server.org)


# PHP chat example

[codepen.io online demo](https://codepen.io/Levhav/pen/vJWWqW)

![php chat demo](https://github.com/CppComet/php-chat-example/blob/master/chat.gif)

# Scheme of chat

Typical scheme of chat:

![scheme-of-chat](https://github.com/CppComet/php-chat-example/blob/master/scheme-of-chat.jpg)

* Connecting to the comet server by websockets
* Send ajax message for add new massage to chat
* Send message to CppComet
* CppComet send messages for all subscribers in pipe
* Add message to database (optional)
 
# Step 1. Connecting to the comet server

[CppComet](https://github.com/CppComet/comet-server) has cloud saas alternative that can be used for testing and demo access. In the following examples I will use demonstration access from https://comet-server.com for those who could not or were too lazy to [deploy the server on their VPS](https://github.com/CppComet/comet-server#building-from-source)

```Login: 15
Password:lPXBFPqNg3f661JcegBY0N0dPXqUBdHXqj2cHf04PZgLHxT6z55e20ozojvMRvB8
Host: app.comet-server.ru
```

To connect to the comet server from the JavaScript API, use the following command:

```
cometApi.start({node:"app.comet-server.ru", dev_id:15})
```

* in parameter node - set hostname of your own server
* parameter dev_id - use only if you use saas service comet-server.com


# Step 2. send message to server

* Send ajax query to php back-end
* Send CometQL query for comet server

[code of php back-end](https://github.com/CppComet/php-chat-example/blob/master/chat.php)

Connection code to CppComet using MySQL protocol:
```
$host = "app.comet-server.ru";
$user = "15";
$password = "lPXBFPqNg3f661JcegBY0N0dPXqUBdHXqj2cHf04PZgLHxT6z55e20ozojvMRvB8";
$comet = mysqli_connect($host, $user, $password, "CometQL_v1");
```


The code for sending a message to the pipe "simplechat" and event 'newMessage':
```
$query = "INSERT INTO pipes_messages (name, event, message)VALUES('simplechat', 'newMessage', '".$msg."')"; 
mysqli_query($comet, $query);
```


# Step 3. receive message from comet server

subscription Code to the pipe on comet server. This callback will be called when somebody send message into channel `simplechat`

```
    cometApi.subscription("simplechat.newMessage", function(event){
        $("#web_chat").append('<b>'+HtmlEncode(event.data.name)+'</b>')
        $("#web_chat").append('<pre>'+HtmlEncode(event.data.text)+'</pre>')
        $("#web_chat").append('<br>')
    })
```

# Links

* [JavaScript API](http://comet-server.org/doku.php/en:comet:javascript_api)
* [CometQL API](http://comet-server.org/doku.php/en:comet:cometql)
* [Online demo](https://jsfiddle.net/o35kvmn2/5/)
* [CppComet](https://github.com/CppComet/comet-server) 
