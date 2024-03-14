# expo-server-sdk-go

Send push notifications to Expo apps using Go

## Installation

```
go get github.com/vanlcief/expo-sdk-go/push
```

## Usage

```go
package main

import (
    "fmt"
    "github.com/vanclief/expo-sdk-go/push"
)

func main() {
    // To check the token is valid
    pushToken, err := push.NewExponentPushToken("ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]")
    if err != nil {
        panic(err)
    }

    // Create a new Expo SDK client
    client := push.NewClient(nil)

    // Publish message
    responses, err := client.Publish(
        &push.PushMessage{
            To: []push.ExponentPushToken{pushToken},
            Body: "This is a test notification",
            Data: map[string]string{"withSome": "data"},
            Sound: "default",
            Title: "Notification Title",
            Priority: push.DefaultPriority,
        },
    )

    // Check errors
    if err != nil {
        panic(err)
    }

    // Validate responses
    for _, res := range responses {
        if res.ValidateResponse() != nil {
            fmt.Println(res.PushMessage.To, "failed")
        }
    }
}
```

## License

MIT
