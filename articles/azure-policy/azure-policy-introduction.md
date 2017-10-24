---
title: "Übersicht über Azure Policy | Microsoft-Dokumentation"
description: "Azure Policy ist ein Dienst in Azure, mit dem Sie Richtliniendefinitionen in Ihrer Azure-Umgebung erstellen, zuweisen und verwalten können."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 10/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: 01b0915cdf37ddc00a4e6a38c99ce7f6f8c4f9c9
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-policy"></a>Was ist Azure Policy?

IT-Governance schafft Transparenz zwischen Geschäftszielen und IT-Projekten. Eine gute IT-Governance zeichnet sich durch die Planung von Initiativen und das Setzen von Prioritäten auf strategischer Ebene aus. Tritt in Ihrem Unternehmen eine erhebliche Anzahl von IT-Problemen auf, die niemals lösbar zu sein scheinen, können Sie diese durch die Implementierung von Richtlinien besser bewältigen und letztlich verhindern. An dieser Stelle kommt Azure Policy ins Spiel.

Azure Policy ist ein Dienst in Azure, mit dem Sie Richtliniendefinitionen erstellen, zuweisen und verwalten können. Richtliniendefinitionen erzwingen unterschiedliche Regeln und Aktionen für Ihre Ressourcen, damit diese Ressourcen stets konform mit Ihren Unternehmensstandards und Vereinbarungen zum Service Level bleiben. Dazu führen sie eine Auswertung Ihrer Ressourcen durch, um zu prüfen, welche Ressourcen nicht den von Ihnen festgelegten Richtliniendefinitionen entsprechen.

## <a name="policy-definition"></a>Richtliniendefinition

Jede Richtliniendefinition enthält Bedingungen, unter denen sie erzwungen wird, und eine begleitende Aktion, die durchgeführt wird, wenn die Bedingungen erfüllt werden.

In Azure Policy bieten wir einige integrierte Richtlinien an, die Ihnen standardmäßig zur Verfügung stehen. Beispiel:

- **SQL Server 12.0 erfordern**: Diese Richtliniendefinition enthält Bedingungen bzw. Regeln, um sicherzustellen, dass alle SQL Server-Instanzen die Version 12.0 aufweisen. Die zugehörige Aktion besteht darin, alle Server abzulehnen, die nicht diese Kriterien erfüllen.
- **Zulässige Speicherkonto-SKUs**: Diese Richtliniendefinition enthält eine Reihe von Bedingungen bzw. Regeln, die festlegen, ob sich ein Speicherkonto, das bereitgestellt wird, in einem Bereich von SKU-Größen befindet. Die zugehörige Aktion besteht darin, alle Server abzulehnen, die nicht der definierten Gruppe von SKU-Größen entsprechen.
- **Zulässiger Ressourcentyp**: Diese Richtliniendefinition enthält eine Reihe von Bedingungen bzw. Regeln, um die Ressourcentypen festzulegen, die Ihre Organisation bereitstellen kann. Die zugehörige Aktion besteht darin, alle Ressourcen abzulehnen, die nicht Teil dieser Liste sind.

