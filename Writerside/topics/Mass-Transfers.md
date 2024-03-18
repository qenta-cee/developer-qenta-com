# Mass Transfers


## Reference architecture

```plantuml
@startuml

scale max 870 width
skinparam actorStyle awesome

rectangle "B2C" <<Person>> {
    actor "Recipient" as Recipient
}

rectangle "Organization" <<Company>> {
    component "System" {
        [SDK] #orange
    }
    actor "Organization User" as OU
}

rectangle "Qenta ecosystem" {

    [Qenta App]

    rectangle "ProWallet ecosystem" {

        interface "HTTP" as HTTP
        HTTP -down- [API Gateway]

        rectangle "User interface" {
            [ProWallet Console]
        }

        component "Mass transfer engine" as engine {
            [Recipients service] #orange
            [Batch service] #orange
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
            [Orders service]
            [Transfer service]
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
Recipient .down.> [Qenta App] : Uses

[ProWallet Console] .left.> [HTTP] : Use
[SDK] .down.> [HTTP] : Create batch

[API Gateway] .down.> [Batch service] : forward
[API Gateway] .down.> [Recipients service] : forward
[Batch service] .down.> [Pricing service] : Use
[Recipients service] .down.> [Notification service] : Use

[Transfer service] .down.> [Wallets] : Use

[Qenta App] .down.> [Payment service] : Pay

[Payment service] <.down.> [EMConnect] : Use

[EMConnect] .down.> [Local & Regional Banks] : Integrates

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

