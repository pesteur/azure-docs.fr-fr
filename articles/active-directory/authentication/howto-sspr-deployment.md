---
title: Guide de déploiement de réinitialisation de mot de passe libre-service - Azure Active Directory
description: Conseils pour réussir le lancement de la réinitialisation du mot de passe libre-service Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 4d3e07c6c395645ef34b1707f33a4e37a20bf05d
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Comment réussir le lancement de la réinitialisation de mot de passe en libre-service

Pour garantir le déploiement sans problèmes de la fonctionnalité de réinitialisation du mot de passe libre-service (SSPR) d’Azure Active Directory (Azure AD), les clients procèdent généralement comme suit :

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. [Activez la réinitialisation du mot de passe dans votre répertoire](quickstart-sspr.md).
2. [Configurez les autorisations Active Directory locales pour l’écriture différée du mot de passe](howto-sspr-writeback.md#active-directory-permissions).
3. [Configurez l’écriture différée du mot de passe](howto-sspr-writeback.md#configure-password-writeback) pour écrire des mots de passe en différé à partir d’Azure AD dans votre répertoire local.
4. [Affectez et vérifiez les licences requises](concept-sspr-licensing.md).
5. Décidez si vous souhaitez effectuer un déploiement progressif. Si vous souhaitez lancer progressivement la réinitialisation de mot de passe en libre-service, vous pouvez limiter l’accès à un groupe d’utilisateurs afin de piloter le programme avec un groupe spécifique. Pour effectuer le lancement pour un groupe spécifique, définissez le commutateur **Réinitialisation du mot de passe en libre-service activée** sur **Sélectionné** et sélectionnez le groupe de sécurité qui doit pouvoir utiliser la réinitialisation du mot de passe.  L’imbrication de groupes de sécurité est prise en charge ici.
6. Remplissez les [Données d’authentification](howto-sspr-authenticationdata.md) nécessaires pour l’inscription de vos utilisateurs, telles que leur téléphone de bureau, téléphone mobile et adresse de messagerie.
7. [Personnalisez l’expérience de connexion Azure AD pour inclure la marque de votre société](concept-sspr-customization.md).
8. Apprenez à vos utilisateurs à utiliser la réinitialisation du mot de passe libre-service. Envoyez-leur des instructions pour leur montrer comment s’inscrire et comment réinitialiser leurs mots de passe.
9. Déterminez quand vous voulez appliquer l’inscription. Vous pouvez choisir d’appliquer l’inscription à tout moment. Vous pouvez également demander aux utilisateurs de reconfirmer leurs informations d’authentification après un certain laps de temps.
10. Utilisez la fonctionnalité de création de rapports. Vous pouvez passer en revue l’inscription et l’utilisation des utilisateurs au fil du temps avec la [fonctionnalité de création de rapports fournie par Azure AD](howto-sspr-reporting.md).
11. Activez la réinitialisation du mot de passe. Lorsque vous êtes prêt, activez la réinitialisation du mot de passe pour tous les utilisateurs en définissant le commutateur **Réinitialisation du mot de passe en libre-service activée** sur **Tout le monde**. 

   > [!NOTE]
   > Modifier cette option de façon qu’elle passe d’un groupe sélectionné à tout le monde ne rend pas non valides les données d’authentification existantes qu’un utilisateur a enregistrées dans le cadre d’un groupe de test. Les utilisateurs qui sont configurés et qui possèdent des données d’authentification valides enregistrées continuent de fonctionner.

12. [Permettez aux utilisateurs de Windows 10 de réinitialiser leur mot de passe sur l’écran de connexion](tutorial-sspr-windows.md).

   > [!IMPORTANT]
   > Testez la réinitialisation de mot de passe libre-service avec un utilisateur plutôt qu’un administrateur, car Microsoft applique des spécifications d’authentification forte pour les comptes d’administrateur Azure. Pour plus d’informations sur la stratégie de mot de passe administrateur, consultez notre article [Stratégie de mot de passe](concept-sspr-policy.md#administrator-password-policy-differences).

## <a name="email-based-rollout"></a>Déploiement par courrier électronique

De nombreux clients estiment qu’une campagne par courrier électronique incluant des instructions simples d’utilisation constitue le moyen le plus simple pour que les utilisateurs aient recours à la réinitialisation du mot de passe libre-service. [Nous avons créé trois courriers électroniques simples que vous pouvez utiliser comme modèles pour vous aider dans votre lancement](https://www.microsoft.com/download/details.aspx?id=56768) :

* **Bientôt disponible** : modèle de courrier électronique à utiliser dans les semaines ou les jours précédant le lancement pour informer les utilisateurs s’ils ont besoin d’effectuer une action.
* **Disponible dès maintenant** : modèle de courrier électronique à utiliser le jour du lancement du programme pour inciter les utilisateurs à s’inscrire et à confirmer leurs données d’authentification. En s’inscrivant maintenant, les utilisateurs pourront utiliser la réinitialisation du mot de passe libre-service lorsqu’ils en auront besoin.
* **Rappel d’inscription** : modèle de courrier électronique à utiliser dans les jours ou les semaines qui suivent le déploiement pour rappeler aux utilisateurs de s’inscrire et de confirmer leurs données d’authentification.

![E-mail][Email]

## <a name="create-your-own-password-portal"></a>Création de votre propre portail de mot de passe

Nombre de nos clients choisissent d’héberger une page web et de créer une entrée DNS racine, comme https://passwords.contoso.com. Ils remplissent cette page avec des liens vers les informations suivantes :

* [Portail de réinitialisation du mot de passe Azure AD - https://aka.ms/sspr](https://aka.ms/sspr)
* [Portail d’inscription à la réinitialisation de mot de passe Azure AD - https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Portail de modification de mot de passe Azure AD - https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Autres informations spécifiques à l’organisation

Dans toutes vos communications par courrier électronique ou prospectus, vous pouvez inclure une URL de marque, facile à retenir que les utilisateurs peuvent suivre quand ils doivent utiliser les services. Pour votre bénéfice, nous avons créé une [page d’exemple de réinitialisation du mot de passe](https://github.com/ajamess/password-reset-page) que vous pouvez utiliser et personnaliser pour les besoins de votre organisation.

## <a name="step-by-step-deployment-plan"></a>Plan de déploiement étape par étape

Le groupe de produits Azure Active Directory a créé un [plan de déploiement étape par étape](https://aka.ms/SSPRDeploymentPlan) que les organisations peuvent utiliser en parallèle avec la documentation disponible sur ce site pour effectuer une étude de cas et un plan de déploiement de la réinitialisation de mot de passe en libre-service.

## <a name="use-enforced-registration"></a>Utilisation de l’inscription forcée

Si vous souhaitez que vos utilisateurs s’inscrivent pour la réinitialisation du mot de passe, vous pouvez les obliger à s’inscrire lorsqu’ils se connectent via Azure AD. Vous pouvez activer cette option dans le volet **Réinitialisation de mot de passe** de votre répertoire en activant l’option **Demander aux utilisateurs de s’inscrire lorsqu’ils se connectent** dans l’onglet **Inscription**.

Les administrateurs peuvent demander aux utilisateurs de se réinscrire après une période spécifique. Ils peuvent définir l’option **Nombre de jours avant que les utilisateurs ne soient invités à reconfirmer leurs informations d’authentification** sur 0 à 730 jours.

Une fois que vous avez activé cette option, lorsque les utilisateurs se connectent, un message les informe que leur administrateur leur a demandé de vérifier leurs informations d’authentification.

## <a name="populate-authentication-data"></a>Renseigner les données d’authentification

Vous devez [renseigner les données d’authentification de vos utilisateurs](howto-sspr-authenticationdata.md). De cette façon, les utilisateurs n’ont pas besoin de s’inscrire pour la réinitialisation du mot de passe avant de pouvoir utiliser la fonctionnalité SSPR. Tant que leurs données d’authentification sont conformes à la stratégie de réinitialisation de mot de passe que vous avez définie, les utilisateurs sont en mesure de réinitialiser leurs mots de passe.

## <a name="disable-self-service-password-reset"></a>Désactiver la réinitialisation du mot de passe libre-service

Il est facile de désactiver la réinitialisation du mot de passe libre-service. Ouvrez votre locataire Azure AD et accédez à **Réinitialisation de mot de passe** > **Propriétés**, puis sélectionnez **Aucun** sous **Réinitialisation de mot de passe en libre-service activée**.

## <a name="next-steps"></a>Étapes suivantes

* [Réinitialiser ou modifier votre mot de passe](../active-directory-passwords-update-your-own-password.md)
* [S’inscrire pour la réinitialisation du mot de passe en libre-service](../active-directory-passwords-reset-register.md)
* [Vous avez une question relative à la licence ?](concept-sspr-licensing.md)
* [Quelles données sont utilisées par la réinitialisation de mot de passe en libre-service et quelles données vous devez renseigner pour vos utilisateurs ?](howto-sspr-authenticationdata.md)
* [Quelles sont les options de stratégie disponibles avec la réinitialisation de mot de passe en libre-service ?](concept-sspr-policy.md)
* [Quelle est l’écriture différée de mot de passe et pourquoi dois-je m’y intéresser ?](howto-sspr-writeback.md)
* [Comment puis-je générer des rapports sur l’activité dans la réinitialisation de mot de passe en libre-service ?](howto-sspr-reporting.md)
* [Quelles sont toutes les options disponibles dans la réinitialisation de mot de passe en libre-service et que signifient-elles ?](concept-sspr-howitworks.md)
* [Je pense qu’il y a une panne quelque part. Comment puis-je résoudre les problèmes de la réinitialisation de mot de passe en libre-service ?](active-directory-passwords-troubleshoot.md)
* [J’ai une question à laquelle je n’ai pas trouvé de réponse ailleurs](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Personnalisez ces modèles de messages électroniques en fonction des exigences de votre organisation"
