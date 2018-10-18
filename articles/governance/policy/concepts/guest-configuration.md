---
title: Informationen zum Durchführen von Überprüfungen in einem virtuellen Computer mithilfe von Azure Policy
description: Hier erfahren Sie, wie Azure Policy mithilfe von Guest Configuration Einstellungen in einem virtuellen Azure-Computer überprüft.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: ca96aea8f359f1df7da48f84a3317a2d8c7b52e4
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167721"
---
# <a name="understand-azure-policys-guest-configuration"></a>Informationen zu Guest Configuration von Azure Policy

Mit Azure Policy können Sie nicht nur Azure-Ressourcen überprüfen und [korrigieren](../how-to/remediate-resources.md), sondern auch Einstellungen in einem virtuellen Computer überprüfen. Für die Überprüfung verwenden Sie die Erweiterung und den Client Guest Configuration. Mit der Erweiterung werden über den Client Einstellungen wie die Konfiguration des Betriebssystems, die Konfiguration oder das Vorhandensein einer Anwendung, Umgebungseinstellungen und vieles mehr überprüft.

> [!IMPORTANT]
> Derzeit werden mit Guest Configuration nur **integrierte** Richtlinien unterstützt.

## <a name="extension-and-client"></a>Erweiterung und Client

Zur Überprüfung von Einstellungen in einem virtuellen Computer wird eine [VM-Erweiterung](../../../virtual-machines/extensions/overview.md) aktiviert. Mit der Erweiterung werden anwendbare Richtlinienzuweisungen sowie die entsprechende Konfigurationsdefinition heruntergeladen.

### <a name="register-guest-configuration-resource-provider"></a>Registrieren des Guest Configuration-Ressourcenanbieters

Bevor Sie Guest Configuration verwenden können, müssen Sie den Ressourcenanbieter registrieren. Hierfür können Sie das Portal oder PowerShell verwenden.

#### <a name="registration---portal"></a>Registrierung – Portal

Gehen Sie wie folgt vor, um den Ressourcenanbieter für Guest Configuration über das Azure-Portal zu registrieren:

1. Rufen Sie das Azure-Portal auf, und klicken Sie auf **Alle Dienste**. Suchen Sie nach **Abonnements**, und wählen Sie diese Option aus.

1. Suchen Sie nach dem Abonnement, das Sie für Guest Configuration aktivieren möchten, und klicken Sie darauf.

1. Klicken Sie im linken Menü der Seite **Abonnement** auf **Ressourcenanbieter**.

1. Suchen Sie durch Filtern oder Blättern nach **Microsoft.GuestConfiguration**, und klicken Sie dann in derselben Zeile auf **Registrieren**.

#### <a name="registration---powershell"></a>Registrierung – PowerShell

Führen Sie zum Registrieren des Ressourcenanbieters für Guest Configuration über PowerShell den folgenden Befehl aus:

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell
Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Überprüfungstools

Im virtuellen Computer verwendet der Guest Configuration-Client lokale Tools zum Durchführen der Überprüfung.

In der folgenden Tabelle sind die lokalen Tools aufgeführt, die unter den jeweiligen unterstützten Betriebssystemen verwendet werden:

|Betriebssystem|Überprüfungstool|Notizen|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby und Python werden durch die Erweiterung Guest Configuration installiert. |

### <a name="supported-client-types"></a>Unterstützte Clienttypen

In der folgenden Tabelle sind die in Azure-Images unterstützten Betriebssysteme aufgeführt:

|Herausgeber|NAME|Versionen|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|Credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter|
|OpenLogic|CentOS|7.3, 7.4, 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|SUSE|SLES|12 SP3|

> [!IMPORTANT]
> Guest Configuration wird derzeit in benutzerdefinierten VM-Images nicht unterstützt.

### <a name="unsupported-client-types"></a>Nicht unterstützte Clienttypen

In der folgenden Tabelle werden die Betriebssysteme aufgelistet, die nicht unterstützt werden:

|Betriebssystem|Notizen|
|-|-|
|Windows-Client | Clientbetriebssysteme (z.B. Windows 7 und Windows 10) werden nicht unterstützt.
|Windows Server 2016 Nano Server | Nicht unterstützt.|

## <a name="guest-configuration-definition-requirements"></a>Anforderungen an die Guest Configuration-Definition

Für jede mit Guest Configuration durchgeführte Überprüfung werden die beiden Richtliniendefinitionen **DeployIfNotExists** und **AuditIfNotExists** benötigt. **DeployIfNotExists** dient zum Vorbereiten des virtuellen Computers mit dem Guest Configuration-Agent und anderen Komponenten zur Unterstützung der [Überprüfungstools](#validation-tools).

Mit der Richtliniendefinition **DeployIfNotExists** wird Folgendes überprüft und korrigiert:

- Stellen Sie sicher, dass dem virtuellen Computer eine auszuwertende Konfiguration zugewiesen wurde. Wenn derzeit keine Zuweisung vorhanden ist, gehen Sie wie folgt vor, um die Zuweisung abzurufen und den virtuellen Computer vorzubereiten:
  - Authentifizieren Sie sich beim virtuellen Computer mithilfe einer [verwalteten Identität](../../../active-directory/managed-identities-azure-resources/overview.md).
  - Installieren Sie die neueste Version der Erweiterung **Microsoft.GuestConfiguration**.
  - Installieren Sie [Überprüfungstools](#validation-tools) und ggf. Abhängigkeiten.

Sobald die Richtliniendefinition **DeployIfNotExists** konform ist, wird die Richtliniendefinition **AuditIfNotExists** verwendet, um mithilfe der lokalen Überprüfungstools zu ermitteln, ob die zugewiesene Konfiguration konform ist. Die Ergebnisse werden dem Guest Configuration-Client vom Überprüfungstool bereitgestellt. Dieser leitet sie an die Erweiterung Guest Configuration weiter, damit sie über den Guest Configuration-Ressourcenanbieter verfügbar werden.

Azure Policy verwendet die Eigenschaft **complianceStatus** des Guest Configuration-Ressourcenanbieters, um die Konformität im Knoten **Konformität** zu melden. Weitere Informationen finden Sie unter [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).

> [!NOTE]
> Für jede Guest Configuration-Definition müssen die beiden Richtliniendefinitionen **DeployIfNotExists** und **AuditIfNotExists** vorhanden sein.

Alle integrierten Richtlinien für Guest Configuration sind in einer Initiative zum Gruppieren der Definitionen zur Verwendung in Zuweisungen enthalten. Die integrierte Initiative mit dem Namen *[Vorschau]: Überprüfen der Sicherheitseinstellungen für Kennwörter in virtuellen Computern unter Linux und Windows* enthält 18 Richtlinien. Es gibt sechs **DeployIfNotExists**- und **AuditIfNotExists**-Paare für Windows und drei für Linux. Dabei stellt die Logik innerhalb der Definition nur sicher, dass das Zielbetriebssystem anhand der [Richtlinienregel](definition-structure.md#policy-rule)definition ausgewertet wird.

## <a name="next-steps"></a>Nächste Schritte

- Unter [Azure Policy-Beispiele](../samples/index.md) finden Sie Beispiele.
- Befassen Sie sich mit der [Struktur von Azure Policy-Definitionen](definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Entdecken Sie, wie Sie [nicht konforme Ressourcen](../how-to/remediate-resources.md) korrigieren können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/index.md).