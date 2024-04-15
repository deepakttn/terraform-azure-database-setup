# terraform-azure-databases 

[![Lint Status](https://github.com/tothenew/terraform-aws-template/workflows/Lint/badge.svg)](https://github.com/tothenew/terraform-aws-template/actions)
[![LICENSE](https://img.shields.io/github/license/tothenew/terraform-aws-template)](https://github.com/tothenew/terraform-aws-template/blob/master/LICENSE)

This module sets up different types of databases in Azure, including 
- MYSQL flexible server 

## Providers

| Name | Version |
|------|---------|
| <a name="provider_Azure"></a> [azurerm](#provider\_azure) | >=3.0 |

## Prerequisites

Before you begin, ensure you have the following requirements met:

1. Install Terraform (>= 1.3.0). You can download the latest version of Terraform from the official website: [https://www.terraform.io/downloads.html](https://www.terraform.io/downloads.html)

2. Azure CLI installed and configured with appropriate access rights. You can install the Azure CLI from [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

3. A Database subnet needs to be created with appropriate SUBNET DELEGATION or NETWORK POLICY FOR PRIVATE ENDPOINTS . 


## Resources

| Name                                     | Description                                                            | Type              |
|------------------------------------------|------------------------------------------------------------------------|-------------------|
| azurerm_mysql_flexible_server | Azure MYSQL flexible server         | azurerm           |
| azurerm_mysql_flexible_database | Azure MYSQL flexible server  database       | azurerm           |
| azurerm_private_dns_zone                    | Azure private dns zone     | azurerm           |
| azurerm_private_dns_zone_virtual_network_link                    | Azure  Private DNS zone Virtual Network Links.     |   azurerm           | 


## data source Variables

| Module Name       | Variable Name                                 | Type   | Description                                                                                        |
|-------------------|-----------------------------------------------|--------|----------------------------------------------------------------------------------------------------|
| locals               | project_name_prefix                                       | string | The name of the project.                                                                           |
| data.azurerm_resource_group | resource_group_name                           | string | The name of your resource group that is already created .                                                       |
| data.azurerm_virtual_network  | vnet_name                                     | string | The Name of your existing virtual network  if you want to make database as private only .                                                     |
| data.azurerm_subnet  | subnet_name                                    | string | The Name of your database subnet where you want to create your database.                                                     |


## MySQL Flexible Server Configuration

| Variable Name                   | Description                                           | Example Value               |
| --------------------------------| ------------------------------------------------------| ----------------------------|
| create_mysql_fs                 | Set to `true` to create MySQL Flexible Server.       | `true`                      |
| mysql_fs_server_name            | Name for the MySQL Flexible Server.                   | `"my-mysql-server"`         |
| resource_group_name             | Name of the Azure resource group.                     | `"my-resource-group"`       |
| location                        | Azure region for deployment.                          | `"East US"`                 |
| is_public                       | if you want to make database server as public         |  'true' |
| vnet_name            |  Name of your existing virtual network if is_public is false to make database as private only  |  `"my-vnet"` |
| subnet_name         | Name of your database subnet where you want to create your database.  | `"database_subnet"` |
|  private_dns_zone_name  |      Name of the private DNS zone for the Database Server."                                            |`mysql.mysql.database.azure.com` |
|  dns_zone_virtual_network_link_name | Name of the virtual network link for the private DNS zone.                                 | `"vnet-private-zone-link"`  |
| administrator_login             | Administrator login for MySQL Server.                 | `"myadmin"`                 |
| administrator_password          | Administrator password for MySQL Server.             | `"MyP@ssw0rd123"`           |
| mysql_sku_name                  | SKU name for the MySQL Flexible Server.              | `"B_Standard_B1s"`               |
| backup_retention_days           | Number of days to retain backups.                    | `7`                         |
| create_mode                     | MySQL Server creation mode.                          | `"Default"`                 |
| geo_redundant_backup_enabled    | Enable geo-redundant backups.                        | `true`                      |
| zone                            | Azure Availability Zone for deployment.              | `2`                         |
| high_availability               | Map of high availability configuration.               | See variable definition below |
| maintenance_window              | Map of maintenance window configuration.             | See variable definition below |
| Storage                         | Map of Storage configuration                         | See variable definition below |
| mysql_databases                 | Map of database configuration                         | see variable definition below |


#### High Availability Configuration

| Variable Name                   | Description                                           | Example Value               |
| --------------------------------| ------------------------------------------------------| ----------------------------|
| mode                            | High availability mode for MySQL Flexible Server.     | `"SameZone"` or `"ZoneRedundant"` |
| standby_availability_zone       | Availability zone for the standby Flexible Server.   | `1`, `2`, or `3`            |

#### Maintenance Window Configuration

| Variable Name                   | Description                                           | Example Value               |
| --------------------------------| ------------------------------------------------------| ----------------------------|
| day                             | Day of the week for maintenance window.              | `1` (Monday) to `7` (Sunday) |
| start_hour                      | Start hour of the maintenance window.                 | `2`                          |
| start_minute                    | Start minute of the maintenance window.               | `30`                         |

#### Storage Configuration

| Variable Name                   | Description                                           | Example Value               |
| --------------------------------| ------------------------------------------------------| ----------------------------|
| auto_grow_enabled               | Enable auto-grow for storage.                        | `true`                      |
| iops                            | Input/Output Operations Per Second (IOPS) for storage. | `360`                     |
| size_gb                         | Size of storage in gigabytes.                           | `20`                        |
    
### MySQL Database Configuration

| Variable Name                   | Description                                           | Example Value               |
| --------------------------------| ------------------------------------------------------| ----------------------------|
| create_mysql_fs                 | Set to `true` to create MySQL Database.               | `true`                      |
| charset                         | Charset for the MySQL Database.                      | `"utf8mb4"`                 |
| collation                       | Collation for the MySQL Database.                    | `"utf8mb4_unicode_ci"`      |

Note: Please refer to the module documentation for additional optional variables and their descriptions.

[MySQL Flexible Server Documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mysql_flexible_server)


### Outputs

| Output Name   | Description                            |
|---------------|----------------------------------------|
| mysql_fqdn    | Fully Qualified Domain Name (FQDN) for the MySQL server.      |


## Note

Make sure to provide the necessary values for the input variables and adjust the module accordingly.

Please refer to the documentation of each individual resource for more details on their usage and specific input variables.


## Authors

Module managed by [TO THE NEW Pvt. Ltd.](https://github.com/tothenew)

## License

Apache 2 Licensed. See [LICENSE](https://github.com/tothenew/terraform-azure-databases/blob/main/LICENSE) for full details.