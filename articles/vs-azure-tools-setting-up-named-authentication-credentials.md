---
title: Einrichten benannter Authentifizierungsanmeldeinformationen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Anmeldeinformationen angeben, mit denen Visual Studio Anforderungen für Azure authentifizieren kann, um eine Anwendung von Visual Studio aus in Azure zu veröffentlichen oder einen vorhandenen Clouddienst zu überwachen.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 61570907-42a1-40e8-bcd6-952b21a55786
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: 48c410df1768c2cae7807aa1538d76867a581c71
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42144477"
---
# <a name="set-up-named-authentication-credentials"></a>Einrichten benannter Authentifizierungsanmeldeinformationen

Wenn Sie eine Anwendung in Azure veröffentlichen oder einen vorhandenen Clouddienst überwachen möchten, benötigt Visual Studio folgende Anmeldeinformationen, um an Azure gerichtete Anforderungen zu authentifizieren: die Azure-Abonnement-ID und ein gültiges X.509-Zertifikat (v3) mit einem Schlüssel, der mindestens 2.048 Bit umfasst. Diese Anmeldeinformationen können auf folgende Arten angegeben werden:

- Navigieren Sie in Visual Studio zu **Ansicht > Server-Explorer**, klicken Sie mit der rechten Maustaste auf den Knoten **Azure**, klicken Sie auf **Verbindung mit Microsoft Azure-Abonnement herstellen**, und melden Sie sich an.
- Erstellen Sie eine Abonnementdatei (`.publishsettings`). Diese enthält einen öffentlichen Schlüssel für das Zertifikat. Die Abonnementdatei kann Anmeldeinformationen für mehrere Abonnements enthalten, wie in diesem Artikel beschrieben.

Hinweis: Diese Anmeldeinformationen unterscheiden sich von Anmeldeinformationen zur Authentifizierung von Anforderungen an Azure-Speicherdienste.

## <a name="create-a-subscription-file"></a>Erstellen einer Abonnementdatei

Klicken Sie im Server-Explorer mit der rechten Maustaste auf den Knoten **Azure**, und klicken Sie anschließend auf **Abonnements verwalten und filtern**. Klicken Sie auf die Registerkarte **Zertifikate**, und führen Sie eine der folgenden Aktionen aus:

- Wählen Sie **Importieren**, um das Dialogfeld **Microsoft Azure-Abonnements importieren** zu öffnen. Klicken Sie auf den Link **Abonnementdatei herunterladen**, und speichern Sie die heruntergeladene Datei im Browser an einem temporären Speicherort. Wechseln Sie zurück zum Dialogfeld, navigieren Sie zum Downloadspeicherort, und importieren Sie die Datei für die Authentifizierung.
- Wählen Sie ein aktives Abonnement aus, und klicken Sie auf **Bearbeiten**. Dadurch wird ein Dialogfeld geöffnet, in dem Sie ein vorhandenes Abonnement für die Verwendung bei der Authentifizierung bearbeiten können.
- Klicken Sie auf **Neu**, um das Dialogfeld **Neues Abonnement** zu öffnen, und geben Sie die erforderlichen Details an. Um das Zertifikat für den im Dialogfeld angegebenen Clouddienst hochzuladen, melden Sie sich beim Azure-Portal an, navigieren Sie zu Ihrem Clouddienst, klicken Sie auf **Einstellungen > Verwaltungszertifikate**, klicken Sie auf **Hochladen**, und geben Sie dann den Pfad zu der Datei vom Typ `.cer` an.

Wenn Sie selbst ein Zertifikat erstellen möchten, lesen Sie die Anleitung unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx), und laden Sie das Zertifikat anschließend manuell in das [Azure-Portal](https://portal.azure.com/) hoch.

## <a name="next-steps"></a>Nächste Schritte

- [Allgemeine Übersicht über die Web-Apps](https://docs.microsoft.com/azure/app-service/)
- [Bereitstellen der App in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-deploy-local-git) 
- [Bereitstellen von Webaufträgen mit Visual Studio](https://docs.microsoft.com/azure/app-service/websites-dotnet-deploy-webjobs)
- [Erstellen und Bereitstellen eines Clouddiensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)