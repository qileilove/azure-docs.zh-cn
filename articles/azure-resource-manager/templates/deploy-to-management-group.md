---
title: 将资源部署到管理组
description: 介绍如何通过 Azure 资源管理器模板在管理组范围部署资源。
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 084ab69f463334569d37efd9187bfe587bfc524d
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92668936"
---
# <a name="management-group-deployments-with-arm-templates"></a>具有 ARM 模板的管理组部署

随着组织的不断发展，可以部署 Azure 资源管理器模板（ARM 模板）来创建管理组级别的资源。 例如，你可能需要为管理组定义和分配[策略](../../governance/policy/overview.md)或 [Azure 基于角色的访问控制 (Azure RBAC)](../../role-based-access-control/overview.md)。 使用管理组级别的模板，可以声明方式在管理组级别应用策略和分配角色。

## <a name="supported-resources"></a>支持的资源

并非所有资源类型都可以部署到管理组级别。 本部分列出了支持的资源类型。

对于 Azure 蓝图，请使用：

* [项目](/azure/templates/microsoft.blueprint/blueprints/artifacts)
* [蓝图](/azure/templates/microsoft.blueprint/blueprints)
* [blueprintAssignments](/azure/templates/microsoft.blueprint/blueprintassignments)
* [versions](/azure/templates/microsoft.blueprint/blueprints/versions)

对于 Azure 策略，请使用：

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [remediations](/azure/templates/microsoft.policyinsights/remediations)

对于 azure RBAC)  (基于角色的访问控制，请使用：

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

对于部署到订阅或资源组的嵌套模板，请使用：

* [部署](/azure/templates/microsoft.resources/deployments)

若要管理资源，请使用：

* [标记](/azure/templates/microsoft.resources/tags)

## <a name="schema"></a>架构

用于管理组部署的架构不同于资源组部署的架构。

对于模板，请使用：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    ...
}
```

对于所有部署范围，参数文件的架构都相同。 对于参数文件，请使用：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>部署命令

若要部署到管理组，请使用管理组部署命令。

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

对于 Azure CLI，请使用 [az deployment mg create](/cli/azure/deployment/mg#az-deployment-mg-create)：

```azurecli-interactive
az deployment mg create \
  --name demoMGDeployment \
  --location WestUS \
  --management-group-id myMG \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

对于 Azure PowerShell，请使用 [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment)。

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -Name demoMGDeployment `
  -Location "West US" `
  -ManagementGroupId "myMG" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

---

有关部署 ARM 模板的部署命令和选项的更多详细信息，请参阅：

