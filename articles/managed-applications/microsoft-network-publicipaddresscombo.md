---
title: Élément d’interface utilisateur PublicIpAddressCombo Azure | Microsoft Docs
description: Décrit l’élément d’interface utilisateur Microsoft.Network.PublicIpAddressCombo pour le Portail Azure.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/30/2018
ms.author: tomfitz
ms.openlocfilehash: bf0ef5be609fba14ab12e1e6f9f97bc63f032aae
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Élément d’interface utilisateur Microsoft.Network.PublicIpAddressCombo
Groupe de contrôles pour la sélection d’une nouvelle adresse IP publique ou d’une adresse IP publique existante.

## <a name="ui-sample"></a>Exemple d’interface utilisateur
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Si l’utilisateur sélectionne « Aucune » pour l’adresse IP publique, la zone de texte d’étiquette de nom du domaine est masquée.
- Si l’utilisateur sélectionne une adresse IP publique existante, la zone de texte d’étiquette de nom du domaine est masquée. Sa valeur est l’étiquette de nom de domaine de l’adresse IP sélectionnée.
- Le suffixe du nom de domaine (par exemple, westus.cloudapp.azure.com) se met à jour automatiquement en fonction de l’emplacement sélectionné.

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false,
    "zone": 3
  },
  "visible": true
}
```

## <a name="remarks"></a>Remarques
- Si `constraints.required.domainNameLabel` est défini sur **true**, l’utilisateur doit fournir une étiquette de nom de domaine lors de la création d’une nouvelle adresse IP publique. Les adresses IP publiques existantes sans étiquette ne sont pas disponibles à la sélection.
- Si `options.hideNone` est défini sur **true**, l’option de sélection **Aucune** pour l’adresse IP publique est masquée. La valeur par défaut est **false**.
- Si `options.hideDomainNameLabel` est défini sur **true**, la zone de texte pour l’étiquette de nom de domaine est masquée. La valeur par défaut est **false**.
- Si `options.hideExisting` est défini sur true, l’utilisateur n’est pas en mesure de choisir d’adresse IP publique existante. La valeur par défaut est **false**.
- Pour `zone`, seules les adresses IP de la zone spécifiée et les adresses IP publiques résilientes dans la zone sont disponibles.

## <a name="sample-output"></a>Exemple de sortie
Si l’utilisateur ne sélectionne aucune adresse IP publique, la sortie doit être la suivante :
```json
{
  "newOrExistingOrNone": "none"
}
```

Si l’utilisateur sélectionne une nouvelle adresse IP publique ou une adresse IP publique existante, la sortie doit être la suivante :
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "newOrExistingOrNone": "new"
}
```
- Quand `options.hideNone` est défini sur **true**, `newOrExistingOrNone` peut uniquement avoir une valeur **new** ou **existing**.
- Quand `options.hideDomainNameLabel` est défini sur **true**, `domainNameLabel` n’est pas déclaré.

## <a name="next-steps"></a>Étapes suivantes
* Pour voir une présentation de la création de définitions d’interface utilisateur, consultez la page [Prise en main de CreateUiDefinition](create-uidefinition-overview.md).
* Pour obtenir une description des propriétés communes des éléments d’interface utilisateur, consultez la page [Éléments de CreateUiDefinition](create-uidefinition-elements.md).
