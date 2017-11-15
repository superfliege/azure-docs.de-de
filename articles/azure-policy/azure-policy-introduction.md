---
title: "Übersicht über Azure Policy | Microsoft-Dokumentation"
description: "Azure Policy ist ein Dienst in Azure, mit dem Sie Richtliniendefinitionen in Ihrer Azure-Umgebung erstellen, zuweisen und verwalten können."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 11/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: b784c79166eb614988d84b4553bb95e4d1e457af
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="what-is-azure-policy"></a>Was ist Azure Policy?

IT-Governance schafft Transparenz zwischen Geschäftszielen und IT-Projekten. Eine gute IT-Governance zeichnet sich durch die Planung von Initiativen und das Setzen von Prioritäten auf strategischer Ebene aus. Tritt in Ihrem Unternehmen eine erhebliche Anzahl von IT-Problemen auf, die anscheinend nie gelöst werden? Mit dem Implementieren von Richtlinien können Sie besser mit Problemen umgehen und sie verhindern. Beim Implementieren von Richtlinien kommt Azure Policy ins Spiel.

Azure Policy ist ein Dienst in Azure, mit dem Sie Richtliniendefinitionen erstellen, zuweisen und verwalten können. Richtliniendefinitionen erzwingen unterschiedliche Regeln und Aktionen für Ihre Ressourcen, damit diese Ressourcen stets konform mit Ihren Unternehmensstandards und Vereinbarungen zum Service Level bleiben. Azure Policy führt eine Bewertung Ihrer Ressourcen durch, um die herauszufinden, die nicht mit Ihren Richtliniendefinitionen kompatibel sind. Sie können beispielsweise in einer Richtlinie festlegen, dass nur bestimmte Typen virtueller Computer zulässig sind. Eine andere könnte fordern, dass alle Ressourcen ein bestimmtes Tag aufweisen. Diese Richtlinien werden beim Erstellen und Aktualisieren von Ressourcen ausgewertet.

## <a name="how-is-it-different-from-rbac"></a>Worin unterscheidet sich dies von der rollenbasierten Zugriffssteuerung (RBAC)?

Zwischen einer Richtlinie und der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) gibt es einige entscheidende Unterschiede. Bei RBAC stehen Benutzeraktionen in verschiedenen Bereichen im Mittelpunkt. Beispiel: Sie werden der Rolle „Mitwirkender“ für eine Ressourcengruppe in einem bestimmten Bereich hinzugefügt. Die Rolle erlaubt Ihnen, Änderungen an der Ressourcengruppe vorzunehmen. Bei einer Richtlinie stehen Ressourceneigenschaften während der Bereitstellung und für bereits bestehende Ressourcen im Mittelpunkt. Sie können beispielsweise über Richtlinien steuern, welche Ressourcentypen bereitgestellt werden können. Oder Sie können die Standorte einschränken, an denen die Ressourcen bereitgestellt werden können. Im Gegensatz zur RBAC stellen Richtlinien ein Standardsystem für das Zulassen und explizite Verweigern dar.

Zum Verwenden von Richtlinien müssen Sie über die RBAC authentifiziert sein. Ihr Konto benötigt insbesondere Folgendes:

- Die Berechtigung `Microsoft.Authorization/policydefinitions/write` zum Definieren einer Richtlinie.
- Die Berechtigung `Microsoft.Authorization/policyassignments/write` zum Zuweisen einer Richtlinie.

Diese Berechtigungen sind in der Rolle **Mitwirkender** nicht enthalten.


## <a name="policy-definition"></a>Richtliniendefinition

Jede Richtliniendefinition hat Bedingungen, unter denen sie erzwungen wird. Außerdem gehört zu ihr eine begleitende Aktion, die stattfindet, wenn die Bedingungen erfüllt sind.

In Azure Policy bieten wir einige integrierte Richtlinien an, die Ihnen standardmäßig zur Verfügung stehen. Beispiel:

