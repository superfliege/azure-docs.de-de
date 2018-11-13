---
title: 'Azure Blueprint: Übersicht'
description: Azure Blueprint ist ein Dienst in Azure, den Sie zum Erstellen, Definieren und Bereitstellen von Artefakten in Ihrer Azure-Umgebung verwenden.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/07/2018
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: b2e767bf27962472a19e5d2e704b456cffe18423
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277533"
---
# <a name="what-is-azure-blueprints"></a>Was ist Azure Blueprint?

Genau wie eine Blaupause, die einem Ingenieur oder Architekten die Skizzierung der Entwurfsparameter für ein Projekt ermöglicht, ermöglicht es Azure Blueprints Cloudarchitekten und zentralen IT-Gruppen, eine wiederholbare Gruppe von Azure-Ressourcen zu definieren, mit der die Standards, Muster und Anforderungen einer Organisation implementiert und erzwungen werden. Mit Azure Blueprints können Entwicklungsteams schnell neue Umgebungen bereitstellen und einrichten und dabei darauf vertrauen, dass sie die Konformitätsanforderungen der Organisation erfüllen und über eine Reihe integrierter Komponenten (z.B. Netzwerk) zur Beschleunigung der Entwicklung und Bereitstellung verfügen.

Blaupausen sind eine deklarative Möglichkeit zum Orchestrieren der Bereitstellung mehrerer Ressourcenvorlagen und anderer Artefakte wie etwa:

- Rollenzuweisungen
- Richtlinienzuweisungen
- Azure-Ressourcen-Manager-Vorlagen
- Ressourcengruppen

Der Azure-Dienst für Blaupausen wird vom global verteilten [Azure Cosmos DB](../../cosmos-db/introduction.md)-Dienst unterstützt.
Blaupausenobjekte werden in mehreren Azure-Regionen repliziert. Diese Replikation bietet niedrige Wartezeiten, Hochverfügbarkeit und konsistenten Zugriff auf Ihre Blaupausenobjekte – unabhängig davon, in welcher Region Ihre Ressourcen bereitgestellt werden.

## <a name="how-its-different-from-resource-manager-templates"></a>Unterschied zu Resource Manager-Vorlagen

Der Dienst soll die _Umgebungseinrichtung_ vereinfachen. Diese Einrichtung umfasst häufig eine Reihe von Ressourcengruppen, Richtlinien, Rollenzuweisungen und Resource Manager-Vorlagenbereitstellungen. Eine Blaupause ist ein Paket, in dem die einzelnen _Artefakttypen_ zusammengeführt werden. Mit diesen können Sie das Paket zusammenstellen und versionieren – auch über eine CI/CD-Pipeline. Letztlich wird jede in einem einzelnen Vorgang, der überwacht und nachverfolgt werden kann, einem Abonnement zugewiesen.

Nahezu alle Elemente, die Sie für die Bereitstellung in Blaupausen einfügen möchten, können über eine Resource Manager-Vorlage eingefügt werden. Eine Resource Manager-Vorlage ist jedoch ein Dokument, das in Azure nicht nativ vorhanden ist, sondern entweder lokal oder in der Quellcodeverwaltung gespeichert wird. Die Vorlage wird für die Bereitstellung einer oder mehrerer Azure-Ressourcen verwendet. Nach der Bereitstellung dieser Ressourcen besteht jedoch keine aktive Verbindung oder Beziehung mehr mit der Vorlage.

Mit Blaupausen bleibt die Beziehung zwischen der Blaupausendefinition (was _soll_ bereitgestellt werden) und der Blaupausenzuweisung (was _wurde_  bereitgestellt) erhalten. Diese Verbindung ermöglicht eine erweiterte Nachverfolgung und Überprüfung von Bereitstellungen. Mit Blaupausen lassen sich auch mehrere Abonnements, die der gleichen Blaupause unterliegen, gleichzeitig upgraden.

Es besteht nicht die Notwendigkeit, zwischen einer Resource Manager-Vorlage und einer Blaupause zu wählen. Jede Blaupause kann aus keinem oder mehreren _Artefakten_ für Resource Manager-Vorlagen bestehen. Das bedeutet, dass frühere Bemühungen zur Entwicklung und Verwaltung einer Bibliothek von Resource Manager-Vorlagen in Blaupausen wiederverwendet werden können.

## <a name="how-its-different-from-azure-policy"></a>Unterschied zu Azure Policy

Eine Blaupause ist ein Paket oder Container zum Zusammenstellen von spezifischen Gruppen von Standards, Mustern und Anforderungen in Bezug auf die Implementierung von Azure-Clouddiensten, Sicherheit und Entwurf. Sie kann wiederverwendet werden, um Konsistenz und Konformität zu gewährleisten.

