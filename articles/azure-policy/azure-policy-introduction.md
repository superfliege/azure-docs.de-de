---
title: Übersicht zu Azure-Richtlinien
description: Azure Policy ist ein Dienst in Azure, mit dem Sie Richtliniendefinitionen in Ihrer Azure-Umgebung erstellen, zuweisen und verwalten können.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 51fd0c625ad7e600d54999ddd86e5e49a7c4f14d
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39249863"
---
# <a name="what-is-azure-policy"></a>Was ist Azure Policy?

Mithilfe von IT-Governance wird sichergestellt, dass Ihre Organisation ihre Ziele basierend auf einer effektiven und effizienten IT-Nutzung erreichen kann. Hierzu werden Ihre Geschäftsziele und die IT-Projekte klar aufeinander abgestimmt.

Tritt in Ihrem Unternehmen eine erhebliche Anzahl von IT-Problemen auf, die anscheinend nie gelöst werden?
Eine gute IT-Governance zeichnet sich durch die Planung von Initiativen und das Setzen von Prioritäten auf strategischer Ebene aus, um Probleme managen und verhindern zu können. An dieser Stelle kommt Azure Policy ins Spiel.

Azure Policy ist ein Dienst in Azure, mit dem Sie Richtlinien erstellen, zuweisen und verwalten können. Mit diesen Richtlinien werden unterschiedliche Regeln und Auswirkungen für Ihre Ressourcen erzwungen, damit diese stets mit Ihren Unternehmensstandards und Vereinbarungen zum Servicelevel konform bleiben. Hierzu führt Azure Policy Auswertungen Ihrer Ressourcen durch und sucht nach Fällen, in denen keine Konformität mit den von Ihnen erstellten Richtlinien besteht. Beispielsweise können Sie eine Richtlinie erstellen, die besagt, dass in Ihrer Umgebung für virtuelle Computer nur eine bestimmte SKU-Größe zulässig ist. Nach der Implementierung dieser Richtlinie wird sie nicht nur für bereits vorhandene Ressourcen ausgewertet, sondern auch, wenn Ressourcen erstellt und aktualisiert werden. Weiter unten in dieser Dokumentation werden noch weitere Details zur Erstellung und Implementierung von Richtlinien mit Azure Policy beschrieben.

> [!IMPORTANT]
> Die Kompatibilitätsprüfung von Azure Policy wird nun unabhängig vom Tarif für alle Zuweisungen bereitgestellt. Sollten die Kompatibilitätsdaten für Ihre Zuweisungen nicht angezeigt werden, vergewissern Sie sich, dass das Abonnement beim Ressourcenanbieter „Microsoft.PolicyInsights“ registriert ist.

## <a name="how-is-it-different-from-rbac"></a>Worin unterscheidet sich dies von der rollenbasierten Zugriffssteuerung (RBAC)?

Zwischen einer Richtlinie und der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) gibt es einige entscheidende Unterschiede. Bei RBAC stehen Benutzeraktionen in verschiedenen Bereichen im Mittelpunkt. Beispiel: Sie werden der Rolle „Mitwirkender“ für eine Ressourcengruppe in einem bestimmten Bereich hinzugefügt. Die Rolle erlaubt Ihnen, Änderungen an der Ressourcengruppe vorzunehmen.
Bei einer Richtlinie stehen Ressourceneigenschaften während der Bereitstellung und für bereits bestehende Ressourcen im Mittelpunkt. Sie können beispielsweise über Richtlinien steuern, welche Ressourcentypen bereitgestellt werden können. Oder Sie können die Standorte einschränken, an denen die Ressourcen bereitgestellt werden können. Im Gegensatz zur RBAC stellen Richtlinien ein Standardsystem für das Zulassen und explizite Verweigern dar.

### <a name="rbac-permissions-in-azure-policy"></a>RBAC-Berechtigungen in Azure Policy

Azure Policy verfügt über Berechtigungen, die als Vorgänge in zwei verschiedenen Ressourcenanbietern dargestellt werden:

