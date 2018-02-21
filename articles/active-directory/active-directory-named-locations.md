---
title: Benannte Orte in Azure Active Directory | Microsoft-Dokumentation
description: "Erfahren Sie, was benannte Orte sind und wie Sie sie konfigurieren können."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: b6f80cde24edcbec68309ba033d4da16ee97b731
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="named-locations-in-azure-active-directory"></a>Benannte Orte in Azure Active Directory

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

    ![Das Blatt „Neu“](./media/active-directory-named-locations/56.png)

    a. Geben Sie im Feld **Name** einen Namen für den benannten Ort ein.

    b. Geben Sie im Feld **IP-Bereiche** einen IP-Adressbereich ein. Der IP-Adressbereich muss das Format *Classless Inter-Domain Routing* (CIDR) haben.  

    c. Klicken Sie auf **Create**.



## <a name="what-you-should-know"></a>Wichtige Informationen

**Massenaktualisierungen:** Beim Erstellen oder Aktualisieren benannter Orte können Sie für eine Massenaktualisierung eine CSV-Datei mit den IP-Adressbereichen hochladen. Durch das Hochladen werden die IP-Adressbereiche in der Datei der Liste hinzugefügt, anstatt diese zu überschreiben.

![Die Links zum Hochladen und Herunterladen](./media/active-directory-named-locations/09.png)


**Einschränkungen:** Sie können maximal 60 benannte Orte definieren, wobei jedem ein IP-Adressbereich zugewiesen wird. Wenn Sie nur einen benannten Ort konfiguriert haben, können Sie dafür bis zu 500 IP-Adressbereiche definieren.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- Informationen zu **Risikoereignissen** finden Sie unter [Azure Active Directory-Risikoereignisse](active-directory-reporting-risk-events.md).

- Informationen zum **bedingten Zugriff** finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- Informationen zu **Berichten zu riskanten Anmeldungen** finden Sie unter [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal](active-directory-reporting-security-risky-sign-ins.md).  
