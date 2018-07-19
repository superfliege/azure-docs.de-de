---
title: Zuweisen von MSI-Zugriff zu einer Azure-Ressource über das Azure-Portal
description: Ausführliche Anweisungen für das Zuweisen von MSI zu einer Ressource und das Zuweisen des Zugriffs für eine andere Ressource über das Azure-Portal.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2af83762fdcd3302f2af9b08585febab42387d79
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035981"
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Zuweisen des MSI-Zugriffs (MSI = Managed Service Identity, verwaltete Dienstidentität) zu einer Ressource über das Azure-Portal

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Nachdem Sie eine Azure-Ressource mit einer MSI konfiguriert haben, können Sie der MSI genau wie bei allen anderen Sicherheitsprinzipalen den Zugriff auf eine andere Ressource erteilen. In diesem Artikel wird gezeigt, wie Sie der MSI eines virtuellen Azure-Computers über das Azure-Portal den Zugriff auf ein Azure-Speicherkonto gewähren.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Verwenden Sie RBAC, um der MSI den Zugriff auf eine andere Ressource zuzuweisen.

Nachdem Sie MSI für eine Azure-Ressource aktiviert haben, [zum Beispiel für einen virtuellen Azure-Computer](msi-qs-configure-portal-windows-vm.md), gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die MSI konfiguriert haben.

2. Navigieren Sie zu der gewünschten Ressource, für die Sie die Zugriffssteuerung ändern möchten. In diesem Beispiel wird einem virtuellen Azure-Computer der Zugriff auf ein Speicherkonto gewährt. Aus diesem Grund navigieren wir zu dem Speicherkonto.

3. Wählen Sie die Seite **Zugriffssteuerung (IAM)** der Ressource und anschließend **+ Hinzufügen** aus. Geben Sie anschließend die **Rolle** an, **weisen Sie den Zugriff auf den virtuellen Computer zu**, und geben Sie das entsprechende **Abonnement** und die **Ressourcengruppe** an, in der sich die Ressource befindet. Unter dem Bereich mit den Suchkriterien sollte die Ressource nun angezeigt werden. Wählen Sie die Ressource und anschließend **Speichern** aus. 

   ![Screenshot von „Zugriffssteuerung (IAM)“](../managed-service-identity/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. Sie sind zur Hauptseite **Zugriffssteuerung (IAM)** zurückgekehrt, auf der ein neuer Eintrag für die MSI der Ressource angezeigt wird. In diesem Beispiel wurde dem virtuellen Computer „SimpleWinVM“ aus der Demoressourcengruppe der Zugriff **Mitwirkender** auf das Speicherkonto gewährt.

   ![Screenshot von „Zugriffssteuerung (IAM)“](../managed-service-identity/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Problembehandlung

Wenn die MSI für die Ressource nicht in der Liste der verfügbaren Identitäten angezeigt wird, stellen Sie sicher, dass die MSI ordnungsgemäß aktiviert wurde. In unserem Fall können wir zurück zum virtuellen Azure-Computer wechseln und Folgendes überprüfen:

- Überprüfen Sie auf der Seite **Konfiguration**, ob der Wert für **MSI enabled** (MSI aktiviert) auf **Ja** festgelegt ist.
- Stellen Sie sicher, dass die MSI-Erweiterung auf der Seite **Erweiterungen** erfolgreich bereitgestellt wurde.

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut für die Ressource bereitstellen oder den Fehler bei der Bereitstellung beheben.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Informationen zum Aktivieren von MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer Azure-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md).


