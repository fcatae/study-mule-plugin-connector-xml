# Quick Start

Run the command line:

```
mvn archetype:generate -DarchetypeGroupId=org.mule.extensions -DarchetypeArtifactId=mule-extensions-xml-archetype
```

Fill out the parameters. For example:

```
Define value for property 'groupId' org.mule.extensions.smart.connector: : org.mule-examples
Define value for property 'artifactId' hello-smart-connector: : mulexml-plugin
Define value for property 'version' 1.0.0-SNAPSHOT: : 
Define value for property 'package' org.mule.extensions.smart.connector: : org.mule-examples.mulexml-plugin
Define value for property 'extensionName' Hello: : MuleXML Plugin
Confirm properties configuration:
groupId: org.mule-examples
artifactId: mulexml-plugin
version: 1.0.0-SNAPSHOT
package: org.mule-examples.mulexml-plugin
extensionName: MuleXML Plugin
```

The project contains:
1. Many references to MUnit
2. No repository configured

Solution:
1. Remove all references to MUnit (dependency and pluginManagement)
2. Configure a Plugin Repository

```xml
    <pluginRepositories>
        <pluginRepository>
            <id>mulesoft-releases</id>
            <name>mulesoft release repository</name>
            <layout>default</layout>
            <url>https://repository.mulesoft.org/releases/</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>
```

The module file must be named "module-" (automatically by the scaffold).


# Comparing to a standard Mule App

Differences:
- `<packaging>mule-extension</packaging>` instead of `<packaging>mule-application</packaging>`

- Project dependency: (scope provided)

```
<dependency>
    <groupId>org.mule.runtime</groupId>
    <artifactId>mule-module-extensions-xml-support</artifactId>
    <version>${mule.version}</version>
    <scope>provided</scope>
</dependency>
```

- Build plugin:

```
<plugin>
    <groupId>org.mule.runtime.plugins</groupId>
    <artifactId>mule-extensions-maven-plugin</artifactId>
    <version>${mule.extensions.maven.plugin.version}</version>
    <extensions>true</extensions>
</plugin>
```

# Configure Manual Tests #

Configure the parent:

```
    <parent>
        <groupId>org.mule.extensions</groupId>
        <artifactId>mule-modules-parent</artifactId>
        <version>1.1.3</version>
    </parent>
```

It will indiretly add reference to Functional Tests (org.mule.tests.mule-tests-functional).

Add the repository:

```
	<repositories>
        <repository>
            <id>mulesoft-releases</id>
            <name>MuleSoft Releases Repository</name>
            <url>https://repository.mulesoft.org/releases/</url>
            <layout>default</layout>
        </repository>
    </repositories>
```

# Test Flow #

Create a test flow:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<mule xmlns="http://www.mulesoft.org/schema/mule/core" xmlns:doc="http://www.mulesoft.org/schema/mule/documentation"
	xmlns:module-mulexml-plugin="http://www.mulesoft.org/schema/mule/module-mulexml-plugin" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.mulesoft.org/schema/mule/core http://www.mulesoft.org/schema/mule/core/current/mule.xsd
		http://www.mulesoft.org/schema/mule/module-mulexml-plugin http://www.mulesoft.org/schema/mule/module-mulexml-plugin/current/mule-module-mulexml-plugin.xsd">

	<flow name="testFlow">
		<module-mulexml-plugin:say-good-morning doc:name="Say good morning" />
	</flow>
</mule>
```

# Icon #

Create SVG icon at /icon/icon.svg


# Example: Modules and Operators

```xml
<?xml version="1.0" encoding="UTF-8"?>
<module name="Hello World"
        prefix="hello-world"
        doc:description="This module relies in runtime provided components"

        xmlns="http://www.mulesoft.org/schema/mule/module"
        xmlns:mule="http://www.mulesoft.org/schema/mule/core"
        xmlns:doc="http://www.mulesoft.org/schema/mule/documentation"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="
           http://www.mulesoft.org/schema/mule/module http://www.mulesoft.org/schema/mule/module/current/mule-module.xsd
           http://www.mulesoft.org/schema/mule/core http://www.mulesoft.org/schema/mule/core/current/mule.xsd">

    <property name="text" type="string" doc:description="Message" />
    
    <operation name="say" doc:description="Say hello world!">
        <body>
            <mule:set-payload value="#[vars.text default 'Hello World']"/>
        </body>
        <output type="string" doc:description="Greetings"/>
    </operation>

    <operation name="sum" doc:description="Sum numbers">
        <parameters>
            <parameter name="numA" type="number"/>
            <parameter name="numB" type="number"/>
        </parameters>
        <body>
            <mule:set-payload value="#[vars.numA+vars.numB]" />
        </body>
        <output type="number"/>
    </operation>

 </module>
```