## AZ

These settings apply only when `--az` is specified on the command line.

For new Resource Provider. It is highly recommended to onboard Azure CLI extensions. There's no differences in terms of customer usage. 

``` yaml $(az) && $(target-mode) != 'core'
az:
    extensions: MarketplaceOrdering
    namespace: azure.mgmt.marketplaceordering
    package-name: azure-mgmt-marketplaceordering
az-output-folder: $(azure-cli-folder)/src/azure/cli/command_modules/marketplaceordering
python-sdk-output-folder: "$(az-output-folder)/azext_[[ServiceName]]/vendored_sdks/[[ServiceName]]"
# add additinal configuration here specific for Azure CLI
# refer to the faq.md for more details
```

This is for command modules that already in azure cli main repo. 
``` yaml $(az) && $(target-mode) == 'core'
az:
    extensions: marketplaceordering
    namespace: azure.mgmt.marketplaceordering
    package-name: azure-mgmt-marketplaceordering
az-output-folder: $(azure-cli-folder)/src/azure-cli/azure/cli/command_modules/marketplaceordering
python-sdk-output-folder: "$(az-output-folder)/vendored_sdks/marketplaceordering"

``` 

``` yaml
directive:
  - where:
      group: marketplaceordering marketplace-agreement
    set:
      group: term
  - where:
      command: term create
    set:
      command: term accept
  - where:
      command: term accept
    set:
      command-description: Accept marketplace terms.
  - where: 
      parameter-name: publisher-id 
    set:
      parameter-name: publisher
  - where: 
      parameter-name: offer-id 
    set:
      parameter-name: offer
  - where: 
      parameter-name: plan-id 
    set:
      parameter-name: plan

cli:
  cli-directive:
    - where:
        group: MarketplaceAgreements
        op: GetAgreement
      hidden: true
    - where:
        group: MarketplaceAgreements
        op: Sign
      hidden: true
    - where:
        group: MarketplaceAgreements
        op: Cancel
      hidden: true
    - where:
        group: MarketplaceAgreements
        op: List
      hidden: true
    - where:
        group: MarketplaceAgreements
        set:
          extensionMode: preview
```