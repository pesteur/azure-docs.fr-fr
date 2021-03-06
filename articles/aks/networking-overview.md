---
title: Configuration réseau dans Azure Kubernetes Service (AKS)
description: En savoir plus sur la configuration réseau de base et avancée dans Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/07/2018
ms.author: marsma
ms.openlocfilehash: 818bae2e05f6a3256ccbf0cbcc901dd337b9a260
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuration réseau dans Azure Kubernetes Service (AKS)

Lorsque vous créez un cluster Azure Kubernetes Service (AKS), vous pouvez sélectionner deux options de mise en réseau : **de base** ou **avancée**.

## <a name="basic-networking"></a>Mise en réseau de base

L’option de mise en réseau **de base** est la configuration par défaut de la création de cluster AKS. La configuration réseau du cluster et de ses pods est gérée intégralement par Azure, et est appropriée pour les déploiements qui ne nécessitent pas de configuration personnalisée de réseau virtuel. Avec la mise en réseau de base, vous ne disposez d’aucun contrôle sur la configuration réseau, par exemple sur les sous-réseaux ou les plages d’adresses IP affectées au cluster.

Les nœuds d’un cluster AKS configurés pour la mise en réseau de base utilisent le plug-in Kubernetes [kubenet][kubenet].

## <a name="advanced-networking"></a>Mise en réseau avancée

La mise en réseau **avancée** place vos pods dans un réseau virtuel Azure que vous configurez, en leur octroyant une connectivité automatique aux ressources de réseau virtuel et une intégration avec le riche ensemble de fonctionnalités offert par les réseaux virtuels.
La mise en réseau avancée est actuellement disponible uniquement lors du déploiement de clusters AKS dans le [portail Azure][portal] ou avec un modèle Resource Manager.

Les nœuds d’un cluster AKS configurés pour la mise en réseau avancée utilisent le plug-in Kubernetes[Azure Container Networking Interface (CNI)][cni-networking].

