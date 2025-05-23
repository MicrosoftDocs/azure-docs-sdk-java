---
title: Azure Resource Manager RecoveryServicesBackup client library for Java
keywords: Azure, java, SDK, API, azure-resourcemanager-recoveryservicesbackup, recoveryservicesbackup
ms.date: 04/17/2025
ms.topic: reference
ms.devlang: java
ms.service: recoveryservicesbackup
---
# Azure Resource Manager RecoveryServicesBackup client library for Java - version 1.6.0 


Azure Resource Manager RecoveryServicesBackup client library for Java.

This package contains Microsoft Azure SDK for RecoveryServicesBackup Management SDK. Open API 2.0 Specs for Azure RecoveryServices Backup service. Package tag package-2025-02-01. For documentation on how to use this package, please see [Azure Management Libraries for Java](https://aka.ms/azsdk/java/mgmt).

## We'd love to hear your feedback

We're always working on improving our products and the way we communicate with our users. So we'd love to learn what's working and how we can do better.

If you haven't already, please take a few minutes to [complete this short survey][survey] we have put together.

Thank you in advance for your collaboration. We really appreciate your time!

## Documentation

Various documentation is available to help you get started

- [API reference documentation][docs]

## Getting started

### Prerequisites

- [Java Development Kit (JDK)][jdk] with version 8 or above
- [Azure Subscription][azure_subscription]

### Adding the package to your product

[//]: # ({x-version-update-start;com.azure.resourcemanager:azure-resourcemanager-recoveryservicesbackup;current})
```xml
<dependency>
    <groupId>com.azure.resourcemanager</groupId>
    <artifactId>azure-resourcemanager-recoveryservicesbackup</artifactId>
    <version>1.6.0</version>
</dependency>
```
[//]: # ({x-version-update-end})

### Include the recommended packages

Azure Management Libraries require a `TokenCredential` implementation for authentication and an `HttpClient` implementation for HTTP client.

[Azure Identity][azure_identity] and [Azure Core Netty HTTP][azure_core_http_netty] packages provide the default implementation.

### Authentication

Microsoft Entra ID token authentication relies on the [credential class][azure_identity_credentials] from [Azure Identity][azure_identity] package.

Azure subscription ID can be configured via `AZURE_SUBSCRIPTION_ID` environment variable.

Assuming the use of the `DefaultAzureCredential` credential class, the client can be authenticated using the following code:

```java
AzureProfile profile = new AzureProfile(AzureCloud.AZURE_PUBLIC_CLOUD);
TokenCredential credential = new DefaultAzureCredentialBuilder()
    .authorityHost(profile.getEnvironment().getActiveDirectoryEndpoint())
    .build();
RecoveryServicesBackupManager manager = RecoveryServicesBackupManager
    .authenticate(credential, profile);
```

The sample code assumes global Azure. Please change the `AzureCloud.AZURE_PUBLIC_CLOUD` variable if otherwise.

See [Authentication][authenticate] for more options.

## Key concepts

See [API design][design] for general introduction on design and key concepts on Azure Management Libraries.

## Examples

```java
OffsetDateTime scheduleDateTime = OffsetDateTime.parse(OffsetDateTime.now(Clock.systemUTC())
    .withNano(0)
    .withMinute(0)
    .withSecond(0)
    .plusDays(1)
    .format(DateTimeFormatter.ISO_INSTANT));

List<SubProtectionPolicy> lstSubProtectionPolicy
    = Arrays
        .asList(
            new SubProtectionPolicy().withPolicyType(PolicyType.FULL)
                .withSchedulePolicy(
                    new SimpleSchedulePolicy().withScheduleRunFrequency(ScheduleRunType.WEEKLY)
                        .withScheduleRunDays(Arrays.asList(DayOfWeek.SUNDAY, DayOfWeek.TUESDAY))
                        .withScheduleRunTimes(Arrays.asList(scheduleDateTime)))
                .withRetentionPolicy(
                    new LongTermRetentionPolicy()
                        .withWeeklySchedule(new WeeklyRetentionSchedule()
                            .withDaysOfTheWeek(Arrays.asList(DayOfWeek.SUNDAY, DayOfWeek.TUESDAY))
                            .withRetentionTimes(Arrays.asList(scheduleDateTime))
                            .withRetentionDuration(new RetentionDuration().withCount(2)
                                .withDurationType(RetentionDurationType.WEEKS)))
                        .withMonthlySchedule(new MonthlyRetentionSchedule()
                            .withRetentionScheduleFormatType(RetentionScheduleFormat.WEEKLY)
                            .withRetentionScheduleWeekly(new WeeklyRetentionFormat()
                                .withDaysOfTheWeek(Arrays.asList(DayOfWeek.SUNDAY))
                                .withWeeksOfTheMonth(Arrays.asList(WeekOfMonth.SECOND)))
                            .withRetentionTimes(Arrays.asList(scheduleDateTime))
                            .withRetentionDuration(new RetentionDuration()
                                .withCount(1)
                                .withDurationType(RetentionDurationType.MONTHS)))
                        .withYearlySchedule(new YearlyRetentionSchedule()
                            .withRetentionScheduleFormatType(RetentionScheduleFormat.WEEKLY)
                            .withMonthsOfYear(
                                Arrays.asList(MonthOfYear.JANUARY, MonthOfYear.JUNE, MonthOfYear.DECEMBER))
                            .withRetentionScheduleWeekly(new WeeklyRetentionFormat()
                                .withDaysOfTheWeek(Arrays.asList(DayOfWeek.SUNDAY))
                                .withWeeksOfTheMonth(Arrays.asList(WeekOfMonth.LAST)))
                            .withRetentionTimes(Arrays.asList(scheduleDateTime))
                            .withRetentionDuration(new RetentionDuration().withCount(1)
                                .withDurationType(RetentionDurationType.YEARS)))),
            new SubProtectionPolicy().withPolicyType(PolicyType.DIFFERENTIAL)
                .withSchedulePolicy(
                    new SimpleSchedulePolicy().withScheduleRunFrequency(ScheduleRunType.WEEKLY)
                        .withScheduleRunDays(Arrays.asList(DayOfWeek.FRIDAY))
                        .withScheduleRunTimes(Arrays.asList(scheduleDateTime)))
                .withRetentionPolicy(new SimpleRetentionPolicy().withRetentionDuration(
                    new RetentionDuration().withCount(8).withDurationType(RetentionDurationType.DAYS))),
            new SubProtectionPolicy().withPolicyType(PolicyType.LOG)
                .withSchedulePolicy(new LogSchedulePolicy().withScheduleFrequencyInMins(60))
                .withRetentionPolicy(new SimpleRetentionPolicy().withRetentionDuration(
                    new RetentionDuration().withCount(7).withDurationType(RetentionDurationType.DAYS))));

vault = recoveryServicesManager.vaults()
    .define(vaultName)
    .withRegion(REGION)
    .withExistingResourceGroup(resourceGroupName)
    .withSku(new Sku().withName(SkuName.RS0).withTier("Standard"))
    .withProperties(new VaultProperties().withPublicNetworkAccess(PublicNetworkAccess.ENABLED)
        .withRestoreSettings(new RestoreSettings()
            .withCrossSubscriptionRestoreSettings(new CrossSubscriptionRestoreSettings()
                .withCrossSubscriptionRestoreState(CrossSubscriptionRestoreState.ENABLED))))
    .create();

protectionPolicyResource = recoveryServicesBackupManager.protectionPolicies()
    .define(policyName)
    .withRegion(REGION)
    .withExistingVault(vaultName, resourceGroupName)
    .withProperties(new AzureVmWorkloadProtectionPolicy().withWorkLoadType(WorkloadType.SQLDATA_BASE)
        .withSettings(new Settings().withTimeZone("Pacific Standard Time").withIssqlcompression(false))
        .withSubProtectionPolicy(lstSubProtectionPolicy))
    .create();
```
[Code snippets and samples](https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-recoveryservicesbackup_1.6.0/sdk/recoveryservicesbackup/azure-resourcemanager-recoveryservicesbackup/SAMPLE.md)


## Troubleshooting

## Next steps

## Contributing

For details on contributing to this repository, see the [contributing guide][cg].

This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit <https://cla.microsoft.com>.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repositories using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct][coc]. For more information see the [Code of Conduct FAQ][coc_faq] or contact <opencode@microsoft.com> with any additional questions or comments.

<!-- LINKS -->
[survey]: https://microsoft.qualtrics.com/jfe/form/SV_ehN0lIk2FKEBkwd?Q_CHL=DOCS
[docs]: https://azure.github.io/azure-sdk-for-java/
[jdk]: https://learn.microsoft.com/azure/developer/java/fundamentals/
[azure_subscription]: https://azure.microsoft.com/free/
[azure_identity]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-recoveryservicesbackup_1.6.0/sdk/identity/azure-identity
[azure_identity_credentials]: https://github.com/Azure/azure-sdk-for-java/tree/azure-resourcemanager-recoveryservicesbackup_1.6.0/sdk/identity/azure-identity#credentials
[azure_core_http_netty]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-recoveryservicesbackup_1.6.0/sdk/core/azure-core-http-netty
[authenticate]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-recoveryservicesbackup_1.6.0/sdk/resourcemanager/docs/AUTH.md
[design]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-recoveryservicesbackup_1.6.0/sdk/resourcemanager/docs/DESIGN.md
[cg]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-recoveryservicesbackup_1.6.0/CONTRIBUTING.md
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/

