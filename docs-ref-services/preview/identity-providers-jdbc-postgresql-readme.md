---
title: Azure identity JDBC PostgreSQL plugin library for Java
keywords: Azure, java, SDK, API, azure-identity-providers-jdbc-postgresql, jdbc
ms.date: 09/23/2022
ms.topic: reference
ms.devlang: java
ms.service: jdbc
---
# Azure identity JDBC PostgreSQL plugin library for Java - version 1.0.0-beta.1 


This package contains the jdbc authentication plugin to authenticate with Azure AD for Azure hosted PostgreSQL services.

## Getting started

### Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/pricing/purchase-options/azure-account?cid=msft_learn).
- [Java Development Kit (JDK)][jdk] with version 8 or above.
- [Apache Maven](https://maven.apache.org/download.cgi).

### Include the package

[//]: # ({x-version-update-start;com.azure:azure-identity-providers-jdbc-postgresql;current})
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity-providers-jdbc-postgresql</artifactId>
    <version>1.0.0-beta.1</version> <!-- {x-version-update;com.azure:azure-identity-providers-jdbc-postgresql;current} -->
</dependency>
```
[//]: # ({x-version-update-end})

## Key concepts

This picture shows how the jdbc authentication plugin for PostgreSQl authenticating with MSI.

![postgresql_design.png](img/postgresql_design.png)

## Examples
For documentation on how to use this package, please refer to [Quickstart:Use Java and JDBC with Azure Database for PostgreSQL](https://aka.ms/quickstart-jdbc-postgresql).

## Troubleshooting
If you encounter any bugs, please file issues via [Issues](https://github.com/Azure/azure-sdk-for-java/issues).

## Next steps

## Contributing

For details on contributing to this repository, see the [contributing guide](https://github.com/Azure/azure-sdk-for-java/blob/azure-identity-providers-jdbc-postgresql_1.0.0-beta.1/CONTRIBUTING.md).

1. Fork it
1. Create your feature branch (`git checkout -b my-new-feature`)
1. Commit your changes (`git commit -am 'Add some feature'`)
1. Push to the branch (`git push origin my-new-feature`)
1. Create new Pull Request


<!-- LINKS -->
[jdk]: /java/azure/jdk/

