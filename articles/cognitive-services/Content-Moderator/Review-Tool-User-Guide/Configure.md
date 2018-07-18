---
title: Konfigurieren der Einstellungen des Prüfungstools in Content Moderator | Microsoft Docs
description: Sie können Ihr Team, Ihre Tags, Konnektoren, Workflows und Anmeldeinformationen konfigurieren oder abrufen.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: a3432a1d8f424fbe78570f47b774c6e7942e16b1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372906"
---
# <a name="about-review-tool-settings"></a>Infos zu den Einstellungen des Prüfungstools #

Über die Registerkarte „Einstellungen“ auf dem Dashboard des Prüfungstools können Sie zahlreiche Komponenten einfach definieren und ändern.

![Prüfungseinstellungen in Content Moderator](images/settings-1.png)

## <a name="team-and-subteams"></a>Team und untergeordnete Teams ## 

Auf dieser Registerkarte können Sie Ihr Team und die untergeordneten Teams verwalten. Sie können nur ein Team haben, aber Sie können [mehrere untergeordnete Teams](subteams.md) erstellen und Einladungen an zukünftige Mitglieder senden. Nachdem Sie Einladungen verschickt haben, können Sie diese überwachen, Berechtigungen für Teammitglieder ändern und weitere Benutzer einladen. Nachdem die Teammitglieder Ihre Einladung angenommen haben, können Sie diese Mitglieder verschiedenen untergeordneten Teams zuordnen. Sie können die Rollen der Teammitglieder entweder als Administratoren oder als Prüfer festlegen: Administratoren können im Gegensatz zu Prüfern andere Benutzer einladen.

![Teameinstellungen in Content Moderator](images/settings-2-team.png)

## <a name="tags"></a>Tags ##

Hier können Sie [benutzerdefinierte Tags](tags.md) definieren, indem Sie den Kurzcode, den Namen und die Beschreibung für Ihre Tags eingeben. Nach der Erstellung sind sie während der Überprüfungen verfügbar. Sie können verschiedene Tags für unterschiedliche Überprüfungen verwenden, indem Sie deren Sichtbarkeit ein- bzw. ausschalten.

![Tag-Einstellungen in Content Moderator](images/settings-3-tags.png)

## <a name="connectors"></a>Konnektoren ##

Workflows fügen Funktionalität hinzu, indem sie Konnektoren zur Kommunikation mit dem Prüfungstool verwenden. Das Prüfungstool ruft die Content Moderator-APIs mit dem Standardworkflow zum Moderieren von Inhalt auf. Wenn Sie sich für das Prüfungstool registrieren, werden die Anmeldeinformationen für die Moderator-API automatisch für Sie bereitgestellt. Darüber hinaus wird die Integration anderer Konnektor-APIs unterstützt, sofern ein Konnektor verfügbar ist. Einige Konnektor sind sofort verfügbar.

Auf der [Registerkarte „Verbindungen“](connectors.md) können Sie Konnektoren verwalten. Sie können Konnektoren hinzufügen oder löschen und Ihren Abonnementschlüssel für einen bestimmten Konnektor finden. Klicken Sie auf „Verbinden“, um diese zu Ihren benutzerdefinierten Workflows hinzufügen. 

![Konnektoreinstellungen in Content Moderator](images/settings-4-connectors.png)

## <a name="workflows"></a>Workflows ##

Workflows werden auf der Registerkarte „Workflows“ verwaltet. Sie können Workflows testen, indem Sie eine Beispieldatei hochladen. Sie können für Bilder und Text auch [benutzerdefinierte Workflows definieren](workflows.md), indem Sie die verfügbaren API-Konnektoren (auf der Registerkarte „Konnektoren“) verwenden. 

![Workfloweinstellungen in Content Moderator](images/settings-5-workflows.png)

## <a name="credentials"></a>Anmeldeinformationen ##

Diese Registerkarte bietet schnellen Zugriff auf Ihren Content Moderator-Abonnementschlüssel, den Sie benötigen, um die in Content Moderator enthaltenen APIs (APIs für Bildmoderation, Textmoderation, Listenverwaltung, Workflow und Überprüfung) zu verwenden.
 
![Anmeldeinformationen in Content Moderator](images/settings-6-credentials.png)
