# Java SDK

 This toolkit offers streamlined methods designed for creating orders and seamless interaction with the %product_name% API. Our Java SDK facilitates the integration process and enhance the functionality of your Java based applications.

> **Requirements**
>
> - Java 11 or later
> - Maven 3.6.3 or later or Gradle 6.7 or later

{style="tip"}

## Installation

To install the Java SDK, you can use Maven. Add the following dependency to your `pom.xml` file:

```xml
<dependency>
    <groupId>com.qenta</groupId>
    <artifactId>qenta-sdk-java</artifactId>
    <!-- Check the latest version in the Maven repository -->
    <version>1.14</version>
</dependency>
```

or Gradle:

```groovy
implementation 'com.qenta:qenta-sdk-java:1.14'
```

## Configuration

To use the SDK, you need to initialize a `QentaClient` instance on your code. 

An instance of `QentaConfigurationProvider` is required to provide the necessary configuration parameters to the `QentaClient`.

### The configuration provider

The `QentaConfigurationProvider` interface provides the necessary configuration parameters to the `QentaClient`. You can implement this interface to provide the configuration parameters from environment variable or JSON configuration file.

<tabs>
<tab title="Environment variables">
You can use environment variables to provide the configuration parameters. The following example shows how to use environment variables to provide the configuration parameters.<br/>

```java
@Bean
public QentaConfigurationProvider defaultConfigurationProvider() {
    return new EnvironmentVariableConfigurationProvider();
}
```

You need to provide the following environment variables:

<table>
    <tr>
        <th>Environment variable</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><code>ENVIRONMENT_CONFIG_URL</code></td>
        <td>The URL where the SDK will take the configuration from</td>
    </tr>
    <tr>
        <td><code>QENTA_ENVIRONMENT</code></td>
        <td>Qenta environment identifier. Allowed values: <code>SANDBOX</code> and <code>PRODUCTION</code></td>
    </tr>
    <tr>
        <td><code>QENTA_USER_ID</code></td>
        <td>Unique identifier of the user</td>
    </tr>
    <tr>
        <td><code>QENTA_ORGANIZATION_ID</code></td>
        <td>Unique identifier of the organization</td>
    </tr>
    <tr>
        <td><code>QENTA_ACCESS_TOKEN</code></td>
        <td>Allow the SDK make request to the REST APIs</td>
    </tr>
    <tr>
        <td><code>QENTA_PRIVATE_KEY</code></td>
        <td>Allows a client side signature of transactions</td>
    </tr>
</table>

</tab>
<tab title="JSON configuration file">
You can use a JSON configuration file to provide the configuration parameters. The following example shows how to use a JSON configuration file to provide the configuration parameters.

```java
@Bean
public QentaConfigurationProvider configurationProvider() {
        return new JsonFileConfigurationProvider(Paths.get("src/main/resources/qenta_configuration.json"));
    }
```

</tab>
</tabs>


<note>
You can consult the <a href="SDK-and-tools.md">previous topic</a> to get the configuration JSON file from the %prowallet_name%.
</note>

## Usage

### Initialize the client

You can initialize the `QentaClient` instance using the `StandardQentaClient.Builder` class. The following example shows how to initialize the client in a Spring base project, using the `QentaConfigurationProvider` instance.

```java
@Bean
public QentaClient qentaClient(QentaConfigurationProvider configurationProvider) {
    return StandardQentaClient.builder()
        .withConfiguration(configurationProvider)
        .build();
}
```

Once you have the `QentaClient` instance, you can use it to create orders and interact with the %product_name% API.

```java
import com.qenta.sdk.QentaClient;
import com.qenta.sdk.model.OrderRequest;
import com.qenta.sdk.model.OrderResponse;

// ...

private final QentaClient qentaClient;

// ...

OrderRequest orderRequest = new OrderRequest(/* provide order details here */);
OrderResponse orderResponse = qentaClient.createOrder(orderRequest);

// You can now work with the order response
```

That's it! You've successfully integrated the Qenta Java SDK into your Spring Boot project. You can now create orders and interact with Qenta's services.