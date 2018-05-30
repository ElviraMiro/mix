```mermaid
sequenceDiagram
    participant User
    participant Proxy
    participant AppLogic
    participant Peatio
    participant RabbitMQ
    participant PeatioDaemons
    participant Db
    participant Vault
    participant Pusher

    Peatio->>Db: save order
    PeatioDaemons->>Pusher: [trigger pusher_member / order]
    Peatio->>Db: lock funds (change account balance)
    Db-->>Peatio: result
    opt success
        Peatio->>RabbitMQ: [in default channel publish action:submit order to the matching queue]
        opt daemons worker
            RabbitMQ-->>PeatioDaemons: [receive data from default channel]
            PeatioDaemons->>RabbitMQ: [in orderbook channel publish action:new marketID+sell/marketId+buy]
            RabbitMQ-->>PeatioDaemons: [receive data from orderbook channel]
            PeatioDaemons->>RabbitMQ: [in default channel to the matching new_trade, order_processor]
            PeatioDaemons-->>PeatioDaemons: matching
            PeatioDaemons->>Db: save trade, update orders
            PeatioDaemons->>Pusher: [trigger global.trade-market / trade]
        end
    end
```

HEADER

```javascript
const Pusher = require("pusher")
```

> - [ ] | fsdfsf   | dfsdfsf   | sdfsdfsdf  |
>   | -------- | --------- | ---------- |
>   | dsfsdf   | sdfsdf    | sdfsdfsdfs |
>   | sdfsdfsd | sdfsdfsdf | sdfsdfsdf  |
>   |          |           |            |
>
>   