* [使用 ARM 模板和 Azure 门户部署资源](deploy-portal.md)
* [使用 ARM 模板和 Azure CLI 部署资源](deploy-cli.md)
* [使用 ARM 模板和 Azure PowerShell 部署资源](deploy-powershell.md)
* [利用 ARM 模板和 Azure 资源管理器部署资源 REST API](deploy-rest.md)
* [使用部署按钮从 GitHub 存储库部署模板](deploy-to-azure-button.md)
* [从 Cloud Shell 部署 ARM 模板](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>部署范围

部署到管理组时，可以将资源部署到：

* 操作的目标管理组
* 租户中的另一个管理组
* 管理组中的订阅
* 管理组中的资源组通过两个嵌套部署 () 
* [扩展资源](scope-extension-resources.md) 可以应用于资源

部署模板的用户必须有权访问指定的作用域。

本部分说明如何指定不同的范围。 可以在单个模板中组合这些不同的范围。

### <a name="scope-to-target-management-group"></a>作用域到目标管理组

将通过部署命令对管理组应用模板的资源部分中定义的资源。

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-mg.json" highlight="5":::

### <a name="scope-to-another-management-group"></a>作用域到另一个管理组

若要以另一个管理组为目标，请添加嵌套部署并指定 `scope` 属性。 将 `scope` 属性设置为 `Microsoft.Management/managementGroups/<mg-name>` 格式的值。

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/scope-mg.json" highlight="10,17,22":::

### <a name="scope-to-subscription"></a>作用域到订阅

你还可以将订阅作为管理组中的目标。 部署模板的用户必须有权访问指定的作用域。

若要以管理组中的订阅为目标，请使用嵌套部署和 `subscriptionId` 属性。

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-subscription.json" highlight="10,18":::

### <a name="scope-to-resource-group"></a>作用域到资源组

若要以该订阅中的资源组为目标，请添加两个嵌套部署。 第一个目标是具有资源组的订阅。 第二个目标是通过设置属性来以资源组为目标 `resourceGroup` 。

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-resource-group.json" highlight="10,21,25":::

若要使用管理组部署在订阅中创建资源组并将存储帐户部署到该资源组，请参阅[部署到订阅和资源组](#deploy-to-subscription-and-resource-group)。

## <a name="deployment-location-and-name"></a>部署位置和名称

对于管理组级别部署，必须为部署提供位置。 部署位置独立于部署的资源的位置。 部署位置指定何处存储部署数据。

可以为部署提供一个名称，也可以使用默认部署名称。 默认名称是模板文件的名称。 例如，部署一个名为 **azuredeploy.json** 的模板将创建默认部署名称 **azuredeploy** 。

每个部署名称的位置不可变。 当某个位置中已有某个部署时，无法在另一位置创建同名的部署。 如果出现错误代码 `InvalidDeploymentLocation`，请使用其他名称或使用与该名称的以前部署相同的位置。

## <a name="azure-policy"></a>Azure Policy

部署到管理组的自定义策略定义是管理组的扩展。 若要获取自定义策略定义的 ID，请使用 [extensionResourceId ( # B1 ](template-functions-resource.md#extensionresourceid) 函数。 内置策略定义是租户级别资源。 若要获取内置策略定义的 ID，请使用 [tenantResourceId](template-functions-resource.md#tenantresourceid) 函数。

下面的示例演示如何[定义](../../governance/policy/concepts/definition-structure.md)管理组级别策略，并对其进行分配。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMG": {
            "type": "string",
            "metadata": {
                "description": "Target Management Group"
            }
        },
        "allowedLocations": {
            "type": "array",
            "defaultValue": [
                "australiaeast",
                "australiasoutheast",
                "australiacentral"
            ],
            "metadata": {
                "description": "An array of the allowed locations, all other locations will be denied by the created policy."
            }
        }
    },
    "variables": {
        "mgScope": "[tenantResourceId('Microsoft.Management/managementGroups', parameters('targetMG'))]",
        "policyDefinition": "LocationRestriction"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "[variables('policyDefinition')]",
            "apiVersion": "2019-09-01",
            "properties": {
                "policyType": "Custom",
                "mode": "All",
                "parameters": {
                },
                "policyRule": {
                    "if": {
                        "not": {
                            "field": "location",
                            "in": "[parameters('allowedLocations')]"
                        }
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2019-09-01",
            "dependsOn": [
                "[variables('policyDefinition')]"
            ],
            "properties": {
                "scope": "[variables('mgScope')]",
                "policyDefinitionId": "[extensionResourceId(variables('mgScope'), 'Microsoft.Authorization/policyDefinitions', variables('policyDefinition'))]"
            }
        }
    ]
}
```

## <a name="deploy-to-subscription-and-resource-group"></a>部署到订阅和资源组

在管理组级别的部署中，可以以管理组中的订阅为目标。 以下示例在一个订阅中创建资源组，并向该资源组部署存储帐户。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nestedsubId": {
      "type": "string"
    },
    "nestedRG": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    "nestedLocation": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "nestedSub",
      "location": "[parameters('nestedLocation')]",
      "subscriptionId": "[parameters('nestedSubId')]",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
            {
              "type": "Microsoft.Resources/resourceGroups",
              "apiVersion": "2020-06-01",
              "name": "[parameters('nestedRG')]",
              "location": "[parameters('nestedLocation')]",
            },
            {
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2020-06-01",
              "name": "nestedSubRG",
              "resourceGroup": "[parameters('nestedRG')]",
              "dependsOn": [
                "[parameters('nestedRG')]"
              ],
              "properties": {
                "mode": "Incremental",
                "template": {
                  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                  "contentVersion": "1.0.0.0",
                  "resources": [
                    {
                      "type": "Microsoft.Storage/storageAccounts",
                      "apiVersion": "2019-04-01",
                      "name": "[parameters('storageAccountName')]",
                      "location": "[parameters('nestedLocation')]",
                      "sku": {
                        "name": "Standard_LRS"
                      }
                    }
                  ]
                }
              }
            }
          ]
        }
      }
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

* 若要了解如何分配角色，请参阅[使用 Azure 资源管理器模板添加 Azure 角色分配](../../role-based-access-control/role-assignments-template.md)。
* 若要通过示例来了解如何为 Azure 安全中心部署工作区设置，请参阅 [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json)。
* 还可以在[订阅级别](deploy-to-subscription.md)和[租户级别](deploy-to-tenant.md)部署模板。
