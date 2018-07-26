---
title: Konfigurieren benannter Orte in Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie benannte Orte konfigurieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.component: protection
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f739917b201d5255716d22930d7c4bd9e6602f37
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224516"
---
# <a name="configure-named-locations-in-azure-active-directory"></a>Konfigurieren benannter Orte in Azure Active Directory

Mit benannten Orten können Sie in Ihrer Organisation vertrauenswürdige IP-Adressbereiche bezeichnen. Azure Active Directory verwendet benannte Orte im folgenden Kontext:

- Die Erkennung von [Risikoereignissen](active-directory-reporting-risk-events.md), um die Anzahl der gemeldeten falsch positiven Ergebnisse zu reduzieren.  

- [Standortbasierter bedingter Zugriff](active-directory-conditional-access-locations.md).


In diesem Artikel wird erklärt, wie Sie benannte Orte in Ihrer Umgebung konfigurieren können.


## <a name="entry-points"></a>Einstiegspunkte

Sie können auf die Seite mit der Konfiguration des benannten Orts im Abschnitt **Sicherheit** der Azure Active Directory-Seite zugreifen, indem Sie auf Folgendes klicken:

![Einstiegspunkte](./media/active-directory-named-locations/34.png)

- **Bedingter Zugriff:**

    - Klicken Sie im Bereich **Verwalten** auf **Benannte Orte**.
    
        ![Der Befehl „Benannte Orte“](./media/active-directory-named-locations/06.png)

- **Riskante Anmeldungen:**

    - Klicken Sie auf der Symbolleiste am oberen Rand auf **Bekannte IP-Adressbereiche hinzufügen**.

       ![Der Befehl „Benannte Orte“](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Konfigurationsbeispiel

**So konfigurieren Sie einen benannten Ort**

1. Melden Sie sich als globaler Administrator beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im linken Bereich auf **Azure Active Directory**.

    ![Klicken Sie im linken Bereich auf den Link „Azure Active Directory“.](./media/active-directory-named-locations/01.png)

3. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Sicherheit** auf **Bedingter Zugriff**.

    ![Der Befehl „Bedingter Zugriff“](./media/active-directory-named-locations/05.png)


4. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Benannte Orte**.

    ![Der Befehl „Benannte Orte“](./media/active-directory-named-locations/06.png)


5. Klicken Sie auf der Seite **Benannte Orte** auf **Neuer Standort**.

    ![Der Befehl „Neuer Standort“](./media/active-directory-named-locations/07.png)


6. Gehen Sie auf der Seite **Neu** wie folgt vor:

    ![Das Blatt „Neu“](./media/active-directory-named-locations/61.png)

    a. Geben Sie im Feld **Name** einen Namen für den benannten Ort ein.

    b. Geben Sie im Feld **IP-Bereiche** einen IP-Adressbereich ein. Der IP-Adressbereich muss das Format *Classless Inter-Domain Routing* (CIDR) haben.  

    c. Klicken Sie auf **Create**.



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

- [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md)

- [Standortbedingungen beim bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-locations.md)

- [Azure Active Directory-Risikoereignisse](active-directory-reporting-risk-events.md)

- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal](active-directory-reporting-security-risky-sign-ins.md)  
