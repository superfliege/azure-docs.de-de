---
title: 'Azure Blueprint: Übersicht'
description: Azure Blueprint ist ein Dienst in Azure, den Sie zum Erstellen, Definieren und Bereitstellen von Artefakten in Ihrer Azure-Umgebung verwenden.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: c7ffbe86407bc776870890e5b4151556f572832e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957510"
---
# <a name="what-is-azure-blueprints"></a>Was ist Azure Blueprint?

So wie eine Blaupause einem Ingenieur oder einem Architekten ermöglicht, die Entwurfsparameter für ein Projekt zu skizzieren, ermöglicht Azure Blueprint Cloudarchitekten und der zentralen IT-Abteilung, eine wiederholbare Gruppe von Azure-Ressourcen zu definieren, mit denen die Standards, Muster und Anforderungen einer Organisation implementiert und erzwungen werden. Mit Azure Blueprint können Entwicklungsteams schnell neue Umgebungen bereitstellen und einrichten, mit dem Wissen, dass sie entsprechend den Konformitätsanforderungen der Organisation erstellt werden und eine Reihe integrierter Komponenten (z.B. Netzwerkfunktionen) zur Beschleunigung der Entwicklung und Bereitstellung enthalten.

Blaupausen sind eine deklarative Möglichkeit zum Orchestrieren der Bereitstellung mehrerer Ressourcenvorlagen und anderer Artefakte wie beispielsweise:

- Rollenzuweisungen
- Richtlinienzuweisungen
- Azure-Ressourcen-Manager-Vorlagen
- Ressourcengruppen

## <a name="how-it-is-different-from-resource-manager-templates"></a>Unterschied zu Resource Manager-Vorlagen

Blaupausen sollen beim _Einrichten der Umgebung_ helfen, bei der neben der Bereitstellung von Resource Manager-Vorlagen häufig eine Reihe von Ressourcengruppen, Richtlinien und Rollenzuweisungen eingerichtet wird. Eine Blaupause ist ein Paket, in dem die einzelnen _Artefakttypen_ zusammengeführt werden. Mit diesen können Sie das Paket zusammenstellen und versionieren – auch über eine CI/CD-Pipeline. Letztlich wird jede in einem einzelnen Vorgang, der überwacht und nachverfolgt werden kann, einem Abonnement zugewiesen.

Nahezu alle Elemente, die Sie für die Bereitstellung in Blaupausen einfügen möchten, können über eine Resource Manager-Vorlage eingefügt werden. Allerdings ist eine Resource Manager-Vorlage ein Dokument, das in Azure nicht nativ vorhanden ist, sondern entweder lokal oder in der Quellcodeverwaltung gespeichert wird. Die Vorlage wird für die Bereitstellung einer oder mehrerer Azure-Ressourcen verwendet. Nachdem diese Ressourcen bereitgestellt wurden, geht die Verbindung und Beziehung zu der Vorlage jedoch verloren.

Mit Blaupausen bleibt die Beziehung zwischen der Blaupausendefinition (_was soll_ bereitgestellt werden) und der Blaupausenzuweisung (_was wurde_  bereitgestellt) erhalten. Diese Verbindung ermöglicht eine bessere Nachverfolgung und Überwachung von Bereitstellungen und bietet unter anderem die Möglichkeit, mehrere Abonnements, die über die gleiche Blaupause gesteuert werden, gleichzeitig zu aktualisieren.

Es besteht nicht die Notwendigkeit, zwischen einer Resource Manager-Vorlage und einer Blaupause zu wählen. Jede Blaupause kann aus keinem oder mehreren _Artefakten_ für Resource Manager-Vorlagen bestehen. Das bedeutet, dass frühere Bemühungen zur Entwicklung und Verwaltung einer Bibliothek von Resource Manager-Vorlagen in Blaupausen genutzt werden können.

## <a name="how-it-is-different-from-azure-policy"></a>Unterschied zu Azure Policy

Eine Blaupause ist ein Paket oder ein Container zum Zusammenstellen von spezifischen Gruppen von Standards, Mustern und Anforderungen in Bezug auf die Implementierung von Azure-Clouddiensten, Sicherheit und Entwurf. Sie kann wiederverwendet werden, um Konsistenz und Konformität sicherzustellen.

