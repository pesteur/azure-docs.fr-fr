---
title: Diffuser en continu le journal d’activité Azure sur les Event Hubs | Microsoft Docs
description: Apprenez comment diffuser en continu le journal d’activité Azure sur les Event Hubs.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2018
ms.author: johnkem
ms.openlocfilehash: 8a599558fc35ca2bf48ce2a5f11ec4978bf10277
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Diffuser en continu le journal des activités Azure sur les Event Hubs
Vous pouvez diffuser en continu le [journal d’activité Azure](monitoring-overview-activity-logs.md) en temps quasi réel vers n’importe quelle application :

* À l’aide de la fonction **Exportation** intégrée dans le portail
* En activant l’ID de règle Azure Service Bus dans un profil de journal via les applets de commande Azure PowerShell ou CLI d’Azure

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Ce que vous pouvez faire avec le journal d’activité et Event Hubs
Voici deux façons d’utiliser la fonctionnalité de diffusion en continu pour le journal d’activité :

* **Diffuser en continu sur des systèmes de journalisation et de télémétrie tiers** : au fil du temps, la diffusion en continu sur Azure Event Hubs deviendra le mécanisme de diffusion de votre journal d’activité vers les SIEM et les solutions d’analyse de journaux tiers.
* **Créer une plateforme de journalisation et de télémétrie personnalisée** : si vous disposez déjà d’une plate-forme de télémétrie personnalisée, ou si vous envisagez d’en créer une, la nature hautement évolutive de publication et d’abonnement d’Event Hubs vous permet d’intégrer avec souplesse le journal d’activité. Pour plus d’informations, regardez la [vidéo de Dan Rosanova sur l’utilisation d’Event Hubs dans une plateforme de télémétrie à l’échelle mondiale](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Activer la diffusion en continu du journal d’activité
Vous pouvez activer la diffusion en continu du journal d’activité soit par programmation, soit via le portail. Dans tous les cas, vous pouvez choisir un espace de noms Event Hubs et une stratégie d’accès partagé pour cet espace de noms. Un Hub d’événements appelé insights-logs-operationallogs est créé dans cet espace de noms lorsque le premier nouvel événement de journal d’activité se produit. 

Si vous n’avez pas d’espace de noms Event Hubs, vous devez d’abord en créer un. Si vous avez précédemment diffusé en continu des événements du journal d’activité vers cet espace de noms Event Hubs, le Hub d’événements créé précédemment sera réutilisé. 

La stratégie d’accès partagé définit les autorisations dont dispose le mécanisme de diffusion en continu. À l’heure actuelle, la diffusion vers Event Hubs requiert des autorisations de **gestion**, d’**envoi** et d’**écoute**. Vous pouvez créer ou modifier des stratégies d’accès partagé de l’espace de noms Event Hubs dans le portail Azure, sous l’onglet **Configurer** de votre espace de noms Event Hubs. 

Pour mettre à jour le profil de journal d’activité afin d’inclure la diffusion en continu, l’utilisateur qui apporte la modification doit avoir l’autorisation ListKey sur la règle d’autorisation Event Hubs. Il n’est pas nécessaire que l’espace de noms Event Hubs se trouve dans le même abonnement que l’abonnement qui génère des journaux, à condition que l’utilisateur configurant le paramètre ait un accès RBAC approprié aux deux abonnements.

### <a name="via-the-azure-portal"></a>Via le portail azure
1. Accédez au panneau **Journal d’activité** à l’aide de la recherche **Tous les services** sur le côté gauche du portail.
   
   ![Sélection du journal d’activité dans la liste des services du portail](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Sélectionnez le bouton **Exporter** en haut du journal.
   
   ![Bouton Exporter dans le portail](./media/monitoring-stream-activity-logs-event-hubs/export.png)

   Notez que les paramètres de filtre que vous aviez appliqué lors de l’affichage du journal d’activité dans la vue précédente n’ont aucun impact sur vos paramètres d’exportation. Ils concernent uniquement le filtrage que vous voyez lorsque vous parcourez votre journal d’activité dans le portail.
3. Dans la section qui s’affiche, sélectionnez **Toutes les régions**. Ne sélectionnez pas de régions particulières.
   
   ![Section Exportation](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)

   > [!WARNING]  
   > Si vous sélectionnez autre chose que **Toutes les régions**, vous manquerez des événements clés que vous attendez de recevoir. Le journal d’activité étant un journal global (non régional), aucune région n’est associée à la plupart des événements. 
   >

4. Sélectionnez **Enregistrer** pour enregistrer ces paramètres. Les paramètres sont appliqués immédiatement à votre abonnement.
5. Si vous avez plusieurs abonnements, répétez cette action et envoyez toutes les données au même Hub d’événements.

### <a name="via-powershell-cmdlets"></a>Via les applets de commande PowerShell
Si un profil de journal existe déjà, vous devez tout d’abord le supprimer, puis créer un nouveau profil de journal.

1. Utilisez `Get-AzureRmLogProfile` pour déterminer s’il existe un profil de journal.  S’il n’existe aucun profil de journal, recherchez la propriété *name*.
2. Utilisez `Remove-AzureRmLogProfile` pour supprimer le profil de journal à l’aide de la valeur provenant de la propriété *name*.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzureRmLogProfile -Name "default"
    ```
3. Utilisez `Add-AzureRmLogProfile` pour créer un profil de journal :

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespaceName/authorizationrules/RootManageSharedAccessKey"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```

### <a name="via-azure-cli"></a>Via l’interface de ligne de commande Azure
Si un profil de journal existe déjà, vous devez tout d’abord le supprimer, puis créer un nouveau profil de journal.

1. Utilisez `az monitor log-profiles list` pour déterminer s’il existe un profil de journal.
2. Utilisez `az monitor log-profiles delete --name "<log profile name>` pour supprimer le profil de journal à l’aide de la valeur provenant de la propriété *name*.
3. Utilisez `az monitor log-profiles create` pour créer un profil de journal :

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

## <a name="consume-the-log-data-from-event-hubs"></a>Utilisez les données de journal d’Event Hubs
Le schéma pour le journal d’activité est disponible dans [Surveiller l’activité d’abonnement avec le journal d’activité Azure](monitoring-overview-activity-logs.md). Chaque événement est dans un tableau d’objets blob JSON appelé *enregistrements*.

## <a name="next-steps"></a>Étapes suivantes
* [Archiver le journal d’activité dans un compte de stockage](monitoring-archive-activity-log.md)
* [Lire la présentation du journal d’activité Azure](monitoring-overview-activity-logs.md)
* [Définir une alerte basée sur un événement de journal d’activité](insights-auditlog-to-webhook-email.md)