- **SQL Server 12.0 erfordern**: Diese Richtliniendefinition enthält Bedingungen bzw. Regeln, um sicherzustellen, dass alle SQL Server-Instanzen die Version 12.0 aufweisen. Die zugehörige Aktion besteht darin, alle Server abzulehnen, die nicht diese Kriterien erfüllen.
- **Zulässige Speicherkonto-SKUs**: Diese Richtliniendefinition enthält eine Reihe von Bedingungen bzw. Regeln, die festlegen, ob sich ein Speicherkonto, das bereitgestellt wird, in einem Bereich von SKU-Größen befindet. Die zugehörige Aktion besteht darin, alle Server abzulehnen, die nicht der definierten Gruppe von SKU-Größen entsprechen.
- **Zulässiger Ressourcentyp**: Diese Richtliniendefinition enthält eine Reihe von Bedingungen bzw. Regeln, um die Ressourcentypen festzulegen, die Ihre Organisation bereitstellen kann. Die zugehörige Aktion besteht darin, alle Ressourcen abzulehnen, die nicht Teil dieser Liste sind.
- **Zulässige Speicherorte**: Mit dieser Richtlinie können Sie die Speicherorte einschränken, die Ihre Organisation beim Bereitstellen von Ressourcen angeben kann. Mit dieser Aktion werden Ihre Geokonformitätsanforderungen erzwungen.
- **Zulässige VM-SKUs**: Mit dieser Richtlinie können Sie eine Gruppe von VM-SKUs angeben, die Ihre Organisation bereitstellen kann.
- **Tag und seinen Standardwert anwenden**: Diese Richtlinie wendet ein erforderliches Tag und seinen Standardwert an, falls es nicht vom Benutzer angegeben wird.
- **Tag und seinen Wert erzwingen**: Diese Richtlinie setzt ein erforderliches Tag und seinen Wert bei einer Ressource durch.
- **Nicht zulässige Ressourcentypen**: Mit dieser Richtlinie können Sie die Ressourcentypen angeben, die Ihre Organisation nicht bereitstellen kann.

Sie können diese Richtlinien über das Azure-Portal, PowerShell oder die Azure CLI zuweisen.