- [Microsoft.Authorization](../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsight](../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Mehrere integrierte Rollen weisen verschiedene Berechtigungsebenen für Azure Policy-Ressourcen auf, z.B. **Sicherheitsadministrator**, womit Richtlinienzuweisungen und -definitionen verwaltet, aber keine Konformitätsinformationen angezeigt werden können. Ein weiteres Beispiel ist **Leser**, womit Details zu Richtlinienzuweisungen und -definitionen gelesen, aber keine Änderungen vorgenommen oder Konformitätsinformationen angezeigt werden können. Während die Rolle **Besitzer** über die vollständigen Rechte verfügt, ist **Mitwirkender** nicht mit Azure Policy-Berechtigungen verbunden. Erstellen Sie eine [benutzerdefinierte Rolle](../role-based-access-control/custom-roles.md), um die Berechtigung zum Anzeigen von Policy-Konformitätsdetails zu gewähren.

## <a name="policy-definition"></a>Richtliniendefinition

Die Erstellung und Implementierung einer Richtlinie in Azure Policy beginnt mit dem Erstellen einer Richtliniendefinition. Jede Richtliniendefinition hat Bedingungen, unter denen sie erzwungen wird. Des Weiteren verfügt sie über einen dazugehörigen Effekt, der wirksam wird, wenn die Bedingungen erfüllt sind.

In Azure Policy bieten wir einige integrierte Richtlinien an, die Ihnen standardmäßig zur Verfügung stehen. Beispiel: 

- **SQL Server 12.0 erfordern**: Diese Richtliniendefinition enthält Bedingungen bzw. Regeln, um sicherzustellen, dass alle SQL Server-Instanzen die Version 12.0 aufweisen. Der dazugehörige Effekt besteht in der Ablehnung aller Server, die diese Kriterien nicht erfüllen.
- **Zulässige Speicherkonto-SKUs**: Diese Richtliniendefinition enthält eine Reihe von Bedingungen bzw. Regeln, die festlegen, ob sich ein Speicherkonto, das bereitgestellt wird, in einem Bereich von SKU-Größen befindet. Dies hat die Auswirkung, dass alle Speicherkonten abgelehnt werden, die nicht der definierten Gruppe von SKU-Größen entsprechen.
- **Zulässiger Ressourcentyp**: Diese Richtliniendefinition enthält eine Reihe von Bedingungen bzw. Regeln, um die Ressourcentypen festzulegen, die Ihre Organisation bereitstellen kann. Der dazugehörige Effekt besteht in der Ablehnung aller Ressourcen, die nicht Teil dieser Liste sind.
- **Zulässige Speicherorte**: Mit dieser Richtlinie können Sie die Speicherorte einschränken, die Ihre Organisation beim Bereitstellen von Ressourcen angeben kann. Der dazugehörige Effekt dient zur Erzwingung Ihrer Geokonformitätsanforderungen.
- **Zulässige VM-SKUs**: Mit dieser Richtlinie können Sie eine Gruppe von VM-SKUs angeben, die Ihre Organisation bereitstellen kann.
- **Tag und seinen Standardwert anwenden**: Diese Richtlinie wendet ein erforderliches Tag und seinen Standardwert an, falls es nicht vom Benutzer angegeben wird.
- **Tag und seinen Wert erzwingen**: Diese Richtlinie setzt ein erforderliches Tag und seinen Wert bei einer Ressource durch.
- 
  **Nicht zulässige Ressourcentypen**: Mit dieser Richtlinie können Sie die Ressourcentypen angeben, die Ihre Organisation nicht bereitstellen kann.

Um diese Richtliniendefinitionen implementieren zu können (integrierte und benutzerdefinierte Definitionen), müssen Sie sie zuweisen. Sie können diese Richtlinien über das Azure-Portal, PowerShell oder die Azure CLI zuweisen.

Beachten Sie, dass eine erneute Auswertung einer Richtlinie ca. einmal pro Stunde erfolgt. Dies bedeutet Folgendes: Wenn Sie nach der Implementierung der Richtlinie (Erstellung einer Richtlinienzuweisung) Änderungen vornehmen, wird sie innerhalb einer Stunde für Ihre Ressourcen erneut ausgewertet.

Weitere Informationen zu den Strukturen von Richtliniendefinitionen finden Sie unter [Struktur von Richtliniendefinitionen](policy-definition.md).

## <a name="policy-assignment"></a>Richtlinienzuweisung

Eine Richtlinienzuweisung ist eine zugewiesene Richtliniendefinition, die innerhalb eines bestimmten Bereichs angewendet werden soll. Ein solcher Bereich kann sich von einer [Verwaltungsgruppe](../azure-resource-manager/management-groups-overview.md) bis hin zu einer Ressourcengruppe erstrecken. Der Begriff *Bereich* bezieht sich auf alle Ressourcengruppen, Abonnements oder Verwaltungsgruppen, denen die Richtliniendefinition zugewiesen ist. Richtlinienzuordnungen werden von allen untergeordneten Ressourcen geerbt. Dies bedeutet Folgendes: Wenn eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe. Sie können jedoch einen Unterbereich von der Richtlinienzuordnung ausschließen.

Im Abonnementbereich können Sie beispielsweise eine Richtlinie zuweisen, die die Erstellung von Netzwerkressourcen verhindert. Allerdings schließen Sie dann eine Ressourcengruppe innerhalb des Abonnements aus, die für die Netzwerkinfrastruktur vorgesehen ist. Sie gewähren Benutzern Zugriff auf diese Netzwerkressourcengruppe, denen Sie für das Erstellen von Netzwerkressourcen vertrauen.

In einem anderen Beispiel möchten Sie möglicherweise eine Ressourcentyp-Whitelist-Richtlinie auf der Verwaltungsgruppenebene zuweisen. Außerdem möchten Sie einer untergeordneten Verwaltungsgruppe (oder sogar Abonnements direkt) eine weniger restriktive Richtlinie zuweisen, die mehr Ressourcentypen zulässt. Dieses Beispiel funktioniert jedoch nicht, da es sich bei einer Richtlinie um ein explizites Verbotssystem handelt. Stattdessen müssen Sie die untergeordnete Verwaltungsgruppe oder das Abonnement aus der Richtlinienzuweisung auf der Verwaltungsgruppenebene ausschließen. Anschließend müssen Sie die weniger restriktive Richtlinie auf der Ebene der untergeordneten Verwaltungsgruppe oder des Abonnements zuweisen. Kurz gesagt: Falls eine Ressource aufgrund einer Richtlinie abgelehnt wird, muss zum Zulassen der Ressource die ablehnende Richtlinie geändert werden.

Weitere Informationen zum Einrichten von Richtliniendefinitionen und Zuweisungen finden Sie unter [Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung](assign-policy-definition.md).

## <a name="policy-parameters"></a>Richtlinienparameter

Richtlinienparameter vereinfachen die Richtlinienverwaltung für Sie, da sie die Anzahl der Richtliniendefinitionen, die Sie erstellen müssen, reduzieren. Beim Erstellen einer Richtliniendefinition können Sie Parameter definieren, damit sie allgemeingültiger ist. Anschließend können Sie diese Richtliniendefinition für verschiedene Szenarien wiederverwenden. Dazu übergeben Sie unterschiedliche Werte beim Zuweisen der Richtliniendefinition. Geben Sie z.B. eine Gruppe von Speicherorten für ein Abonnement an.

Parameter werden beim Erstellen einer Richtliniendefinition definiert bzw. erstellt. Bei der Definition eines Parameters wird ein Name und optional ein Wert vergeben. Sie können z.B. einen Parameter für eine Richtlinie mit dem Titel *location* definieren. Dann können Sie ihm beim Zuweisen einer Richtlinie unterschiedliche Werte wie z.B. *EastUS* oder *WestUS* geben.

Weitere Informationen zu Richtlinienparametern finden Sie unter [Übersicht über Ressourcenrichtlinien – Parameter](policy-definition.md#parameters).

## <a name="initiative-definition"></a>Initiativdefinition

Eine Initiativdefinition ist eine Auflistung von Richtliniendefinitionen, die auf das Erreichen eines einzigen übergeordneten Ziels ausgerichtet sind. Initiativdefinitionen vereinfachen das Verwalten und Zuweisen von Richtliniendefinitionen. Die Vereinfachung besteht im Gruppieren einer Reihe von Richtlinien zu einem einzelnen Element. Beispielsweise können Sie eine Initiative mit dem Titel **Überwachung im Azure Security Center aktivieren** mit dem Ziel erstellen, alle vorhandenen Sicherheitsempfehlungen in Ihrem Azure Security Center zu überwachen.

Im Rahmen dieser Initiative würden Sie Richtliniendefinitionen wie etwa Folgende haben:

- **Unverschlüsselte SQL-Datenbank im Security Center überwachen** – Zum Überwachen von unverschlüsselten SQL-Datenbanken und Servern.
- **Betriebssystem-Sicherheitsrisiken im Security Center überwachen** – Zum Überwachen von Servern, die nicht die konfigurierte Baseline erfüllen.
- **Fehlendes Endpoint Protection im Security Center überwachen** – Zum Überwachen von Servern ohne installierten Endpoint Protection-Agent.

## <a name="initiative-assignment"></a>Initiativzuweisung

Bei einer Initiativzuweisung handelt es sich ähnlich wie bei einer Richtlinienzuweisung um eine Initiativdefinition, die einem bestimmten Bereich zugewiesen ist. Initiativzuweisungen reduzieren den Bedarf dafür, für jeden Bereich mehrere Initiativdefinitionen zu definieren. Ein solcher Bereich kann sich auch von einer Verwaltungsgruppe bis hin zu einer Ressourcengruppe erstrecken.

Die Initiative **Überwachung im Azure Security Center aktivieren** aus dem vorhergehenden Beispiel kann verschiedenen Bereichen zugeordnet werden. Beispielsweise kann eine Zuweisung **subscriptionA** zugewiesen werden.
Eine andere kann **subscriptionB** zugewiesen werden.

## <a name="initiative-parameters"></a>Initiativparameter

Genau wie Richtlinienparameter vereinfachen auch Initiativparameter die Initiativverwaltung, indem sie die Redundanz verringern. Bei Initiativparameter handelt es sich im Wesentlichen um die Liste der Parameter, die von den Richtliniendefinitionen innerhalb der Initiative verwendet werden.

Stellen Sie sich beispielsweise ein Szenario vor, in dem Sie die Initiativdefinition **initiativeC** mit den Richtliniendefinitionen **policyA** und **policyB** verwenden, die jeweils einen anderen Parametertyp erwarten:

| Richtlinie | Name des Parameters |Typ des Parameters  |Hinweis |
|---|---|---|---|
| policyA | allowedLocations | Array  |Dieser Parameter erwartet eine Liste von Zeichenfolgen als Wert, da der Parametertyp als Array definiert wurde. |
| policyB | allowedSingleLocation |Zeichenfolge |Dieser Parameter erwartet ein Wort als Wert, da der Parametertyp als Zeichenfolge definiert wurde. |

In diesem Szenario haben Sie bei der Definition der Initiativparameter für **initiativeC** drei Optionen:

- Nutzen Sie die Parameter der Richtliniendefinitionen innerhalb dieser Initiative: In diesem Beispiel werden *allowedLocations* und *allowedSingleLocation* zu Initiativparametern für **initiativeC**.
- Geben Sie Werte für die Parameter der Richtliniendefinitionen innerhalb dieser Initiativdefinition ein. In diesem Beispiel können Sie eine Liste von Standorten für den **Parameter von policyA – allowedLocations** und den **Parameter von policyB – allowedSingleLocation** zur Verfügung stellen. Bei der Zuweisung dieser Initiative können Sie auch Werte bereitstellen.
- Geben Sie eine Liste von *Wertoptionen* an, die bei der Zuweisung dieser Initiative verwendet werden können. Damit dürfen die von den Richtliniendefinitionen geerbten Parameter innerhalb der Initiative bei der Zuweisung dieser Initiative nur Werte aus der angegebenen Liste enthalten.

Sie können z.B. eine Liste der Wertoptionen in einer Initiativdefinition erstellen, die *USA, Osten*, *USA, Westen*, *USA, Mitte* und *Europa, Westen* enthält. Dann können Sie während der Initiativzuweisung keinen anderen Wert wie z.B. *Asien, Südosten* eingeben, da er nicht Teil der Liste ist.

## <a name="maximum-count-of-policy-objects"></a>Maximale Anzahl von Richtlinienobjekten

Für jeden Objekttyp für Azure Policy gibt es eine maximale Anzahl. Ein _Scope_-Eintrag (Bereich) gilt entweder für das Abonnement oder die Verwaltungsgruppe.

| Hierbei gilt: | Was | Maximale Anzahl |
|---|---|---|
| Bereich | Richtliniendefinitionen | 250 |
| Bereich | Initiativdefinitionen | 100 |
| Tenant | Initiativdefinitionen | 1000 |
| Bereich | Richtlinienzuweisung | 100 |
| Richtliniendefinition | Parameter | 20 |
| Initiativdefinition | Richtlinien | 100 |
| Initiativdefinition | Parameter | 100 |
| Richtlinienzuweisung | Ausschlüsse (notScopes) | 100 |
| Richtlinienregel | Konditionelle Abschnitte | 512 |

## <a name="recommendations-for-managing-policies"></a>Empfehlungen für die Verwaltung von Richtlinien

Im Folgenden sind einige Hinweise zum Erstellen und Verwalten von Richtliniendefinitionen und -zuweisungen, die Sie berücksichtigen sollten, sowie einige nützliche Tipps aufgeführt:

- Bei der Erstellung von Richtliniendefinitionen in Ihrer Umgebung sollten Sie anstatt mit einem Ablehnungseffekt mit einem Überwachungseffekt beginnen, um die Auswirkungen Ihrer Richtliniendefinition auf die Ressourcen in Ihrer Umgebung nachzuverfolgen. Wenn Sie bereits Skripts zur automatischen Skalierung Ihrer Anwendungen eingerichtet haben, kann ein Ablehnungseffekt die von Ihnen bereits eingerichteten Automatisierungsaufgaben verhindern.
- Es ist wichtig, beim Erstellen von Definitionen und Zuweisungen die Organisationshierarchien zu berücksichtigen. Daher wird empfohlen, Definitionen auf einer übergeordneten Ebene wie etwa der Verwaltungsgruppen- oder Abonnementebene zu erstellen und der nächsten untergeordneten Ebene zuzuweisen. Wenn Sie z.B. eine Richtliniendefinition auf Verwaltungsgruppenebene erstellen, kann eine Richtlinienzuweisung dieser Definition innerhalb dieser Verwaltungsgruppe bis auf Abonnementebene heruntergebrochen werden.
- Es wird empfohlen, anstelle von Richtliniendefinitionen immer Initiativdefinitionen zu verwenden, auch wenn Sie nur eine Richtlinie im Auge haben. Wenn Sie z.B. die Richtliniendefinition *policyDefA* unter der Initiativdefinition *initiativeDefC* erstellen und sich entschließen, eine andere Richtliniendefinition für *policyDefB* mit Zielen wie denen von *policyDefA* zu erstellen, können Sie sie unter *initiativeDefC* hinzufügen und auf diese Weise besser nachverfolgen.
- Beachten Sie, dass neue Richtliniendefinitionen, die der Initiativdefinition hinzugefügt werden, automatisch unter die Initiativzuweisung(en) dieser Initiativdefinition fallen, nachdem Sie eine Initiativzuweisung über eine Initiativdefinition erstellt haben.
- Nach der Auslösung einer Initiativzuweisung werden auch alle Richtlinien in der Initiative ausgelöst. Wenn Sie eine Richtlinie einzeln ausführen möchten, empfiehlt es sich, sie nicht in eine Initiative aufzunehmen.

## <a name="video-overview"></a>Videoübersicht

Die folgende Übersicht über Azure Policy stammt von der Build 2018. Folien und den Videodownload finden Sie unter [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Steuern Ihrer Azure-Umgebung per Azure Policy) auf Channel 9.

> [!VIDEO https://channel9.msdn.com/events/Build/2018/THR2030/player]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Überblick über Azure Policy erhalten und einige Schlüsselbegriffe kennengelernt haben, geht es mit den nächsten empfohlenen Schritten weiter:

- [Zuweisen einer Richtliniendefinition](assign-policy-definition.md)
- [Zuweisen einer Richtliniendefinition mit der Azure CLI](assign-policy-definition-cli.md)
- [Zuweisen einer Richtliniendefinition mit PowerShell](assign-policy-definition-ps.md)
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../azure-resource-manager/management-groups-overview.md).
- Sehen Sie sich [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Steuern Ihrer Azure-Umgebung per Azure Policy) auf Channel 9 an.