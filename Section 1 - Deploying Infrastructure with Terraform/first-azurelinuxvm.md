### Documentation Referred:

## https://registry.terraform.io/

## https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/linux_virtual_machine

### Azure Linux VM

 ```sh

terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.85.0"
    }
  }
}

provider "azurerm" {
  #skip_provider_registration = true # This is only required when the User, Service Principal, or Identity running Terraform lacks the permissions to register Azure Resource Providers.
  features {}
}

# Import block to import existing resources which are not managed by terraform
import{
  id = "/subscriptions/050c5b91-875c-4c0e-a193-1c57c0d2d61f/resourceGroups/RG7DEC"
  to = azurerm_resource_group.linuxvmrg
}
import {
  id ="/subscriptions/050c5b91-875c-4c0e-a193-1c57c0d2d61f/resourceGroups/RG7DEC/providers/Microsoft.Network/virtualNetworks/linuxvm-vnet"
  to = azurerm_virtual_network.linuxvmvnet
}

# Resource Group
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

# Azure Virtual Network
resource "azurerm_virtual_network" "linuxvmvnet" {
  name                = "linuxvm-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.linuxvmrg.location
  resource_group_name = azurerm_resource_group.linuxvmrg.name
}
# Azure Subnet
resource "azurerm_subnet" "linuxvmsubnet" {
  name                 = "internal"
  resource_group_name  = azurerm_resource_group.linuxvmrg.name
  virtual_network_name = azurerm_virtual_network.linuxvmvnet.name
  address_prefixes     = ["10.0.2.0/24"]
}
# Azure Network Interface
resource "azurerm_network_interface" "linuxvmvnetnetworkinterface" {
  name                = "linuxvm-nic"
  location            = azurerm_resource_group.linuxvmrg.location
  resource_group_name = azurerm_resource_group.linuxvmrg.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.linuxvmsubnet.id
    private_ip_address_allocation = "Dynamic"
  }
}
# Azure VM ubuntu 
resource "azurerm_linux_virtual_machine" "linuxvmvm" {
  name                = "linuxvm"
  resource_group_name = azurerm_resource_group.linuxvmrg.name
  location            = azurerm_resource_group.linuxvmrg.location
  size                = "Standard_F2"
  admin_username      = "adminuser"
  network_interface_ids = [
    azurerm_network_interface.linuxvmvnetnetworkinterface.id,
  ]

  admin_ssh_key {
    username   = "adminuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "0001-com-ubuntu-server-jammy"
    sku       = "22_04-lts"
    version   = "latest"
  }
  tags = {
    Name = "My First VM"
  }
}

```