![Diagramme représentant 2 nœuds avec des ponts les reliant chacun à un réseau virtuel Azure][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Fonctions de la mise en réseau avancée

La mise en réseau avancée procure les avantages suivants :

* Déployez votre cluster AKS dans un réseau virtuel existant ou créez un nouveau réseau virtuel et un sous-réseau pour votre cluster.
* Chaque pod du cluster se voit affecter une adresse IP dans le réseau virtuel, et peut communiquer directement avec les autres pods du cluster et les autres machines virtuelles du réseau virtuel.
* Un pod peut se connecter à d’autres services dans un réseau virtuel appairé, et à d’autres réseaux locaux via des connexions ExpressRoute et VPN site à site (S2S). Les pods sont également accessibles en local.
* Exposez un service Kubernetes en interne ou en externe via Azure Load Balancer. Également une fonction de la mise en réseau de base.
* Les pods dans un sous-réseau disposant de points de terminaison de service activés peuvent se connecter en toute sécurité aux services Azure (Stockage et SQL Database, par exemple).
* Utilisez les itinéraires définis par l’utilisateur pour acheminer le trafic des pods vers une appliance virtuelle réseau.
* Les pods peuvent accéder aux ressources sur les réseaux Internet publics. Également une fonction de la mise en réseau de base.

> [!IMPORTANT]
> Chaque nœud d’un cluster AKS configuré pour la mise en réseau avancée peut héberger un maximum de **30 pods**. Chaque réseau virtuel configuré pour une utilisation avec le plug-in Azure CNI est limité à **4 096 adresses IP** (/20).

## <a name="configure-advanced-networking"></a>Configurer la mise en réseau avancée

Lorsque vous [créez un cluster AKS](kubernetes-walkthrough-portal.md) dans le portail Azure, les paramètres suivants sont configurables pour la mise en réseau avancée :

**Réseau virtuel** : le réseau virtuel dans lequel vous souhaitez déployer le cluster Kubernetes. Si vous souhaitez créer un réseau virtuel pour votre cluster, sélectionnez *Créer un nouveau*, puis suivez la procédure décrite dans la section *Créer un réseau virtuel*.

**Sous-réseau** : le sous-réseau du réseau virtuel dans lequel vous souhaitez déployer le cluster. Si vous souhaitez créer un nouveau sous-réseau dans le réseau virtuel pour votre cluster, sélectionnez *Créer un nouveau*, puis exécutez la procédure décrite dans la section *Créer un sous-réseau*.

**Plage d’adresses de services Kubernetes** : la plage des adresses IP du service du cluster Kubernetes. Cette plage doit être située en dehors de la plage d’adresses IP du réseau virtuel de votre cluster.

**Adresse IP du service DNS Kubernetes** :l’adresse IP du service DNS du cluster. Cette adresse doit se situer dans la *plage d’adresses du service Kubernetes*.

**Adresse du pont docker** : l’adresse IP et le masque réseau à affecter au pont docker. Cette adresse IP doit être située en dehors de la plage d’adresses IP du réseau virtuel de votre cluster.

La capture d’écran suivante de votre portail Azure représente un exemple de configuration de ces paramètres durant la création du cluster AKS :

![Configuration de la mise en réseau avancée dans le portail Azure][portal-01-networking-advanced]

## <a name="plan-ip-addressing-for-your-cluster"></a>Planifier l’adressage IP pour votre cluster

Les clusters configurés avec la mise en réseau avancée nécessitent une planification supplémentaire. Les tailles de votre réseau virtuel et de son sous-réseau doivent être suffisantes pour prendre en charge le volume de pods que vous envisagez d’exécuter en simultané dans le cluster, ainsi que vos exigences de mise à l’échelle.

Les adresses IP des pods et des nœuds de cluster sont affectées à partir du sous-réseau spécifié du réseau virtuel. Chaque nœud est configuré avec une adresse IP primaire, qui est l’adresse IP du nœud, et 30 adresses IP supplémentaires préconfigurées par Azure CNI, qui sont affectées aux pods planifiés sur le nœud. Lorsque vous faites monter en charge votre cluster, chaque nœud est configuré de manière similaire avec des adresses IP du sous-réseau.

Comme indiqué précédemment, chaque réseau virtuel configuré pour une utilisation avec le plug-in Azure CNI est limité à **4 096 adresses IP** (/20). Chaque nœud de cluster configuré pour la mise en réseau avancée peut héberger un maximum de **30 pods**.

## <a name="frequently-asked-questions"></a>Questions fréquentes (FAQ)

La série suivante de questions-réponses s’applique à la configuration réseau **avancée**.

* *Puis-je configurer la mise en réseau avancée avec l’interface Azure CLI ?*

  Non. La mise en réseau avancée est actuellement disponible uniquement lors du déploiement de clusters AKS dans le portail Azure ou avec un modèle Resource Manager.

* *Puis-je déployer des machines virtuelles dans le sous-réseau de mon cluster ?*

  Non. Le déploiement des machines virtuelles dans le sous-réseau utilisé par votre cluster Kubernetes n’est pas pris en charge. Les machines virtuelles peuvent être déployées dans le même réseau virtuel, mais dans un sous-réseau différent.

* *Puis-je configurer des stratégies de réseau spécifiques aux pods ?*

  Non. Les stratégies de réseau spécifiques aux pods ne sont actuellement pas prises en charge.

* *Le nombre maximal de pods pouvant être déployés sur un nœud peut-il être configuré ?*

  Par défaut, chaque nœud peut héberger 30 pods au maximum. Actuellement, pour modifier la valeur maximale, vous pouvez uniquement modifier la propriété `maxPods` lors du déploiement d’un cluster avec un modèle Resource Manager.

* *Comment configurer des propriétés supplémentaires pour le sous-réseau développé lors de la création du cluster AKS ? Par exemple, des points de terminaison du service.*

  La liste complète des propriétés pour le réseau virtuel et les sous- réseaux développés durant la création du cluster AKS peut être configurée dans la page de configuration du réseau virtuel standard du portail Azure.

## <a name="next-steps"></a>Étapes suivantes

### <a name="networking-in-aks"></a>Mise en réseau dans AKS

Pour plus d’informations sur la mise en réseau dans AKS, consultez les articles suivants :

[Utiliser une adresse IP statique avec l’équilibrage de charge d’Azure Kubernetes Service (AKS)](static-ip.md)

[Entrée HTTPS sur Azure Container Service (AKS)](ingress.md)

[Utiliser un équilibreur de charge interne avec Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>Moteur ACS

Le [moteur Azure Container Service (moteur ACS)][acs-engine] est un projet open source générant des modèles Azure Resource Manager que vous pouvez utiliser pour déployer des clusters Docker sur Azure. Kubernetes, DC/OS, le mode Swarm et les orchestrateurs Swarm peuvent être déployés avec le moteur ACS.

Les clusters Kubernetes créés avec le moteur ACS prennent en charge les plug-ins [kubenet][kubenet] et [Azure CNI][cni-networking]. En l’état, les scénarios de mise en réseau de base et avancée sont pris en charge par le moteur ACS.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[aks-ssh]: aks-ssh.md