Eine [Richtlinie](../policy/overview.md) ist ein System zur standardmäßigen Zulassung und expliziten Ablehnung, das sich auf Ressourceneigenschaften während der Bereitstellung und für bereits vorhandene Ressourcen konzentriert. Es unterstützt Cloud-Governance, indem sichergestellt wird, dass Ressourcen in einem Abonnement den Anforderungen und Standards entsprechen.

Das Einschließen einer Richtlinie in eine Blaupause ermöglicht die Erstellung des richtigen Musters oder Entwurfs während der Blaupausenzuweisung. Die eingeschlossene Richtlinie sorgt dafür, dass an der Umgebung nur genehmigte oder erwartete Änderungen vorgenommen werden können und die Konformität mit der Absicht der Blaupause kontinuierlich gewahrt bleibt.

Eine Richtlinie kann als eines von vielen _Artefakten_ in eine Blaupausendefinition eingefügt werden. Blaupausen unterstützen zudem die Verwendung von Parametern mit Richtlinien und Initiativen.

## <a name="blueprint-definition"></a>Blaupausendefinition

Eine Blaupause besteht aus _Artefakten_. Azure Blueprint unterstützt derzeit die folgenden Ressourcen als Artefakte:

|Ressource  | Hierarchieoptionen| BESCHREIBUNG  |
|---------|---------|---------|
|Ressourcengruppen     | Abonnement | Erstellen einer neuen Ressourcengruppe zur Verwendung durch andere Artefakte innerhalb der Blaupause.  Diese Platzhalter-Ressourcengruppen ermöglichen es, Ressourcen genau auf die gewünschte Weise zu strukturieren. Sie umfassen eine Bereichsbeschränkung für enthaltene Richtlinien- und Rollenzuweisungsartefakte und Azure Resource Manager-Vorlagen.         |
|Azure Resource Manager-Vorlage      | Ressourcengruppe | Vorlagen werden verwendet, um komplexe Umgebungen zusammenzustellen. Beispielumgebungen: SharePoint-Farm, Azure Automation State Configuration oder Log Analytics-Arbeitsbereich. |
|Richtlinienzuweisung     | Abonnement, Ressourcengruppe | Ermöglicht die Zuweisung einer Richtlinie oder Initiative zum Abonnement, dem die Blaupause zugewiesen ist. Die Richtlinie oder Initiative muss innerhalb des Bereichs der Blaupause liegen (in der Verwaltungsgruppe der Blaupause oder darunter). Wenn die Richtlinie oder Initiative über Parameter verfügt, werden diese bei der Erstellung der Blaupause oder bei der Blaupausenzuweisung zugewiesen.       |
|Rollenzuweisung   | Abonnement, Ressourcengruppe | Fügt einer integrierten Rolle einen vorhandenen Benutzer oder eine vorhandene Gruppe zu, um sicherzustellen, dass die richtigen Personen geeigneten Zugriff auf Ihre Ressourcen haben. Rollenzuweisungen können für das gesamte Abonnement definiert oder in einer bestimmten in der Blaupause enthaltenen Ressourcengruppe geschachtelt werden. |

### <a name="blueprints-and-management-groups"></a>Blaupausen und Verwaltungsgruppen

Bei der Erstellung einer Blaupausendefinition legen Sie fest, wo die Blaupause gespeichert wird. Derzeit können Blaupausen nur in einer [Verwaltungsgruppe](../management-groups/overview.md) gespeichert werden, für die Sie über den Zugriff **Mitwirkender** verfügen. Die Blaupause kann dann jedem untergeordneten Element dieser Verwaltungsgruppe zugewiesen werden.

> [!IMPORTANT]
> Wenn Sie keinen Zugriff auf Verwaltungsgruppen haben oder keine Verwaltungsgruppen konfiguriert sind, wird dies beim Laden der Liste der Blaupausendefinitionen angezeigt. Beim Klicken auf **Bereich** wird ein Fenster mit einer Abrufwarnung für Verwaltungsgruppen angezeigt. Um dies zu beheben, stellen Sie sicher, dass das Abonnement, auf das Sie entsprechenden Zugriff haben, Teil einer [Verwaltungsgruppe](../management-groups/overview.md) ist.

### <a name="blueprint-parameters"></a>Blaupausenparameter

Azure Blueprint kann Parameter entweder an eine Richtlinie oder Initiative oder an eine Azure Resource Manager-Vorlage übergeben.
Beim Hinzufügen eines _Artefakts_ zu einer Blaupause entscheidet der Ersteller, ob er einen definierten Wert für jede Blaupausenzuweisung angeben möchte oder ob bei jeder Blaupausenzuweisung ein Wert angegeben werden kann. Diese Flexibilität bietet die Möglichkeit, einen vorab festgelegten Wert für alle Verwendungen der Blaupause zu definieren. Es ist auch möglich, diese Festlegung zum Zeitpunkt der Zuweisung vorzunehmen.