Weitere Informationen zu den Strukturen von Richtliniendefinitionen finden Sie im Artikel [Struktur von Richtliniendefinitionen](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Richtlinienzuweisung

Eine Richtlinienzuweisung ist eine zugewiesene Richtliniendefinition, die innerhalb eines bestimmten Bereichs angewendet werden soll. Ein solcher Bereich kann sich von einer Verwaltungsgruppe bis hin zu einer Ressourcengruppe erstrecken. Der Begriff *Bereich* bezieht sich auf alle Ressourcengruppen, Abonnements oder Verwaltungsgruppen, denen die Richtliniendefinition zugewiesen ist. Richtlinienzuordnungen werden von allen untergeordneten Ressourcen geerbt. Wenn also eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe. Sie können jedoch einen Unterbereich von der Richtlinienzuordnung ausschließen. Im Abonnementbereich können Sie beispielsweise eine Richtlinie zuweisen, die die Erstellung von Netzwerkressourcen verhindert. Allerdings schließen Sie dann eine Ressourcengruppe innerhalb des Abonnements aus, die für die Netzwerkinfrastruktur vorgesehen ist. Sie gewähren Benutzern Zugriff auf diese Netzwerkressourcengruppe, denen Sie für das Erstellen von Netzwerkressourcen vertrauen.

Weitere Informationen zum Einrichten von Richtliniendefinitionen und Zuweisungen finden Sie unter [Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung](assign-policy-definition.md).

## <a name="policy-parameters"></a>Richtlinienparameter

Richtlinienparameter vereinfachen die Richtlinienverwaltung für Sie, da sie die Anzahl der Richtliniendefinitionen, die Sie erstellen müssen, reduzieren. Beim Erstellen einer Richtliniendefinition können Sie Parameter definieren, damit sie allgemeingültiger ist. Anschließend können Sie diese Richtliniendefinition für verschiedene Szenarien wiederverwenden. Dazu übergeben Sie unterschiedliche Werte beim Zuweisen der Richtliniendefinition. Geben Sie z.B. eine Gruppe von Speicherorten für ein Abonnement an.

Parameter werden beim Erstellen einer Richtliniendefinition definiert bzw. erstellt. Bei der Definition eines Parameters wird ein Name und optional ein Wert vergeben. Sie können z.B. einen Parameter für eine Richtlinie mit dem Titel *location* definieren. Dann können Sie ihm beim Zuweisen einer Richtlinie unterschiedliche Werte wie z.B. *EastUS* oder *WestUS* geben.

<!--
Next link should point to new Concept page for Parameters
-->
Weitere Informationen zu Richtlinienparametern finden Sie unter [Übersicht über Ressourcenrichtlinien – Parameter](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Initiativdefinition
Eine Initiativdefinition ist eine Auflistung von Richtliniendefinitionen, die auf das Erreichen eines einzigen übergeordneten Ziels ausgerichtet sind. Initiativdefinitionen vereinfachen das Verwalten und Zuweisen von Richtliniendefinitionen. Die Vereinfachung besteht im Gruppieren einer Reihe von Richtlinien zu einem einzelnen Element. Beispielsweise können Sie eine Initiative mit dem Titel **Überwachung im Azure Security Center aktivieren** mit dem Ziel erstellen, alle vorhandenen Sicherheitsempfehlungen in Ihrem Azure Security Center zu überwachen.

Im Rahmen dieser Initiative würden Sie Richtliniendefinitionen wie etwa Folgende haben:

1. **Unverschlüsselte SQL-Datenbank im Security Center überwachen** – Zum Überwachen von unverschlüsselten SQL-Datenbanken und Servern.
2. **Betriebssystem-Sicherheitsrisiken im Security Center überwachen** – Zum Überwachen von Servern, die nicht die konfigurierte Baseline erfüllen.
3. **Fehlendes Endpoint Protection im Security Center überwachen** – Zum Überwachen von Servern ohne installierten Endpoint Protection-Agent.

<!--
For more information about initiative definitions, see Initiative Definitions.+ (instead of linking to this, link out to Concept page on Initiative Definitions)
-->

## <a name="initiative-assignment"></a>Initiativzuweisung
Bei einer Initiativzuweisung handelt es sich ähnlich wie bei einer Richtlinienzuweisung um eine Initiativdefinition, die einem bestimmten Bereich zugewiesen ist. Initiativzuweisungen reduzieren den Bedarf dafür, für jeden Bereich mehrere Initiativdefinitionen zu definieren. Ein solcher Bereich kann sich auch von einer Verwaltungsgruppe bis hin zu einer Ressourcengruppe erstrecken.

Die Initiative **Überwachung im Azure Security Center aktivieren** aus dem vorhergehenden Beispiel kann verschiedenen Bereichen zugeordnet werden. Beispielsweise kann eine Zuweisung **subscriptionA** zugewiesen werden. Eine andere kann **subscriptionB** zugewiesen werden.

## <a name="initiative-parameters"></a>Initiativparameter
Genau wie Richtlinienparameter vereinfachen auch Initiativparameter die Initiativverwaltung, indem sie die Redundanz verringern. Bei Initiativparameter handelt es sich im Wesentlichen um die Liste der Parameter, die von den Richtliniendefinitionen innerhalb der Initiative verwendet werden.

Stellen Sie sich beispielsweise ein Szenario vor, in dem Sie eine Initiativdefinition – **initiativeC** – mit zwei Richtliniendefinitionen haben. Jede Richtliniendefinition weist dabei einen definierten Parameter auf:

| Richtlinie | Name des Parameters |Typ des Parameters  |Hinweis |
|---|---|---|---|
| policyA | allowedLocations | array  |Dieser Parameter erwartet eine Liste von Zeichenfolgen als Wert, da der Parametertyp als Array definiert wurde. |
| policyB | allowedSingleLocation |string |Dieser Parameter erwartet ein Wort als Wert, da der Parametertyp als Zeichenfolge definiert wurde. |

In diesem Szenario haben Sie bei der Definition der Initiativparameter für **initiativeC** drei Optionen:

1. Nutzen Sie die Parameter der Richtliniendefinitionen innerhalb dieser Initiative: In diesem Beispiel werden *allowedLocations* und *allowedSingleLocation* zu Initiativparametern für **initiativeC**.
2. Geben Sie Werte für die Parameter der Richtliniendefinitionen innerhalb dieser Initiativdefinition ein. In diesem Beispiel können Sie eine Liste von Standorten für den **Parameter von policyA – allowedLocations** und den **Parameter von policyB – allowedSingleLocation** zur Verfügung stellen.
Bei der Zuweisung dieser Initiative können Sie auch Werte bereitstellen.
3. Geben Sie eine Liste von *Wertoptionen* an, die bei der Zuweisung dieser Initiative verwendet werden können. Damit dürfen die von den Richtliniendefinitionen geerbten Parameter innerhalb der Initiative bei der Zuweisung dieser Initiative nur Werte aus der angegebenen Liste enthalten.

Sie können z.B. eine Liste der Wertoptionen in einer Initiativdefinition erstellen, die *USA, Osten*, *USA, Westen*, *USA, Mitte* und *Europa, Westen* enthält. Dann können Sie während der Initiativzuweisung keinen anderen Wert wie z.B. *Asien, Südosten* eingeben, da er nicht Teil der Liste ist.

## <a name="recommendations-for-managing-policies"></a>Empfehlungen für die Verwaltung von Richtlinien

Im Folgenden werden einige Hinweise zum Erstellen und Verwalten von Richtliniendefinitionen und -zuweisungen genannt, die Sie berücksichtigen sollten:

- Bei der Erstellung von Richtliniendefinitionen in Ihrer Umgebung sollten Sie anstatt mit einem Ablehnungseffekt mit einem Überwachungseffekt beginnen, um die Auswirkungen Ihrer Richtliniendefinition auf die Ressourcen in Ihrer Umgebung nachzuverfolgen. Wenn Sie bereits Skripts zur automatischen Skalierung Ihrer Anwendungen eingerichtet haben, kann ein Ablehnungseffekt die von Ihnen bereits eingerichteten Automatisierungsaufgaben verhindern.
- Es ist wichtig, beim Erstellen von Definitionen und Zuweisungen die Organisationshierarchien zu berücksichtigen. Daher wird empfohlen, Definitionen auf einer übergeordneten Ebene wie etwa der Verwaltungsgruppen- oder Abonnementebene zu erstellen und der nächsten untergeordneten Ebene zuzuweisen. Wenn Sie z.B. eine Richtliniendefinition auf Verwaltungsgruppenebene erstellen, kann eine Richtlinienzuweisung dieser Definition innerhalb dieser Verwaltungsgruppe bis auf Abonnementebene heruntergebrochen werden.
- Wir empfehlen, den Tarif „Standard“ zu verwenden, damit Sie einen besseren Überblick über den Konformitätszustand Ihrer Umgebung erhalten. Weitere Informationen zu Preismodellen und was jedes von ihnen bietet, finden Sie unter [Azure Policy – Preise ](https://azure.microsoft.com/pricing/details/azure-policy).
- Es wird empfohlen, anstelle von Richtliniendefinitionen immer Initiativdefinitionen zu verwenden, auch wenn Sie nur eine Richtlinie im Auge haben. Wenn Sie z.B. die Richtliniendefinition *policyDefA* unter der Initiativdefinition *initiativeDefC* erstellen und sich entschließen, eine andere Richtliniendefinition für *policyDefB* mit Zielen wie denen von *policyDefA* zu erstellen, können Sie sie unter *initiativeDefC* hinzufügen und auf diese Weise besser nachverfolgen.

   Beachten Sie, dass neue Richtliniendefinitionen, die der Initiativdefinition hinzugefügt werden, automatisch unter die Initiativzuweisung(en) dieser Initiativdefinition fallen, nachdem Sie eine Initiativzuweisung über eine Initiativdefinition erstellt haben. Wenn jedoch ein neuer Parameter in die neue Richtliniendefinition eingeschlossen wird, müssen Sie die Initiativdefinition und -zuweisungen entsprechend bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Überblick über Azure Policy erhalten und einige Schlüsselbegriffe kennengelernt haben, die wir eingeführt haben, werden nun die nächsten empfohlenen Schritte vorgestellt:

- [Zuweisen einer Richtliniendefinition](./assign-policy-definition.md)
- [Zuweisen einer Richtliniendefinition mit der Azure CLI](./assign-policy-definition-cli.md)
- [Zuweisen einer Richtliniendefinition mit PowerShell](./assign-policy-definition-ps.md)
