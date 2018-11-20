---
title: Konfigurieren benannter Orte in Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie benannte Orte konfigurieren.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.component: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 49d0840ff7a41ed48151b65e1351628d32d46d24
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623247"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Schnellstart: Konfigurieren benannter Orte in Azure Active Directory

Mit benannten Orten können Sie in Ihrer Organisation vertrauenswürdige IP-Adressbereiche bezeichnen. Azure AD verwendet benannte Orte zum:
- Erkennen falsch positiver Ergebnisse in [Risikoereignissen](concept-risk-events.md). Die Anmeldung von einem vertrauenswürdigen Ort aus senkt das Anmelderisiko von Benutzern.   
- Konfigurieren des [standortbasierten bedingten Zugriffs](../conditional-access/location-condition.md).

In diesem Schnellstart erfahren Sie, wie Sie benannte Orte in Ihrer Umgebung konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

* Einen Azure AD-Mandanten. Registrieren Sie sich für eine kostenlose [Testversion](https://azure.microsoft.com/trial/get-started-active-directory/). 
* Einen Benutzer, der über die globale Administratorrolle für den Mandanten verfügt.
* Einen IP-Adressbereich, der in Ihrer Organisation etabliert und vertrauenswürdig ist. Der IP-Adressbereich muss das Format des **klassenlosen domänenübergreifenden Routings (CIDR)** haben.

## <a name="configure-named-locations"></a>Konfigurieren benannter Orte

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im linken Bereich **Azure Active Directory** und dann im Abschnitt **Sicherheit** **Bedingter Zugriff** aus.

    ![Registerkarte „Bedingter Zugriff“](./media/quickstart-configure-named-locations/entrypoint.png)

3. Wählen Sie auf der Seite **Bedingter Zugriff** **Benannte Orte** und dann **Neuer Ort** aus.

    ![Benannter Ort](./media/quickstart-configure-named-locations/namedlocation.png)

6. Füllen Sie das Formular auf der neuen Seite aus. 

    * Geben Sie im Feld **Name** einen Namen für den benannten Ort ein.
    * Geben Sie im Textfeld **IP-Adressbereiche** den IP-Adressbereich im CIDR-Format ein.  
    * Klicken Sie auf **Create**.
    
    ![Das Blatt „Neu“](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

- [Bedingter Zugriff auf Azure AD](../active-directory-conditional-access-azure-portal.md)
- [Standortbedingungen beim bedingten Zugriff in Azure Active Directory](../conditional-access/location-condition.md)
- [Bericht „Riskante Anmeldungen“](concept-risky-sign-ins.md)  
