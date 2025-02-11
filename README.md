# Telia Company - Terraform Azure Landing Zones Vending

This repository contains Terraform code for managing Azure Landing Zones in the Telia Company organization, following the Azure Cloud Adoption Framework (CAF) principles. The Landing Zones are created for different environments including Production, Staging, and Development.

The deployment process is handled using GitHub Actions and the Azure Cloud Provider for Terraform.

## Adding Subscriptions

To add subscriptions to any of the management groups, a Pull Request needs to be created. This PR should add a new entry to the respective YAML file under `subscriptions.yml` in the relevant environment folder.

The YAML structure for adding a subscription is as follows:

```yaml
---
subscriptions:
  - name: <subscription-name>
    alias: <subscription-name>
    allow_billing: <true/false>
    management_group: <management-group>
    subscription_workload: <workload-type>
    group_assignment:
      Contributor: ['<AAD-group-name>']
    tags:
      CMDBAppName: <app-name>
      CMDBAppRole: <app-role>
      Name: <name>
      Environment: <env-type>
      WBS: <WBS-number>
      H2ID: <H2ID>
      BillingCode: <WBS-number>
      SAPAccount: <SAP-Account>
      Owner: <owner-email>
```

### WBS Number (Cost Center)

The WBS (Work Breakdown Structure) Number is a unique identifier for a project or a cost center in an organization. It is crucial for tracking the cost of resources and activities. The WBS number is used as a Billing Code and should be supplied during the subscription creation process.

### H2 System Information

H2 is the system used by Telia Company for handling identities. When creating a subscription, an H2ID should be supplied. This is the ID used within the H2 system for identification.

## Deploying Configuration and Changes

### Vending Machine Module version:  4.1.4

### Storage Account

We implemented the vending machine terraform pipeline to store the state file in the remote azure storage account for security reason. The state file are located in the below storage account container. 
#### Storage account name : tfstatevending
#### Container name : tfstate-vending
#### File name
1.	lz-vending-telia-development-terraform.tfstate – Development
2.	lz-vending-telia-staging-terraform.tfstate – Staging
3.	lz-vending-telia-production-terraform.tfstate – production


### Service Principal Details

The connection between the GitHub Action and Azure enabled through the service principal **'Umi-lz-vending-github-oidc - Prod - Hid100008930'**. The below set of access should be granted for the service principal to perform the vending machine subscription creation process.
Access Detail
1.	**Storage Blob Data Contributor** role access to the storage account.
2.	**Owner** role access over the root management group.
3.	**User Access Administrator** role over the root management group.
4.	**Subscription creator** role access over the root management group.
https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/assign-roles-azure-service-principals#assign-the-subscription-creator-role-to-the-service-principal 


After making changes to the Terraform configuration, GitHub Actions will be used to deploy these changes against the Azure tenant. This automated process ensures a streamlined and error-free deployment.

For more information on how to work with GitHub Actions, you may refer to the GitHub Actions Documentation.
