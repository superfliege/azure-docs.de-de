---
title: 'Azure AD Connect: Geräteoptionen | Microsoft-Dokumentation'
description: In diesem Dokument werden die in Azure AD Connect verfügbaren Geräteoptionen im Detail erläutert.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6993895b6f2b836308b8ed7c375212c3fae12768
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46305919"
---
#<a name="azure-ad-connect-device-options"></a>Azure AD Connect: Geräteoptionen

Die folgende Dokumentation enthält Informationen zu den verschiedenen Geräteoptionen, die in Azure AD Connect verfügbar sind. Mit Azure AD Connect lassen sich die beiden folgenden Vorgänge konfigurieren: 
* **Azure AD Hybrid Join**: Wenn Ihre Umgebung lokalen AD-Speicherbedarf aufweist und Sie zudem die Funktionen von Azure Active Directory nutzen möchten, können Sie in Azure AD Hybrid eingebundene Geräte implementieren. Diese Geräte sind sowohl in Ihr lokales Active Directory als auch in Ihr Azure Active Directory eingebunden.
* **Geräterückschreiben:** Geräterückschreiben ermöglicht bedingten gerätebasierten Zugriff auf geschützte Geräte mit AD FS (ab 2012 R2).

## <a name="configure-device-options-in-azure-ad-connect"></a>Konfigurieren von Geräteoptionen in Azure AD Connect

1.  Führen Sie Azure AD Connect aus. Wählen Sie auf der Seite **Zusätzliche Aufgaben** die Option **Geräteoptionen konfigurieren** aus.  Klicken Sie auf **Weiter**.
    ![Geräteoptionen konfiguriere](./media/how-to-connect-device-options/deviceoptions.png) 

    Die Seite **Übersicht** zeigt die Details an.
    ![Übersicht](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > Die neuen Optionen zum Konfigurieren von Geräten sind nur ab Version 1.1.819.0 verfügbar.

2.  Nachdem Sie die Anmeldeinformationen für Azure AD bereitgestellt haben, können Sie auf der Seite „Geräteoptionen“ den auszuführenden Vorgang auswählen.
    ![Gerätevorgänge](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren der Hybrid-Azure AD-Einbindung](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Konfigurieren/Deaktivieren des Geräterückschreibens](how-to-connect-device-writeback.md)

