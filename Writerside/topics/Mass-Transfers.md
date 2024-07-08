# Mass Transfers

## Reference architecture

```plantuml
@startuml
!include <C4/C4_Container>
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

scale max 850 width
skinparam linetype ortho

title Prowallet container diagram
footer Qenta ProWallet

Boundary(customer, "Customer") {
    Person(user, "Customer user")
    System_Boundary(customerSystem, "Customer System"){
        Container(qentaSDK, "Qenta SDK", "Java based SDK")
    }
}

System_Boundary(qentaEcosystem, "Qenta Ecosystem") {
    Container_Boundary(proWalletConsole, "ProWallet user interface") {
        Container(proWalletWeb, "ProWallet Web", "Web application")
    }

    Container(apiGateway, "API Gateway")

    Container_Boundary(massiveTransactions, "Massive Transactions API") {
        Component(transactionBatches, "Transactions Batchs", "Java")
        Component(recipientsManagement, "Recipients", "Java")
    }

    Container_Boundary(coreService, "Core Microservices"){
        Component(jwt, "JWT", "Microservice")
        Component(pricing, "Pricing", "Microservice")
        Component(registration, "Registration", "Microservice")
        Component(payment, "Payment", "Microservice")
        Component(comm, "Comm", "Microservice")
        Component(pyWallet, "PyWallet", "Microservice")
    }

    Container_Boundary(qos, "QoS"){
        Component(wallets, "Wallets", "Solidity")
        Component(contracts, "Contracts", "Solidity")
    }

    Container_Boundary(qentaPayments, "Qenta Payments"){
        Component(qentaCEE, "Qenta CEE", "Microservice")
        Component(emConnect, "EM Connect", "Microservice")
    }

}

Boundary(thirdPartyPayWalls, "Global payment paywalls & Banks") {
    System_Ext(banks, "Local & Regional Banks")
    System_Ext(paypal, "Paypal")
    System_Ext(payoneer, "Payoneer")
}

Rel(user, proWalletWeb, "Uses")
Rel(proWalletWeb, apiGateway, "Calls")
Rel(qentaSDK, apiGateway, "Calls")

Rel(apiGateway, massiveTransactions, "Exposes")


Rel(apiGateway, coreService, "Exposes")

Rel(transactionBatches, pricing, "Uses")
Rel(transactionBatches, pyWallet, "Uses")
Rel(recipientsManagement, comm, "Uses")

Rel(pyWallet, wallets, "Uses")

Rel(payment, qentaCEE, "Integrates")

Rel(emConnect, banks, "Integrates")
Rel(emConnect, paypal, "Integrates")
Rel(emConnect, payoneer, "Integrates")

@enduml
```


