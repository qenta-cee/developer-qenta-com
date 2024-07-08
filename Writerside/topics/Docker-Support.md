# Docker support

**%product_name%** for non-Java based application, it possible to run a lightweight server that exposes all SDK methods as
a RESTful API with JSON payloads to use SDK methods using a standard HTTP connection.

```plantuml
@startuml
!include <C4/C4_Container>
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

title Qenta docker support
footer Qenta ProWallet

skinparam linetype ortho

Boundary(customerSystems, "Customer Systems") {
    System_Ext(customerApplication, "Customer Application", "A customer-facing application")
    System_Boundary(dockerBased, "Docker-based Runtime") {
        Container(qentaSDKServer, "Qenta SDK Server") {
            Component(qentaSDK, "Qenta SDK for Java", "Java SDK for Qenta")
        }
    }
}

Boundary(qentaSystems, "Qenta Systems") {
    Container(apiGateway, "API Gateway", "API Gateway for Qenta")
    System(qentaServices, "Qenta Services", "Qenta microservices ecosystem")
}

Rel(customerApplication, qentaSDKServer, "Consumes as HTTP REST API")
Rel(qentaSDK, apiGateway, "Consumes as HTTP REST API")
Rel(apiGateway, qentaServices, "Forwards to")
@enduml
```

## Running the docker image


<procedure title="Running server image" type="steps">
<tip>
<b>Before to start</b>
<p>Make sure you have a <code>docker</code> based runtime installed in your environment.</p>
</tip>
<step>
Pull the docker image from the Qenta repository.
<code-block ignore-vars="false" lang="shell">
docker pull %sdk_docker_image_uri%:%sdk_docker_image_tag%
</code-block>
</step>
<step>
<b>Optional: </b> If you want to use a shorter name for the image, you can tag it with a different name.
<code-block ignore-vars="false" lang="shell">
docker tag %sdk_docker_image_uri%:%sdk_docker_image_tag% qenta-sdk-server:latest
</code-block>
</step>
<step>
Once you have the image, you can run the server with the following command.
<code-block lang="shell" ignore-vars="false">
docker run -d -p 8080:8081 -e SDK_CONFIGURATION_SOURCE_TYPE=ENVIRONMENT_VARIABLE \
-e ENVIRONMENT_CONFIG_URL=https://hxklzo87t9.execute-api.us-east2.amazonaws.com/config/development \
-e QENTA_ACCESS_TOKEN=  your_access_token\
-e QENTA_ENVIRONMENT=SANDBOX \
-e QENTA_ORGANIZATION_ID= your_organization_id\
-e QENTA_PRIVATE_KEY= your_private_key\
-e QENTA_USER_ID= your_user_id\
qenta-sdk-server:latest
</code-block>
</step>
<note>
You can consult the <a href="SDK-and-tools.md">SDK and Tools topic</a> to get the configuration values from the %prowallet_name%.
</note>
</procedure>

## Using the server

To verify the server is running, you can perform a `GET` request to the server at port `8080` or the port you have configured set in the running command.

```shell
curl --location --request GET "http://localhost:8080/v3/api-docs" 
```

Once you verified the server is running, you can use the SDK methods using a standard HTTP connection.

Following is an example of how to create an order using the server.

```shell
curl --location --request POST "http://localhost:8080/qenta-sdk/orders" ^
--header "Content-Type: application/json" ^
--header "Accept: */*" ^
--header "Host: localhost:8080" ^
--header "Connection: keep-alive" ^
--data-raw "{ \"currency\" : \"EUR\", \"fiatAmount\" : 120.0, \"accountId\" : \"3213\"}"
```