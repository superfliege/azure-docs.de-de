---
title: "Konfigurieren einer vom Benutzer zugewiesenen MSI für eine Azure-VM mit einem Azure SDK"
description: "Ausführliche Anweisungen für die Konfiguration einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) für eine Azure-VM unter Verwendung eines Azure SDK."
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
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 097304162b85599acd1f4591091f986a646ebc2a
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-sdk"></a>Konfigurieren einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) für eine VM mit einem Azure SDK

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Die verwaltete Dienstidentität (Managed Service Identity, MSI) stellt Azure-Dienste mit einer verwalteten Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie mithilfe eines Azure SDK eine vom Benutzer zugewiesene MSI für eine Azure-VM aktivieren und entfernen können.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="azure-sdks-with-msi-support"></a>Azure SDKs mit MSI-Unterstützung 

Azure unterstützt mit einer Reihe von [Azure SDKs](https://azure.microsoft.com/downloads) mehrere Programmierplattformen. Mehrere davon wurden aktualisiert, um MSI zu unterstützen, und enthalten entsprechende Beispiele, um die Nutzung zu demonstrieren. Diese Liste wird jeweils aktualisiert, wenn weitere Unterstützung hinzugefügt wird:

| SDK | Beispiel |
| --- | ------ | 
| Python | [Create a VM with MSI authentication enabled](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) (Erstellen eines virtuellen Computers mit aktivierter MSI-Authentifizierung) |
| Ruby   | [Create Azure virtual machines with Managed Service Identity using Ruby](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) (Erstellen virtueller Azure-Computer mit verwalteter Dienstidentität mithilfe von Ruby) |

## <a name="next-steps"></a>Nächste Schritte

- In den verwandten Artikeln unter „Konfigurieren einer MSI für ein Azure-VM“ erfahren Sie, wie Sie eine vom Benutzer zugewiesene MSI auf einer Azure-VM konfigurieren.

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.
