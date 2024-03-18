# Treasury management

## Reference architecture

```plantuml
@startuml

scale max 870 width
skinparam actorStyle awesome

rectangle "Organization" <<Company>> {
    component "System" {
        [SDK] #orange
    }
    actor "Organization User" as OU
}

rectangle "Qenta ecosystem" {

    interface "HTTP" as HTTP
    HTTP -down- [API Gateway]
    
    rectangle "ProWallet ecosystem" {


        rectangle "User interface" {
            [ProWallet Console] #orange
        }

        component "Mass transfer engine" as engine {
            [Recipients service]
            [Batch service]
        }


    }

    rectangle "Qenta Payments" {
        [EMConnect]
        [Qenta CEE]
    }

    component "Core services" as core {
            [Pricing service] #orange
            [Notification service]
            [Payment service] #orange
            [Orders service] #orange
            [Transfer service] #orange
        }

    component "QoS" as BC <<Blockchain>> {
        [Wallets]
        [Contracts]
    }

}

rectangle "Global payment paywalls & Banks" {
    [PayPal]
    [Payoneer]
    [Local & Regional Banks]
}


OU .down.> [ProWallet Console] : Manage recipients \n & batches

[ProWallet Console] .left.> [HTTP] : Use
[SDK] .down.> [HTTP] : Create batch

[API Gateway] .down.> [Orders service] : forward

[Orders service] .left.> [Pricing service] : Use
[Transfer service] .left.> [Notification service] : Use

[Transfer service] .down.> [Wallets] : Use

[Payment service] <.down.> [EMConnect] : Use

[EMConnect] .down.> [Local & Regional Banks] : Integrates

@enduml
```

## Purchase orders

