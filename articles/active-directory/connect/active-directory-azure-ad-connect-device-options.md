---
title: 'Azure AD Connect: Geräteoptionen | Microsoft-Dokumentation'
description: In diesem Dokument werden die in Azure AD Connect verfügbaren Geräteoptionen im Detail erläutert.
services: active-directory
documentationcenter: ''
author: anandy
manager: samueld
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: anandy
ms.openlocfilehash: d592cbb55643099b6b628a44f182d0a079a6c71e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34354511"
---
#<a name="azure-ad-connect-device-options"></a>Azure AD Connect: Geräteoptionen

Die folgende Dokumentation enthält Informationen zu den verschiedenen Geräteoptionen, die in Azure AD Connect verfügbar sind. Mit Azure AD Connect lassen sich die beiden folgenden Vorgänge konfigurieren: 
* **Hybrid-Azure AD-Einbindung:** Wenn Ihre Umgebung über einen lokalen AD-Fußabdruck verfügt und Sie zudem die Funktionen von Azure Active Directory nutzen möchten, können Sie in Azure AD eingebundene Hybridgeräte implementieren. Hierbei handelt es sich um Geräte, die sowohl in Ihr lokales Active Directory als auch in Ihr Azure Active Directory eingebunden sind.
* **Geräterückschreiben:** Geräterückschreiben ermöglicht bedingten gerätebasierten Zugriff auf geschützte Geräte mit AD FS (ab 2012 R2).

## <a name="configure-device-options-in-azure-ad-connect"></a>Konfigurieren von Geräteoptionen in Azure AD Connect

1.  Führen Sie Azure AD Connect aus. Wählen Sie auf der Seite **Zusätzliche Aufgaben** die Option **Geräteoptionen konfigurieren** aus.
    ![Geräteoptionen konfiguriere](./media/active-directory-aadconnect-device-options/deviceoptions.png) 

    Wenn Sie auf „Weiter“ klicken, wird eine **Übersichtsseite** angezeigt, die detailliert die Vorgänge enthält, die ausgeführt werden können.
    ![Übersicht](./media/active-directory-aadconnect-device-options/deviceoverview.png)

    >[!NOTE]
    > Die neuen Optionen zum Konfigurieren von Geräten sind nur ab Version 1.1.819.0 verfügbar.

2.  Nachdem Sie die Anmeldeinformationen für Azure AD bereitgestellt haben, können Sie auf der Seite „Geräteoptionen“ den auszuführenden Vorgang auswählen.
    ![Gerätevorgänge](./media/active-directory-aadconnect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren der Hybrid-Azure AD-Einbindung](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Konfigurieren/Deaktivieren des Geräterückschreibens](./active-directory-aadconnect-feature-device-writeback.md)

