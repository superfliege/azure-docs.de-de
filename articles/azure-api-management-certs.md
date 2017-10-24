---
title: "Hochladen eines Zertifikats für die Azure-Verwaltungs-API | Microsoft-Dokumentation"
description: "Erfahren Sie, wie ein Verwaltungs-API-Zertifikat für das klassische Azure-Portal hochgeladen wird."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 89c08f2d1e0e244503e3fc4355cad0b4391618e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Hochladen eines Verwaltungszertifikats für die Azure-Verwaltung-API
Verwaltungszertifikate ermöglichen Ihnen die Authentifizierung mit dem von Azure bereitgestellten klassischen Bereitstellungsmodell. Diese Zertifikate werden in vielen Programmen und Tools (z.B. Visual Studio oder Azure SDK) zum Automatisieren der Konfiguration und Bereitstellung verschiedener Azure-Dienste verwendet. 

> [!WARNING]
> Vorsicht ist geboten! Alle Personen, die die Authentifizierung mit diesen Zertifikaten durchführen, können das zugeordnete Abonnement verwalten.
>
>

Weitere Informationen zu Azure-Zertifikaten (und zum Erstellen eines selbstsignierten Zertifikats) finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Sie können auch [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) für die Authentifizierung von Clientcode zur Automatisierung verwenden.

**Hinweis:** Sie müssen ein Co-Administrator des Abonnements sein, um Vorgänge unter Verwaltungszertifikaten auszuführen. [Erfahren Sie mehr](https://go.microsoft.com/fwlink/?linkid=849300) darüber, wie Sie Co-Administratoren dem neuen Azure-Portal hinzufügen oder daraus entfernen können. 

## <a name="upload-a-management-certificate"></a>Hochladen eines Verwaltungszertifikats
Sobald Sie ein Verwaltungszertifikat erstellt haben (CER-Datei nur mit dem öffentlichen Schlüssel), können Sie es in das Portal hochladen. Wenn das Zertifikat im Portal verfügbar ist, kann jeder Benutzer mit einem entsprechenden Zertifikat (privater Schlüssel) über die Verwaltungs-API eine Verbindung herstellen und auf die Ressourcen für das zugehörige Abonnement zugreifen.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Klicken Sie unten in der Azure-Dienstliste auf **Weitere Dienste**, und wählen Sie dann in der Dienstgruppe _Allgemein_ die Option **Abonnements** aus.

    ![Abonnementmenü](./media/azure-api-management-certs/subscriptions_menu.png)

3. Stellen Sie sicher, dass Sie das Abonnement auswählen, das Sie dem Zertifikat zuordnen möchten.     
4. Wenn Sie das richtige Abonnement ausgewählt haben, klicken Sie in der Gruppe _Einstellungen_ auf **Verwaltungszertifikate**.

    ![Einstellungen](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Wählen Sie die Schaltfläche **Hochladen** aus.

    ![Seite zum Hochladen für Zertifikate](./media/azure-api-management-certs/certificates_page.png)
6. Füllen Sie das Dialogfeld aus, und wählen Sie **Hochladen** aus.

    ![Einstellungen](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt einem Abonnement ein Verwaltungszertifikat zugeordnet haben, können Sie (nachdem Sie das entsprechende Zertifikat lokal installiert haben) programmgesteuert eine Verbindung mit der [REST-API für das klassische Bereitstellungsmodell](https://msdn.microsoft.com/library/azure/mt420159.aspx) herstellen und die verschiedenen Azure-Ressourcen automatisieren, die auch diesem Abonnement zugeordnet sind.