Eine [Richtlinie](../policy/overview.md) ist ein System zur standardmäßigen Zulassung und expliziten Ablehnung, das sich auf Ressourceneigenschaften während der Bereitstellung und für bereits vorhandene Ressourcen konzentriert. Es unterstützt IT-Governance, indem sichergestellt wird, dass Ressourcen in einem Abonnement den Anforderungen und Standards entsprechen.

Das Einfügen einer Richtlinie in einer Blaupause ermöglicht nicht nur die Erstellung des richtigen Musters oder Entwurfs bei der Zuweisung der Blaupause, sondern stellt auch sicher, dass nur genehmigte oder erwartete Änderungen an der Umgebung vorgenommen werden können, um die kontinuierliche Konformität mit der Zielsetzung der Blaupause zu gewährleisten.

Eine Richtlinie kann als eines von vielen _Artefakten_ in eine Blaupausendefinition eingefügt werden. Blaupausen unterstützen zudem die Verwendung von Parametern mit Richtlinien und Initiativen.

## <a name="blueprint-definition"></a>Blaupausendefinition

Eine Blaupause besteht aus _Artefakten_. Azure Blueprint unterstützt derzeit die folgenden Ressourcen als Artefakte:

|Ressource  | Hierarchieoptionen| BESCHREIBUNG  |
|---------|---------|---------|
|Ressourcengruppen     | Abonnement | Erstellen einer neuen Ressourcengruppe zur Verwendung durch andere Artefakte innerhalb der Blaupause.  Diese Platzhalter-Ressourcengruppen ermöglichen Ihnen, Ressourcen genau auf die gewünschte Weise zu strukturieren. Sie umfassen eine Bereichsbeschränkung für enthaltene Richtlinien- und Rollenzuweisungsartefakte sowie Azure Resource Manager-Vorlagen.         |
|Azure Resource Manager-Vorlage      | Ressourcengruppe | Diese Vorlagen können zum Erstellen komplexer Umgebungen verwendet werden, z.B. SharePoint-Farm, Azure Automation State Configuration oder Log Analytics-Arbeitsbereich. |
|Richtlinienzuweisung     | Abonnement, Ressourcengruppe | Ermöglicht die Zuweisung einer Richtlinie oder Initiative zur Verwaltungsgruppe oder zum Abonnement, der bzw. dem die Blaupause zugewiesen ist. Die Richtlinie oder Initiative muss innerhalb des Bereichs der Blaupause liegen (in der Verwaltungsgruppe der Blaupause oder darunter). Wenn die Richtlinie oder Initiative über Parameter verfügt, können diese Parameter bei der Erstellung der Blaupause oder bei der Blaupausenzuweisung zugewiesen werden.       |
|Rollenzuweisung   | Abonnement, Ressourcengruppe | Fügt einen vorhandenen Benutzer oder eine vorhandene Gruppe einer integrierten Rolle zu, um sicherzustellen, dass die richtigen Personen geeigneten Zugriff auf Ihre Ressourcen haben. Rollenzuweisungen können für das gesamte Abonnement definiert oder in einer bestimmten in der Blaupause enthaltenen Ressourcengruppe geschachtelt werden. |

### <a name="blueprints-and-management-groups"></a>Blaupausen und Verwaltungsgruppen

Bei der Erstellung einer Blaupausendefinition legen Sie fest, wo die Blaupause gespeichert wird. Derzeit können Blaupausen nur in einer [Verwaltungsgruppe](../management-groups/overview.md) gespeichert werden, für die Sie über den Zugriff **Mitwirkender** verfügen. Die Blaupause kann dann jedem untergeordneten Element (Verwaltungsgruppe oder Abonnement) dieser Verwaltungsgruppe zugewiesen werden.

> [!IMPORTANT]
> Wenn Sie keinen Zugriff auf Verwaltungsgruppen haben oder keine Verwaltungsgruppen konfiguriert sind, wird dies beim Laden der Liste der Blaupausendefinitionen angezeigt. Beim Klicken auf **Bereich** wird ein Fenster mit einer Warnung zum Abrufen von Verwaltungsgruppen angezeigt. Um dies zu beheben, stellen Sie sicher, dass das Abonnement, auf das Sie entsprechenden Zugriff haben, Teil einer [Verwaltungsgruppe](../management-groups/overview.md) ist.

