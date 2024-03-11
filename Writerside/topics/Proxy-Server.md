# Docker support

**%product_name%** for non-Java based application, it possible to run a lightweight proxy server that exposes all SDK methods as
a
RESTful API with JSON payloads to use SDK methods using a standard HTTP connection.

```plantuml
@startuml

scale max 800 width

package "External System"{
    node "VPC" {
        node "Non-Java based" {
            OutPort -left- [Backend]
        }
        node "Docker-based environment" {
            RestAPI -down- [ProxyServer] 
            component ProxyServer {
                [SDK] #orange
            }
        }
        OutPort .down.> RestAPI : POST \n http://localhost:8080
    }
}

package "Qenta" {
    HTTP - [Qenta Rest APIs]
    [API Gateway] .right.> HTTP
}

SDK .right.> [API Gateway]

@enduml

```

## Qenta SDK Server Manual Integration


## Deploying Qenta Proxy server with Docker on Windows Server 2016

> **BEFORE YOU START**
>
> Make sure you have Docker Installed.
>
{style="warning"}

<procedure title="Installation" id="installation">
    <step>
        <p>Pull the Docker image</p>
        <list>
        <li> <p>Execute the following command:</p>
            <code>docker pull public.ecr.aws/f5d3l7y2/qenta-sdk-server:&lt;image_tag&gt;\</code>
        </li>
        <li> <p>Use the following command to tag the image in Docker:</p>
            <code>docker tag public.ecr.aws/f5d3l7y2/qenta-sdk-server:&lt;image_tag&gt;\ qenta-sdk-server:latest</code>
        </li>
        </list>
    <list>
    </list>
    </step>
    <step>
        <p>Run the container:</p>
        <code-block>
            docker run -d -p 8080:8081
            -e SDK_CONFIGURATION_SOURCE_TYPE=ENVIRONMENT_VARIABLE \
            -e ENVIRONMENT_CONFIG_URL=https://hxklzo87t9.execute-api.us-east2.amazonaws.com/config/development \
            -e QENTA_ACCESS_TOKEN=  &lt;your_access_token&gt;\
            -e QENTA_ENVIRONMENT=dev \
            -e QENTA_ORGANIZATION_ID= &lt;your_organization_id&gt;\
            -e QENTA_PRIVATE_KEY= &lt;your_private_key&gt;\
            -e QENTA_USER_ID= &lt;your_user_id&gt;\
            qenta-sdk-server:latest
        </code-block>
    </step>
</procedure>

## Access the server

To verify the server is up and running, In the address bar, enter the URL where the Spring server is configured to run.
If not specified in your configuration, a common default URL is:

<a href="http://localhost:8080/swagger-ui.html">http://localhost:8080/swagger-ui.html</a>

Make sure to set the correct port number in case you used the --server.port=custom_port_number or docker run in
a different port.


## Confirming Server Availability

If the server is running and accessible, Swagger UI will load, displaying a user-friendly interface for exploring and
testing your API endpoints.
Navigate through the available API documentation to understand the available endpoints, request/response
formats, and any additional information provided.

![Swagger](swagger.png){ style="block" thumbnail="true"}

## Test the server

You can use curl to test the `REST API` endpoints of your Proxy Server. Here's an example using the provided curl
command:


```bash
curl --location --request POST "http://localhost:8080/qenta-sdk/orders" ^
--header "Content-Type: application/json" ^
--header "Accept: */*" ^
--header "Host: localhost:8080" ^
--header "Connection: keep-alive" ^
--data-raw "{ \"currency\" : \"EUR\", \"fiatAmount\" : 120.0, \"accountId\" : \"3213\"}"
```

<p>This curl command performs a POST request to the specified URL <code>(http://localhost:8080/qenta-sdk/orders)</code> with a
<code>JSON</code> payload</p>

<tabs>
    <tab title="Response Confirmation">
        After executing the curl command, you should receive an HTTP response. A successful request returns a 200 OK
status.
    </tab>
    <tab title="Example HTTP Response ">
        <p>Below is an example screenshot from a REST client showing a successful 200 OK HTTP response after executing the
        curl command.</p>
    </tab>
</tabs>

