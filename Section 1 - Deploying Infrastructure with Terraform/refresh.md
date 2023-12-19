

```sh
# terraform refresh
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

provider "azurerm" {
  #skip_provider_registration = true # This is only required when the User, Service Principal, or Identity running Terraform lacks the permissions to register Azure Resource Providers.
  features {}
}

resource "azurerm_resource_group" "linuxvmrg" {
  location = "eastus"
  name     = "RG7DEC"
  tags     = {}
  timeouts {
    create = null
    delete = null
    read   = null
    update = null
  }
}

```