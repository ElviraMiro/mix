## Add exchange/queues to RabbitMQ

```yaml
exchange:
  name: peatio.notice
  type: topic

queue:
  peatio.notice.ticker
  peatio.notice.market
  peatio.notice.withdraw
  peatio.notice.deposit
  peatio.notice.deposit.address
  peatio.notice.account
```

All events publish on RabbitMQ exchange: ```peatio.notice```

1. Slanger message in channel ```private-{member.sn}```:

with events: ```'accounts' and 'members'```

- send to RabbitMQ common queue: ```peatio.notice.account``` with routing key ```{member.sn}```

with events: ```'deposits', 'withdraws', 'deposit_address'```

- send to RabbitMQ queues: ```peatio.notice.withdraw, peatio.notice.deposit, peatio.notice.deposit.address``` with routing key ```{member.sn}``` relatively

2. Slanger message in channel ```market_global``` with event ```'ticker'```

- send to RabbitMQ queue ```peatio.notice.ticker```

3. Slanger message in channel ```market_{market.id}_global``` with event ```'update' and 'trades'```

- send to RabbitMQ queue ```peatio.notice.market``` with routing key ```{market.id}```
