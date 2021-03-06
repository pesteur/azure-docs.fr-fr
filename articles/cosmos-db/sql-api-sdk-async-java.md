---
title: 'Azure Cosmos DB : API, Kit de développement logiciel (SDK) et ressources Java Async SQL | Microsoft Docs'
description: Découvrez l’API et le Kit de développement logiciel (SDK) Java Async SQL, notamment les dates de lancement, les dates de suppression et les modifications apportées entre chaque version du Kit de développement logiciel (SDK) Java Async SQL d’Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
author: SnehaGunda
manager: kfile
ms.assetid: a452ffa2-c15d-4b0a-a8c1-ec9b750ce52b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 03/20/2018
ms.author: sngun
ms.openlocfilehash: 25a84c42430c76d296e12d3f83040fa18febdcb1
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Kit de développement logiciel (SDK) Java Async Azure Cosmos DB pour API SQL : notes de publication et ressources
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Flux de modification .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.JS](sql-api-sdk-node.md)
> * [Java asynchrone](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [API REST Resource Provider](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

Le Kit de développement logiciel (SDK) Java Async de l’API SQL est différent du Kit de développement logiciel (SDK) Java de l’API SQL en fournissant des opérations asynchrones avec prise en charge de la [bibliothèque Netty](http://netty.io/). Le [Kit de développement logiciel (SDK) Java de l’API SQL](sql-api-sdk-java.md) existant ne prend pas en charge les opérations asynchrones. 

<table>

<tr><td>**Téléchargement du Kit de développement logiciel (SDK)**</td><td>[Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)</td></tr>

<tr><td>**Documentation de l’API**</td><td>[Documentation de référence sur l’API Java](https://azure.github.io/azure-cosmosdb-java/)</td></tr>

<tr><td>**Contribution au Kit de développement logiciel (SDK)**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java)</td></tr>

<tr><td>**Prise en main**</td><td>[Prise en main du Kit de développement logiciel (SDK) Java Async](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)</td></tr>

<tr><td>**Code sample**</td><td>[Github](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)</td></tr>

<tr><td>**Conseils sur les performances**</td><td>[Fichier Readme de GitHub](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)</td></tr>

<tr><td>**Runtime minimal pris en charge**</td><td>[JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Notes de publication

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Ajout de la prise en charge de la contre-pression dans la requête.
* Ajout de la prise en charge de l’ID de plage de clés de partition dans la requête.
* Correction visant à autoriser un jeton de continuation plus grand dans l’en-tête de demande (correctif de bogue github n°24).
* Mise à niveau de la dépendance Netty vers 4.1.22.Final pour que la machine virtuelle Java s’arrête après la fin du thread principal.
* Correction visant à éviter la transmission d’un jeton de session durant la lecture de ressources principales.
* Ajout d’exemples.
* Ajout de scénarios de test.
* Correction de fichiers d’en-tête Java pour que les documents java soient correctement générés.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Kit de développement logiciel (SDK) GA avec prise en charge de bout en bout des E/S non bloquantes à l’aide de la [bibliothèque Netty](http://netty.io/) en mode passerelle. 

## <a name="release-and-retirement-dates"></a>Dates de lancement et de suppression
Microsoft fournira une notification au moins **12 mois** avant le retrait d’un Kit de développement logiciel (SDK) pour faciliter la transition vers une version plus récente/prise en charge.

Les nouvelles fonctionnalités et optimisations sont ajoutées uniquement au SDK actuel. Nous vous recommandons donc de toujours effectuer une mise à niveau vers la dernière version du SDK dès que possible.

Le service rejette toute requête envoyée à Cosmos DB à l’aide d’un Kit de développement logiciel (SDK) supprimé.

<br/>

| Version | Date de lancement | Date de suppression |
| --- | --- | --- |
| [1.0.1](#1.0.1) |20 avril 2018|--- |
| [1.0.0](#1.0.0) |27 février 2018|--- |

## <a name="faq"></a>Forum Aux Questions
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur Cosmos DB, consultez la page du service [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

