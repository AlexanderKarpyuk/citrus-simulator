## Concepts

The Citrus simulator project has the primary focus to provide messaging simulation as a standalone server. Once started the simulator
provides different endpoints (Http REST, JMS, SOAP Web Service, and so on) and waits for incoming requests. The time a request arrives at one of the endpoints
the simulator maps the request to a predefined [simulator scenario](scenarios.md) which is automatically executed immediately. 

The mapping is done by [extracting a mapping key](mapping-key-extractor.md) from the request data. This can be a special request header or a XPath
expression that is evaluated on the request payload.

The [simulator scenario](scenarios.md) is capable of handling the request message and will return a proper response message to the calling client. 
With different scenarios defined the simulator is able to respond to different requests accordingly.
 
Each scenario defines a very special simulation logic by using Citrus test actions that perform once the scenario is triggered by incoming requests.

This way each incoming request is processed and a proper response message is provided to the client. You can define default and fallback scenarios for
each simulator.

In addition to that the simulator provides a [user interface](user-interface.md) where executed scenarios state success or failure. 
Also you can trigger scenarios manually.

### Simulator application

The simulator is based on Spring boot. This means we have a main class that loads the Spring boot application.

```java
import com.consol.citrus.simulator.annotation.EnableRest;
import com.consol.citrus.simulator.annotation.SimulatorApplication;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@SimulatorApplication
@EnableRest
public class Simulator {

    public static void main(String[] args) {
        SpringApplication.run(Simulator.class, args);
    }
}
```

This class is the main entrance for all configuration and customization statements. First of all the class is annotated as **@SpringBootApplication**
and **@SimulatorApplication**. This enables auto configuration for the simulator application. In addition to that we enable different aspects of the simulator
by using further annotations provided. In the sample above we use the **@EnableRest** annotation for Http REST endpoint support.

There are multiple annotations available for different transport endpoints:

* **@EnableRest** Enables [Http REST support](rest-support.md)
* **@EnableWs** Enables [SOAP web services support](ws-support.md)
* **@EnableJms** Enables [JMS support](jms-support.md)
* **@EnableEndpointComponent** Enables generic [endpoint component support](endpoint-component-support.md)

### Properties

The simulator is capable of loading configuration from system properties and property files. There are several properties that you can use in order to customize the simulator behavior. These
properties are:
 
* **citrus.simulator.configuration.class** Java configuration class that is automatically loaded. (default is com.consol.citrus.simulator.SimulatorConfig)
* **citrus.simulator.template.path** Default path to message payload template files.
* **citrus.simulator.default.scenario** Default scenario name.
* **citrus.simulator.timeout** Timeout when waiting for inbound messages.
* **citrus.simulator.template.validation** Enable/disable schema validation.

You can set these properties as system properties or you can add a property file in following location:

* **META-INF/citrus-simulator.properties**

The simulator will automatically load these properties during startup.

### Spring bean configuration

Citrus works with the Spring framework and the simulator is a Spring boot application. Therefore the configuration is done by adding and overwriting Spring beans in
the application context. The simulator automatically loads Spring beans defined in following locations:

* **META-INF/citrus-simulator.xml** Xml Spring bean configuration file.
* **com.consol.citrus.simulator.SimulatorConfig** Java configuration class. You can customize this class by defining the property **citrus.simulator.configuration.class** 

All beans defined in there get automatically loaded to the simulator Spring application context.