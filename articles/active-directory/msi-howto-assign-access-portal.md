---
title: "Zuweisen von MSI-Zugriff zu einer Azure-Ressource über das Azure-Portal"
description: "Ausführliche Anweisungen für das Zuweisen von MSI zu einer Ressource und das Zuweisen des Zugriffs für eine andere Ressource über das Azure-Portal."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 6522a5ef7f1e1af89ee70ee74041e1e69614ebca
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Zuweisen des MSI-Zugriffs (MSI = Managed Service Identity, verwaltete Dienstidentität) zu einer Ressource über das Azure-Portal

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Nachdem Sie eine Azure-Ressource mit einer MSI konfiguriert haben, können Sie der MSI genau wie bei allen anderen Sicherheitsprinzipalen den Zugriff auf eine andere Ressource erteilen. In diesem Artikel wird gezeigt, wie Sie der MSI eines virtuellen Azure-Computers über das Azure-Portal den Zugriff auf ein Azure-Speicherkonto oder eine VM-Skalierungsgruppe gewähren.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Verwenden Sie RBAC, um der MSI den Zugriff auf eine andere Ressource zuzuweisen.

Nachdem Sie die MSI für eine Azure-Ressource wie einer [Azure-VM](msi-qs-configure-portal-windows-vm.md) oder [Azure-VMSS](msi-qs-configure-portal-windows-vmss.md) aktiviert haben, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die MSI konfiguriert haben.

2. Navigieren Sie zu der gewünschten Ressource, für die Sie die Zugriffssteuerung ändern möchten. In diesem Beispiel wird einem virtuellen Azure-Computer und einer Azure-VM-Skalierungsgruppe Zugriff auf ein Speicherkonto gewährt. Aus diesem Grund navigieren wir zu dem Speicherkonto.

3. Wählen Sie für einen virtuellen Azure-Computer die Seite **Zugriffssteuerung (IAM)** der Ressource und anschließend **+ Hinzufügen** aus. Geben Sie anschließend die **Rolle** an, **weisen Sie den Zugriff auf den virtuellen Computer zu**, und geben Sie das entsprechende **Abonnement** und die **Ressourcengruppe** an, in der sich die Ressource befindet. Unter dem Bereich mit den Suchkriterien sollte die Ressource nun angezeigt werden. Wählen Sie die Ressource und anschließend **Speichern** aus. 

   ![Screenshot von „Zugriffssteuerung (IAM)“](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
   Wählen Sie für eine Azure-VM-Skalierungsgruppe die Seite **Zugriffssteuerung (IAM)** der Ressource und anschließend **+ Hinzufügen** aus. Legen Sie dann die Felder **Rolle** und **Zuweisen des Zugriffs auf** fest. Suchen Sie im Suchkriterienbereich nach Ihrer VM-Skalierungsgruppe. Wählen Sie die Ressource und anschließend **Speichern** aus.
   
   ![Screenshot von „Zugriffssteuerung (IAM)“](./media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-before.png)  

4. Sie sind zur Hauptseite **Zugriffssteuerung (IAM)** zurückgekehrt, auf der ein neuer Eintrag für die MSI der Ressource angezeigt wird.

    Virtueller Azure-Computer:

   ![Screenshot von „Zugriffssteuerung (IAM)“](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

    Azure-VM-Skalierungsgruppe:

    ![Screenshot von „Zugriffssteuerung (IAM)“](./media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-after.png)

## <a name="troubleshooting"></a>Problembehandlung

Wenn die MSI für die Ressource nicht in der Liste der verfügbaren Identitäten angezeigt wird, stellen Sie sicher, dass die MSI ordnungsgemäß aktiviert wurde. In unserem Fall können wir zurück zum virtuellen Azure-Computer wechseln und Folgendes überprüfen:

- Überprüfen Sie auf der Seite **Konfiguration**, ob der Wert für **MSI enabled** (MSI aktiviert) auf **Ja** festgelegt ist.
- Zeigen Sie die Seite **Erweiterungen** an, und überprüfen Sie, ob die MSI-Erweiterung erfolgreich bereitgestellt wurde (die Seite **Erweiterungen** ist nicht für eine Azure-VM-Skalierungsgruppe verfügbar).

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut für die Ressource bereitstellen oder den Fehler bei der Bereitstellung beheben.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Informationen zum Aktivieren der MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer Azure-VM-MSI (Managed Service Identity) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md).
- Informationen zum Aktivieren der MSI in einer Azure-VM-Skalierungsgruppe finden Sie unter [Konfigurieren einer Azure-VM-MSI (Managed Service Identity) über das Azure-Portal](msi-qs-configure-portal-windows-vmss.md).


