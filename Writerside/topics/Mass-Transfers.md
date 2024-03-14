# Mass Transfers


## Reference architecture

```plantuml
@startuml

skinparam actorStyle awesome

rectangle "B2C" <<Person>> {
    actor "Recipient" as Recipient
}

rectangle "Organization" <<Company>> {
    component "System" {
        [SDK]
    }
    actor "Organization User" as OU
}

package "Qenta" {
    interface "HTTP" as HTTP
    HTTP -down- [API Gateway]
    
    rectangle "User interface" {
        [Qenta App]
        [ProWallet Console]
    }
    
    component "Mass Transfer Engine" as engine {
        [Recipients service]
        [Batch service]
        [Pricing service]
        [Notification service]
    }
    
    component "QoS" as BC <<Blockchain>> {
        [Wallets]
        [Contracts]
    }
    
}

OU .down.> [ProWallet Console] : Manage recipients \n & batches
Recipient .down.> [Qenta App] : Uses

[ProWallet Console] .left.> [HTTP] : Use
[SDK] .down.> [HTTP] : Create batch

[API Gateway] .down.> [Batch service] : forward
[Batch service] .left.> [Pricing service] : Use

[Batch service] .down.> [Wallets]

@enduml
```

## Managing Recipients

Before to include a recipient into a payment batch you need to onboard him. The recipient is the person that will receive the payment.

### Onboarding

```plantuml
@startuml

!theme _none_

|Organization|
start
    :Onboard recipient;
|Recipient|
    if (Already user?) then (yes)
        :Notify subscription;
    else (no)
        :Send invitation;
    endif
stop

@enduml
```

