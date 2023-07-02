---
title: Blazor WebSocket
date: 2023-07-02 12:00:00 +0800
category: Tutorial
tags: dotnet
image:
  path: /images/blazor-websocket.png
  width: 1280   # in pixels
  height: 720   # in pixels
---

# Blazor WebSocket
## About This Project
This Blazor WebAssembly and ASP.NET project is a chat web application that demonstrates the use of WebSockets. The core components of the project are located in 2 files: `\Client\WebSocketClientConnection.cs` (for the client-side) and `\Server\Controllers\WebSocketsController.cs` (for the server-side). This article will provide a comprehensive guide on implementing WebSocket into your own project using these files.

## Client-Side
In the client-side project, do this to initialize WebSocket client connection
``` c#
// file: \Client\Pages\Login.razor.cs
// BEGIN: initialize WebSocket client connection here
WebSocketClientConnection.NavigationManager = mNavigationManager;
WebSocketClientConnection.Username = mModel.Username;
// END: initialize WebSocket client connection here
```

After initialization, connect to the server and start listening to whether the server accepts or rejects the connection.
``` c#
// file: \Client\Pages\Login.razor.cs
// connect and listen
WebSocketClientConnection.AddListener(async x =>
{
    WebSocketClientConnection.ClearListeners();
    var pMessage = Message.FromString(x);
    if (pMessage.MessageType == Message.eType.LoginSuccess)
        mNavigationManager.NavigateTo("#");
    else
        mMessage = pMessage.Text;
    StateHasChanged(); // required to refresh the text of MudAlert
});
```

## Server-Side
On your server-side project, copy the `\Server\Controllers\WebSocketsController.cs` file and customize the `Get` method according to your requirements. 

The `ReceiveMessage` method requires some modification. Just insert your own code below the `todo` comment line.

The `SendMessage` method can be used as is and requires no modification.

## Client Side

Back to your client-side project. After the connection has been accepted, use the following code to listen to messages from the server.
``` c#
// file: \Client\Pages\Index.razor.cs
WebSocketClientConnection.AddListener(x =>
{
    ProcessMessage(x);
});
```
To send a message to the server, do the following.
``` c#
string pMessage = "your message here or JSON";
WebSocketClientConnection.SendStringAsync(pMessage);
```
-- The End --