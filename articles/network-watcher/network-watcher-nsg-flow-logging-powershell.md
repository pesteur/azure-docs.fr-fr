---
title: Gérer les journaux des flux de groupe de sécurité réseau avec Azure Network Watcher - PowerShell | Microsoft Docs
description: Cette page explique comment gérer les journaux des flux de groupe de sécurité réseau dans Azure Network Watcher avec PowerShell
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 2419bf981f613137dba02ba0b767edc491324d27
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2018
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>Configuration des journaux des flux de groupe de sécurité réseau avec PowerShell

> [!div class="op_single_selector"]
> - [Portail Azure](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [API REST](network-watcher-nsg-flow-logging-rest.md)

Les journaux des flux de groupe de sécurité réseau désignent une fonctionnalité de Network Watcher qui vous permet de visualiser des informations sur le trafic IP d’entrée et de sortie par le biais d’un groupe de sécurité réseau. Ces flux de journaux sont écrits au format json et affichent les flux entrants et sortants en fonction de la règle, la carte réseau à laquelle le flux s’applique, des informations à 5 tuples sur le flux (adresse IP source/de destination, port source/de destination, protocole), ainsi que l’autorisation ou le refus du trafic.

## <a name="register-insights-provider"></a>Inscription du fournisseur Insights

Pour que les journaux de flux puissent correctement fonctionner, le fournisseur **Microsoft.Insights** doit être inscrit. Si vous ne savez pas si le fournisseur **Microsoft.Insights** est inscrit, exécutez le script suivant.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Activer les journaux des flux de groupe de sécurité réseau

La commande d’activation des journaux de flux est illustrée dans l’exemple suivant :

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

Le compte de stockage que vous spécifiez ne peut pas avoir de règles réseau configurées qui limitent l’accès réseau pour les services Microsoft ou des réseaux virtuels spécifiques uniquement.

## <a name="disable-network-security-group-flow-logs"></a>Désactiver les journaux des flux de groupe de sécurité réseau

Utilisez l’exemple suivant pour désactiver les journaux de flux :

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Télécharger un journal de flux

L’emplacement de stockage d’un journal de flux est défini au moment de la création. L’Explorateur Stockage Microsoft Azure est un outil très pratique pour accéder à ces journaux de flux enregistrés dans un compte de stockage. Vous pouvez le télécharger ici : http://storageexplorer.com/

Si un compte de stockage est spécifié, les fichiers de capture de paquets sont enregistrés dans un compte de stockage à l’emplacement suivant :

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Pour plus d’informations sur la structure du journal, consultez [Network Security Group Flow log Overview (Présentation de la journalisation des flux de groupe de sécurité réseau)](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [visualiser vos journaux de flux de groupe de sécurité réseau avec Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md).

Découvrez comment [visualiser vos journaux de flux de groupe de sécurité réseau avec des outils open source](network-watcher-visualize-nsg-flow-logs-open-source-tools.md).
