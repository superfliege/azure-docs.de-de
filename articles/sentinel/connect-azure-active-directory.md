---
title: Sammeln von Azure AD-Daten in der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Active Directory-Daten in Azure Sentinel sammeln.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: 315b18feb74862bbeca6ff8265ee003fbad48595
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242310"
---
# <a name="collect-data-from-azure-active-directory"></a>Sammeln von Daten ais Azure Active Directory

> [!IMPORTANT]
> Azure Sentinel ist aktuell als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit Azure Sentinel können Sie Daten aus [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) sammeln und in Azure Sentinel streamen. Sie können wählen, ob Sie [Anmeldeprotokolle](../active-directory/reports-monitoring/concept-sign-ins.md) und [Auditprotokolle](../active-directory/reports-monitoring/concept-audit-logs.md) streamen möchten.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie Anmeldedaten aus Active Directory exportieren möchten, müssen Sie über eine Azure AD P1- oder P2-Lizenz verfügen.

- Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen für den Mandanten, von dem aus Sie die Protokolle streamen möchten.


## <a name="connect-to-azure-ad"></a>Stellen Sie eine Verbindung mit Azure AD her.

1. Wählen Sie in Azure Sentinel **Datenerfassung** aus, und klicken Sie dann auf die Kachel **Azure Active Directory**.

2. Klicken Sie neben den Protokollen, die Sie in Azure Sentinel streamen möchten, auf **Verbinden**.






## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie Azure AD mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [ Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
