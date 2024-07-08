# Treasury management

## Reference architecture

```plantuml
@startuml
!include <C4/C4_Container>
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

scale max 870 width
skinparam linetype ortho
skinparam actorStyle awesome

title Treasury Management Reference Architecture
footer Qenta ProWallet

Person(OU, "Organization User", "User managing recipients & batches")
System_Boundary(org, "Organization") {
    Container(sdk, "SDK", "System", "Software Development Kit for integration")
}

System_Boundary(qentaEcosystem, "Qenta Ecosystem") {
    Container(apiGateway, "API Gateway", "Interface", "HTTP API Gateway")

    Container_Boundary(proWalletEcosystem, "ProWallet Ecosystem") {
        Container(proWalletConsole, "ProWallet Console", "Web application", "User interface for managing mass transfers")
    }

    Container_Boundary(coreServices, "Core Services") {
        Component(pricingService, "Pricing Service", "Microservice", "Handles pricing logic")
        Component(notificationService, "Notification Service", "Microservice", "Manages notifications")
        Component(paymentService, "Payment Service", "Microservice", "Processes payments")
        Component(ordersService, "Orders Service", "Microservice", "Manages orders")
        Component(transferService, "Transfer Service", "Microservice", "Handles transfers")
    }

    Container_Boundary(qentaPayments, "Qenta Payments") {
        Component(emConnect, "EM Connect", "Microservice", "Integrates with banks and payment services")
        Component(qentaCEE, "Qenta CEE", "Microservice", "Central and Eastern Europe payment processing")
    }

    Container_Boundary(qos, "QoS", "Blockchain") {
        Component(wallets, "Wallets", "Solidity", "Manages blockchain wallets")
        Component(contracts, "Contracts", "Solidity", "Smart contracts for operations")
    }
}

System_Ext(paypal, "Paypal", "Global payment service")
System_Ext(payoneer, "Payoneer", "Global payment service")
System_Ext(banks, "Local & Regional Banks", "Financial institutions")

Rel(OU, proWalletConsole, "Uses")
Rel(sdk, apiGateway, "Interacts via")
Rel(proWalletConsole, apiGateway, "Interacts via")

Rel(apiGateway, ordersService, "Forwards requests to")
Rel(ordersService, pricingService, "Utilizes")
Rel(transferService, notificationService, "Utilizes")
Rel(transferService, wallets, "Utilizes")
Rel(paymentService, emConnect, "Utilizes")

Rel(emConnect, banks, "Integrates with")
Rel(emConnect, paypal, "Integrates with")
Rel(emConnect, payoneer, "Integrates with")

@enduml
```


