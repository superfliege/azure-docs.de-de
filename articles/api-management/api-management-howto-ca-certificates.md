---
title: Hinzufügen eines benutzerdefinierten ZS-Zertifikats – Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure API Management ein benutzerdefiniertes ZS-Zertifikat hinzufügen.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: apimpm
ms.openlocfilehash: b05713fd8acb49b25dba04781c1049c9f05115fb
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444688"
---
# <a name="how-to-add-a-custom-ca-certificate-in-azure-api-management"></a>Hinzufügen eines benutzerdefinierten ZS-Zertifikats in Azure API Management

Azure API Management ermöglicht die Installation von ZS-Zertifikaten (Zertifizierungsstellenzertifikaten) auf dem Computer im vertrauenswürdigen Stammzertifikatspeicher und den Zwischenzertifikatspeichern. Diese Funktion sollte verwendet werden, wenn Ihre Dienste ein benutzerdefiniertes ZS-Zertifikat erfordern.

Dieser Artikel erläutert die Verwaltung von ZS-Zertifikaten einer Azure API Management-Dienstinstanz im Azure-Portal.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="step1"> </a>Hochladen eines ZS-Zertifikats

![Hinzufügen von ZS-Zertifikaten](media/api-management-howto-ca-certificates/00.png)

Gehen Sie folgendermaßen vor, um ein neues ZS-Zertifikat hochzuladen. Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Tutorial [Erstellen einer API Management-Dienstinstanz](get-started-create-service-instance.md).

1. Navigieren Sie im Azure-Portal zu Ihrer Azure API Management-Dienstinstanz.

2. Wählen Sie im Menü die Option **ZS-Zertifikate** aus.

3. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.  

    ![Hinzufügen von ZS-Zertifikaten](media/api-management-howto-ca-certificates/01.png)  

4. Suchen Sie nach dem Zertifikat, und wählen Sie den Zertifikatspeicher aus. Es ist nur der öffentliche Schlüssel erforderlich, kein Kennwort.

    ![Hinzufügen von ZS-Zertifikaten](media/api-management-howto-ca-certificates/02.png)  

5. Klicken Sie auf **Speichern**. Dieser Vorgang kann einige Minuten dauern.

    ![Hinzufügen von ZS-Zertifikaten](media/api-management-howto-ca-certificates/03.png)  

> [!NOTE]
> Sie können ein ZS-Zertifikat mithilfe des PowerShell-Befehls `New-AzureRmApiManagementSystemCertificate` hochladen.

## <a name="step1a"></a>Löschen eines Clientzertifikats

Um ein Zertifikat zu löschen, klicken Sie auf das Kontextmenü **...**, und wählen Sie neben dem betreffenden Zertifikat **Löschen** aus.

![Löschen von ZS-Zertifikaten](media/api-management-howto-ca-certificates/04.png)  

[Upload a CA certificate]: #step1
[Delete a CA certificate]: #step1a