> [!NOTE]
> Eine Blaupause kann über eigene Parameter verfügen, diese können derzeit jedoch nur erstellt werden, wenn die Blaupause mit der REST-API und nicht im Portal generiert wird.

Weitere Informationen finden Sie unter [Blaupausenparameter](./concepts/parameters.md).

### <a name="blueprint-publishing"></a>Blaupausenveröffentlichung

Wenn eine Blaupause erstellt wird, befindet sie sich im **Entwurfsmodus**. Wenn sie bereit ist für die Zuweisung, muss sie **veröffentlicht** werden. Für die Veröffentlichung muss eine **Versionszeichenfolge** (Buchstaben, Zahlen und Bindestriche mit einer maximalen Länge von 20 Zeichen) zusammen mit optionalen **Änderungshinweisen** definiert werden. Durch die **Version** unterscheidet sich eine Blaupause von zukünftigen Änderungen an der Blaupause, sodass jede Version zugewiesen werden kann. Diese Versionierung bedeutet auch, dass einem Abonnement verschiedene **Versionen** der gleichen Blaupause zugewiesen werden können. Wenn zusätzliche Änderungen an der Blaupause vorgenommen werden, existiert die **veröffentlichte** **Version** parallel zu den **unveröffentlichten Änderungen**. Nachdem die Änderungen abgeschlossen wurden, wird die aktualisierte Blaupause mit einer neuen und eindeutigen **Version** **veröffentlicht** und kann dann auch zugewiesen werden.

## <a name="blueprint-assignment"></a>Blaupausenzuweisung

Jede **veröffentlichte** **Version** einer Blaupause kann einem vorhandenen Abonnement zugewiesen werden. Im Portal wird standardmäßig die **Version** der Blaupause verwendet, die zuletzt **veröffentlicht** wurde. Wenn Artefaktparameter (oder Blaupausenparameter) vorhanden sind, werden die Parameter während des Zuweisungsvorgangs definiert.

## <a name="permissions-in-azure-blueprints"></a>Berechtigungen in Azure Blueprint

Zur Verwendung von Blaupausen müssen Ihnen über die [rollenbasierte Zugriffssteuerung](../../role-based-access-control/overview.md) (Role-Based Access Control, RBAC) Berechtigungen erteilt werden. Zur Erstellung von Blaupausen sind für Ihr Konto die folgenden Berechtigungen erforderlich:

- `Microsoft.Blueprint/blueprints/write`: Erstellen einer Blaupausendefinition
- `Microsoft.Blueprint/blueprints/artifacts/write`: Erstellen von Artefakten in einer Blaupausendefinition
- `Microsoft.Blueprint/blueprints/versions/write`: Veröffentlichen einer Blaupause

Zum Löschen von Blaupausen sind für Ihr Konto die folgenden Berechtigungen erforderlich:

- `Microsoft.Blueprint/blueprints/delete`
- `Microsoft.Blueprint/blueprints/artifacts/delete`
- `Microsoft.Blueprint/blueprints/versions/delete`

> [!NOTE]
> Da Blaupausendefinitionen in einer Verwaltungsgruppe erstellt werden, müssen die Berechtigungen für Blaupausendefinitionen in einem Verwaltungsgruppenbereich erteilt oder vererbt werden.

Zum Zuweisen oder zum Aufheben der Zuweisung einer Blaupause sind für Ihr Konto die folgenden Berechtigungen erforderlich:

- `Microsoft.Blueprint/blueprintAssignments/write`: Zuweisen einer Blaupause
- `Microsoft.Blueprint/blueprintAssignments/delete`: Aufheben der Zuweisung einer Blaupause

> [!NOTE]
> Da Blaupausenzuweisungen in einem Abonnement erstellt werden, müssen die Berechtigungen zum Zuweisen und Aufheben der Zuweisung von Blaupausen in einem Abonnementbereich erteilt oder vererbt werden.

Mit Ausnahme der Berechtigungen zum Zuweisen von Blaupausen sind diese Berechtigungen in der Rolle **Besitzer** sowie in den Rollen vom Typ **Mitwirkender** enthalten. Sollten diese integrierten Rollen nicht Ihren Sicherheitsanforderungen entsprechen, können Sie eine [benutzerdefinierte Rolle](../../role-based-access-control/custom-roles.md) erstellen.

> [!NOTE]
> Für den Dienstprinzipal für Azure Blueprint ist die Rolle **Besitzer** für das zugewiesene Abonnement erforderlich, um die Bereitstellung zu ermöglichen. Bei Verwendung des Portals wird diese Rolle für die Bereitstellung automatisch erteilt und widerrufen. Bei Verwendung der REST-API muss diese Rolle manuell erteilt werden, sie wird jedoch nach Abschluss der Bereitstellung auch automatisch widerrufen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Blaupause – Portal](create-blueprint-portal.md)
- [Erstellen einer Blaupause – REST-API](create-blueprint-rest-api.md)