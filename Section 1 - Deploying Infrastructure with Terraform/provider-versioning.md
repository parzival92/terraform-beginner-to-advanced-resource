
### Different Version Parameters used in video:
```sh
version    = "2.7"
version    = ">= 2.8"
version    = "<= 2.8"
version    = ">=2.10,<=2.30"
```

### Base Configuration - provider.versioning.tf

```sh
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
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
