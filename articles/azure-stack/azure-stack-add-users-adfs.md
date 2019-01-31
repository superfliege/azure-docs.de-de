---
title: Hinzufügen von Benutzern für ADFS-Bereitstellungen von Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Benutzer für ADFS-Bereitstellungen von Azure Stack hinzufügen.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 7ecffacee03650adc709a055ba942ef182da5fb8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239979"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Hinzufügen von Azure Stack-Benutzern in AD FS
Sie können das Snap-In **Active Directory-Benutzer und -Computer** zum Hinzufügen weiterer Benutzer zu einer Azure Stack-Umgebung verwenden und dabei AD FS als Identitätsanbieter nutzen.

## <a name="add-windows-server-active-directory-users"></a>Hinzufügen von Windows Server Active Directory-Benutzern
> [!TIP]
> In diesem Beispiel wird das standardmäßige ASDK-Active Directory „azurestack.local“ verwendet. 

1.  Melden Sie sich bei einem Computer mit einem Konto an, das Zugriff auf die Windows-Verwaltungstools bietet, und öffnen Sie eine neue Microsoft Management Console (MMC).
2.  Klicken Sie auf **Datei > Snap-Ins hinzufügen bzw. entfernen**.
3.  Klicken Sie auf **Active Directory-Benutzer und -Computer** > **AzureStack.local** > **Benutzer**.
4.  Klicken Sie auf **Aktion** > **Neu** > **Benutzer**.
5.  Geben Sie im Fenster „Neues Objekt – Benutzer“ ein Kennwort ein, und bestätigen Sie es.
6.  Klicken Sie auf **Weiter**, um die Eingabe von Werten abzuschließen, und dann auf „Fertig stellen“, um den Benutzer zu erstellen.


## <a name="next-steps"></a>Nächste Schritte
[Erstellen von Dienstprinzipalen](azure-stack-create-service-principals.md)