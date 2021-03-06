---
title: Distribution et nouvelle tentative de distribution avec Azure Event Grid
description: Décrit comment Azure Event Grid distribue des événements et gère les messages qui n’ont pas été distribués.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: tomfitz
ms.openlocfilehash: 8eb6717369b48289bd31dcd1972ce275bc550c77
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/18/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Distribution et nouvelle tentative de distribution de messages avec Azure Grid 

Cet article décrit comment Azure Event Grid gère les événements en l’absence d’accusé de réception d’une distribution.

Event Grid assure une distribution fiable. Il distribue chaque message au moins une fois pour chaque abonnement. Les événements sont envoyés immédiatement au webhook inscrit de chaque abonnement. Si un webhook n’accuse pas réception d’un événement dans les 60 secondes suivant la première tentative de distribution, Event Grid effectue une nouvelle tentative de distribution de l’événement. 

Actuellement, Event Grid envoie chaque événement individuellement aux abonnés. L’abonné reçoit un tableau ne comprenant qu’un seul événement.

## <a name="message-delivery-status"></a>État de distribution du message

Event Grid utilise les codes de réponse HTTP pour accuser réception des événements. 

### <a name="success-codes"></a>Codes de réussite

Les codes de réponse HTTP suivants indiquent qu’un événement a bien été distribué à votre webhook. Event Grid considère que la distribution est effectuée.

- 200 OK
- 202 Accepté

### <a name="failure-codes"></a>Codes d’échec

Les codes de réponse HTTP suivants indiquent un échec de la tentative de distribution d’un événement. 

- 400 Demande incorrecte
- 401 Non autorisé
- 404 Introuvable
- 408 Délai d’expiration de la demande
- 414 URI trop long
- 500 Erreur interne du serveur
- 503 Service indisponible
- 504 Dépassement du délai de la passerelle

Si Event Grid reçoit une erreur qui indique que le point de terminaison n’est pas disponible, il tente à nouveau d’envoyer l’événement. 

## <a name="retry-intervals-and-duration"></a>Intervalles avant nouvelle tentative et durée

Event Grid utilise une stratégie de nouvelle tentative d’interruption exponentielle pour la distribution des événements. Si votre webhook ne répond pas ou s’il retourne un code d’échec, Event Grid effectue une nouvelle tentative de distribution aux intervalles suivants :

1. 10 secondes
2. 30 secondes
3. 1 minute
4. 5 minutes
5. 10 minutes
6. 30 minutes
7. 1 heure

Event Grid ajoute une petite randomisation à tous les intervalles de nouvelle tentative. La remise des événements est renouvelée après une heure, une fois par heure.

Par défaut, Event Grid fait expirer tous les événements qui ne sont pas distribués dans les 24 heures.

## <a name="next-steps"></a>Étapes suivantes

* Pour afficher l’état des remises des événements, consultez [Surveiller la remise des messages Event Grid](monitor-event-delivery.md).
* Pour une présentation d’Event Grid, consultez [À propos d’Event Grid](overview.md).
* Pour une prise en main rapide d’Event Grid, consultez [Créer et acheminer des événements personnalisés avec Azure Event Grid](custom-event-quickstart.md).