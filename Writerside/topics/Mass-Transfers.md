# Mass Transfers

```plantuml
@startuml

!theme _none_
left to right direction
scale max 800 width
skinparam packageStyle rect
skinparam actorStyle awesome

actor Recipient as R <<Recipient>>

rectangle "Organization" <<Company>> {
    actor OrgUser as OU
    rectangle System as S <<System>> {
        rectangle SDK as SDK
    }
}

rectangle "Qenta" {

    package "Qenta App" {
        usecase "Query balance" as UC1
        usecase "Query transactions" as UC2
        usecase "Cash out" as UC3
    }
    
    package "ProWallet Console" {
        usecase "Manage recipients" as UC4
        usecase "Manage payment batches" as UC5
        usecase "Approve & process batch" as UC6
    }
    
    package "Qenta API" {
        usecase "Manage recipients" as UC7
        usecase "Manage payment batches" as UC8
        usecase "Approve & process batch" as UC9
    }
}

R ..> UC1
R ..> UC2
R ..> UC3

UC7 <.. S
UC8 <.. S
UC9 <.. S

UC4 <.. OU
UC5 <.. OU
UC6 <.. OU

@enduml
```


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
        :Info notification;
    else (no)
        :Create recipient;
    endif
stop

@enduml
```