Weitere Informationen zu den Strukturen von Richtliniendefinitionen finden Sie im Artikel [Struktur von Richtliniendefinitionen](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Richtlinienzuweisung
Eine Richtlinienzuweisung ist eine zugewiesene Richtliniendefinition, die innerhalb eines bestimmten Bereichs angewendet werden soll. Ein solcher Bereich kann sich von einer Verwaltungsgruppe bis hin zu einer Ressourcengruppe erstrecken.

Weitere Informationen zum Festlegen von Richtliniendefinitionen und -zuweisungen finden Sie unter [Übersicht über Ressourcenrichtlinien](../azure-resource-manager/resource-manager-policy.md).

## <a name="policy-parameters"></a>Richtlinienparameter
Richtlinienparameter vereinfachen die Richtlinienverwaltung für Sie, da sie die Anzahl der Richtliniendefinitionen, die Sie erstellen müssen, reduzieren. Beim Erstellen einer Richtliniendefinition können Sie Parameter definieren, damit sie allgemeingültiger ist. Diese Richtliniendefinition können Sie dann auch für andere Szenarien verwenden, indem Sie beim Zuweisen der Richtlinie andere Werte (etwa eine Gruppe von Standorten für ein Abonnement) übergeben.

Parameter werden beim Erstellen einer Richtliniendefinition definiert bzw. erstellt. Bei der Definition eines Parameters wird ein Name und optional ein Wert vergeben. So können Sie beispielsweise für einen Standort einen Parameter einer Richtlinie definieren und bei der Zuweisung einer Richtlinie dann unterschiedliche Werte wie *EastUS* oder *WestUS* verwenden.

Weitere Informationen zu Richtlinienparametern finden Sie unter [Übersicht über Ressourcenrichtlinien – Parameter](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Initiativdefinition
Eine Initiativdefinition ist eine Auflistung von Richtliniendefinitionen, die auf das Erreichen eines einzigen übergeordneten Ziels ausgerichtet sind. Beispielsweise können Sie eine Initiative mit dem Titel **Überwachung im Azure Security Center aktivieren** mit dem Ziel erstellen, alle vorhandenen Sicherheitsempfehlungen in Ihrem Azure Security Center zu überwachen.

Im Rahmen dieser Initiative würden Sie Richtliniendefinitionen wie etwa Folgende haben:

1. **Unverschlüsselte SQL-Datenbank im Security Center überwachen** – Zum Überwachen von unverschlüsselten SQL-Datenbanken und Servern.
2. **Betriebssystem-Sicherheitsrisiken im Security Center überwachen** – Zum Überwachen von Servern, die nicht die konfigurierte Baseline erfüllen.
3. **Fehlendes Endpoint Protection im Security Center überwachen** – Zum Überwachen von Servern ohne installierten Endpoint Protection-Agent.

## <a name="initiative-assignment"></a>Initiativzuweisung
Bei einer Initiativzuweisung handelt es sich ähnlich wie bei einer Richtlinienzuweisung um eine Initiativdefinition, die einem bestimmten Bereich zugewiesen ist. Initiativzuweisungen reduzieren den Bedarf dafür, für jeden Bereich mehrere Initiativdefinitionen zu definieren. Ein solcher Bereich kann sich auch von einer Verwaltungsgruppe bis hin zu einer Ressourcengruppe erstrecken.

Die Initiative **Überwachung im Azure Security Center aktivieren** aus dem vorhergehenden Beispiel kann verschiedenen Bereichen zugeordnet werden. So kann z.B. eine Zuweisung **subscriptionA** zugeordnet werden, während eine andere Zuordnung **subscriptionB** zugeordnet werden kann.

## <a name="initiative-parameters"></a>Initiativparameter
Genau wie Richtlinienparameter vereinfachen auch Initiativparameter die Initiativverwaltung, indem sie die Redundanz verringern. Bei Initiativparameter handelt es sich im Wesentlichen um die Liste der Parameter, die von den Richtliniendefinitionen innerhalb der Initiative verwendet werden.

Stellen Sie sich beispielsweise ein Szenario vor, in dem Sie eine Initiativdefinition – **initiativeC** – mit zwei Richtliniendefinitionen haben. Jede Richtliniendefinition weist dabei einen definierten Parameter auf:

|Richtlinie  |Name des Parameters     |Typ des Parameters  |Hinweis                                                                                                |
|--------|----------------------|-------|----------------------------------------------------------------------------------------------------------------|
|policyA |allowedLocations      |array  |Dieser Parameter erwartet eine Liste von Zeichenfolgen als Wert, da der Parametertyp als Array definiert wurde. |
|policyB |allowedSingleLocation |string |Dieser Parameter erwartet ein Wort als Wert, da der Parametertyp als Zeichenfolge definiert wurde.          |

In diesem Szenario haben Sie bei der Definition der Initiativparameter für **initiativeC** drei Optionen:

1. Nutzen Sie die Parameter der Richtliniendefinitionen innerhalb dieser Initiative: In diesem Beispiel werden *allowedLocations* und *allowedSingleLocation* zu Initiativparametern für **initiativeC**.
2. Geben Sie Werte für die Parameter der Richtliniendefinitionen innerhalb dieser Initiativdefinition ein. In diesem Beispiel können Sie eine Liste von Standorten für den **Parameter von policyA – allowedLocations** und den **Parameter von policyB – allowedSingleLocation** zur Verfügung stellen.
Bei der Zuweisung dieser Initiative können Sie auch Werte bereitstellen.
3. Geben Sie eine Liste von *Wertoptionen* an, die bei der Zuweisung dieser Initiative verwendet werden können. Damit dürfen die von den Richtliniendefinitionen geerbten Parameter innerhalb der Initiative bei der Zuweisung dieser Initiative nur Werte aus der angegebenen Liste enthalten.

Wenn Ihre angegebene Liste mit Wertoptionen beim Erstellen der Initiativdefinition die Werte *EastUS*, *WestUS*, *CentralUS* und *WestEurope* aufweist, kann bei der Initiativzuweisung kein anderer Wert wie *Southeast Asia* angegeben werden, da dieser nicht Teil der Liste ist.

## <a name="recommendations-for-managing-policies"></a>Empfehlungen für die Verwaltung von Richtlinien

Im Folgenden werden einige Hinweise zum Erstellen und Verwalten von Richtliniendefinitionen und -zuweisungen genannt, die Sie berücksichtigen sollten:

- Bei der Erstellung von Richtliniendefinitionen in Ihrer Umgebung wird empfohlen, anstelle eines Ablehnungseffekts mit einem Überwachungseffekt zu beginnen, um die Auswirkungen Ihrer Richtliniendefinition in Ihrer Umgebung nachzuverfolgen. Wenn Sie bereits Skripts zur automatischen Skalierung Ihrer Anwendungen eingerichtet haben, kann ein Ablehnungseffekt die von Ihnen bereits eingerichteten Automatisierungsaufgaben verhindern.
- Es ist wichtig, beim Erstellen von Definitionen und Zuweisungen die Organisationshierarchien zu berücksichtigen. Daher wird empfohlen, Definitionen auf einer übergeordneten Ebene wie etwa der Verwaltungsgruppen- oder Abonnementebene zu erstellen und der nächsten untergeordneten Ebene zuzuweisen. Wenn Sie z.B. eine Richtliniendefinition auf Verwaltungsgruppenebene erstellen, kann eine Richtlinienzuweisung dieser Definition bis auf Abonnementebene heruntergebrochen werden.
- Wir empfehlen, den Tarif „Standard“ zu verwenden, damit Sie einen besseren Überblick über den Konformitätszustand Ihrer Umgebung erhalten.
- Es wird empfohlen, anstelle von Richtliniendefinitionen immer Initiativdefinitionen zu verwenden, auch wenn Sie nur eine Richtlinie im Auge haben. Wenn Sie z.B. die Richtliniendefinition *policyDefA* unter der Initiativdefinition *initiativeDefC* erstellen und sich entschließen, eine andere Richtliniendefinition mit Zielen wie denen von *policyDefA* zu erstellen, können Sie sie einfach unter *initiativeDefC* hinzufügen und auf diese Weise besser nachverfolgen.

   Beachten Sie, dass neue Richtliniendefinitionen, die der Initiativdefinition hinzugefügt werden, automatisch unter die Initiativzuweisung(en) dieser Initiativdefinition fallen, nachdem Sie eine Initiativzuweisung über eine Initiativdefinition erstellt haben. Wenn jedoch ein neuer Parameter in die neue Richtliniendefinition eingeschlossen wird, müssen Sie die Initiativdefinition und -zuweisungen entsprechend bearbeiten.

## <a name="next-steps"></a>Nächste Schritte:
Nachdem Sie nun einen Überblick über Azure Policy erhalten und einige Schlüsselbegriffe kennengelernt haben, die wir eingeführt haben, werden nun die nächsten empfohlenen Schritte vorgestellt:

- [Zuweisen einer Richtliniendefinition](./assign-policy-definition.md)
- [Zuweisen einer Richtliniendefinition mit der Azure CLI](./assign-policy-definition-cli.md)
- [Zuweisen einer Richtliniendefinition mit PowerShell](./assign-policy-definition-ps.md)
