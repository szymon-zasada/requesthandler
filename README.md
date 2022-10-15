# Request handler
This is a simple tool to make life easier while working with [Unity Netcode Custom Messages](https://docs-multiplayer.unity3d.com/netcode/current/advanced-topics/message-system/custom-messages/index.html)
### Code Example - Online Chat System

```csharp
public struct ChatMessage : INetworkSerializable // creating struct for transfering data
{
    public string Owner;
    public string Message;
    
    public void NetworkSerialize<T>(BufferSerializer<T> serializer) where T : IReaderWriter
    {
        serializer.SerializeValue(ref Owner);
        serializer.SerializeValue(ref Message);
    }
}
```

```csharp
public struct ChatManager : NetworkBehaviour
{
    RequestHandler<ChatMessage> chatMessageHandler = new RequestHandler<ChatMessage>();
    
    public override void OnNetworkSpawn()
    {
        chatMessageHandler.OnRequestReceived += CreateChatMessage;
    }
    
    void CreateChatMessage(ChatMessage message)
    {
        Debug.Log($"{message.Owner}: {message.Message}");
    }
    
    public void SendChatMessage(string message)
    {
        chatMessageHandler.SendToServer(new ChatMessage
        {
            Owner = "PlayerX",
            Message = message
        });
    }
    
}
```