### <a name="blueprint-parameters"></a>Blaupausenparameter

Azure Blueprint kann Parameter entweder an eine Richtlinie oder Initiative oder an eine Azure Resource Manager-Vorlage übergeben.
Wenn einer Blaupause ein _Artefakt_ hinzugefügt wird, kann der Autor einen definierten Wert für jede Blaupausenzuweisung angeben oder festlegen, dass für jede Blaupausenzuweisung zum Zeitpunkt der Zuweisung ein Wert angegeben werden kann. Diese Flexibilität bietet die Möglichkeit, einen vorab festgelegten Wert für alle Verwendungen der Blaupause zu definieren. Es ist auch möglich, diese Festlegung zum Zeitpunkt der Zuweisung vorzunehmen.

> [!NOTE]
> Eine Blaupause kann über eigene Parameter verfügen, diese können derzeit jedoch nur erstellt werden, wenn die Blaupause mit der REST-API und nicht im Portal generiert wird.

Weitere Informationen finden Sie unter [Blaupausenparameter](./concepts/parameters.md).

### <a name="blueprint-publishing"></a>Blaupausenveröffentlichung

Wenn eine Blaupause erstellt wird, befindet sie sich im **Entwurfsmodus**. Wenn sie bereit ist für die Zuweisung, muss sie **veröffentlicht** werden. Für die Veröffentlichung muss eine **Versionszeichenfolge** (Buchstaben, Zahlen und Bindestriche mit einer maximalen Länge von 20 Zeichen) zusammen mit optionalen **Änderungshinweisen** definiert werden.
Durch die **Version** unterscheidet sich eine Blaupause von zukünftigen Änderungen an der Blaupause, sodass jede Version zugewiesen werden kann. Dies bedeutet auch, dass einem Abonnement verschiedene **Versionen** derselben Blaupause zugewiesen werden können. Wenn zusätzliche Änderungen an der Blaupause vorgenommen werden, ist die **veröffentlichte** **Version** zusammen mit den **unveröffentlichten Änderungen** weiterhin vorhanden. Nachdem die Änderungen abgeschlossen wurden, wird die aktualisierte Blaupause mit einer neuen und eindeutigen **Version** **veröffentlicht** und kann dann auch zugewiesen werden.

## <a name="blueprint-assignment"></a>Blaupausenzuweisung

Jede **veröffentlichte** **Version** einer Blaupause kann einem vorhandenen Abonnement zugewiesen werden. Im Portal wird standardmäßig die **Version** der Blaupause angezeigt, die zuletzt **veröffentlicht** wurde. Wenn Artefaktparameter (oder Blaupausenparameter) vorhanden sind, werden die Parameter während des Zuweisungsvorgangs definiert.

## <a name="permissions-in-azure-blueprints"></a>Berechtigungen in Azure Blueprint

Zur Verwendung von Blaupausen müssen Sie über die [rollenbasierte Zugriffssteuerung](../../role-based-access-control/overview.md) (RBAC) autorisiert werden. Zur Erstellung von Blaupausen sind für Ihr Konto die folgenden Berechtigungen erforderlich:

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

Diese Berechtigungen sind in der Rolle **Besitzer** und mit Ausnahme der Berechtigungen für Blaupausenzuweisungen auch in der Rolle **Mitwirkender** enthalten. Wenn diese integrierten Rollen nicht Ihren Sicherheitsanforderungen entsprechen, können Sie eine [benutzerdefinierte Rolle](../../role-based-access-control/custom-roles.md) erstellen.

> [!NOTE]
> Für den Dienstprinzipal für Azure Blueprint ist die Rolle **Besitzer** für das zugewiesene Abonnement erforderlich, um die Bereitstellung zu ermöglichen. Bei Verwendung des Portals wird diese Rolle für die Bereitstellung automatisch erteilt und widerrufen. Bei Verwendung der REST-API muss diese Rolle manuell erteilt werden, sie wird jedoch nach Abschluss der Bereitstellung auch automatisch widerrufen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Blaupause – Portal](create-blueprint-portal.md)
- [Erstellen einer Blaupause – REST-API](create-blueprint-rest-api.md)