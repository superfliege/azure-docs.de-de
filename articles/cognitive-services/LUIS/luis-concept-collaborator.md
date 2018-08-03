---
title: Grundlegendes zur Zusammenarbeit bei LUIS-Apps – Azure | Microsoft-Dokumentation
description: LUIS-Apps erfordern einen einzelnen Besitzer und optionale Projektmitarbeiter.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: 684507b58e5ac766e9b42d3aa53cb0867a131a93
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222671"
---
# <a name="collaborating"></a>Zusammenarbeiten

LUIS ermöglicht die Zusammenarbeit einer Gruppe von Personen beim Erstellen einer App.

## <a name="luis-account"></a>LUIS-Konto
Ein LUIS-Konto ist einem einzelnen [Microsoft Live](https://login.live.com/)-Konto zugeordnet. Jedes LUIS-Konto erhält einen kostenlosen [Erstellungsschlüssel](luis-concept-keys.md#authoring-key) zum Erstellen sämtlicher LUIS-Apps, auf die das Konto Zugriff hat. 

Ein LUIS-Konto kann über viele LUIS-Apps verfügen.

Unter [Azure Active Directory-Mandantenbenutzer](luis-how-to-account-settings.md#azure-active-directory-tenant-user) finden Sie weitere Informationen zu Active Directory-Benutzerkonten. 

## <a name="luis-app-owner"></a>LUIS-App-Besitzer
Das Konto, das eine App erstellt, ist der Besitzer. Jede App weist genau einen Besitzer auf. Der Besitzer wird in den **[Einstellungen](luis-how-to-collaborate.md)** der App aufgeführt. Dies ist das Konto, das die App löschen kann. Dies ist auch das Konto, das eine E-Mail empfängt, wenn das Endpunktkontingent 75 % des monatliches Limits erreicht. 

## <a name="transfer-ownership"></a>Übertragen des Besitzes
LUIS erlaubt nicht das Übertragen des Besitzers, Projektmitarbeiter können jedoch die App exportieren und dann eine App durch Importieren erstellen. Beachten Sie, dass die neue App eine andere App-ID aufweist. Die neue App muss trainiert und veröffentlicht werden, und es muss ein neuer Endpunkt verwendet werden.

## <a name="luis-app-collaborators"></a>LUIS-App-Projektmitarbeiter
Ein App-Besitzer kann einer App Projektmitarbeiter hinzufügen. Der Besitzer muss die E-Mail-Adressen der Projektmitarbeiter in den **[Einstellungen](luis-how-to-collaborate.md)** der App hinzufügen. Projektmitarbeiter haben Vollzugriff auf die App. Wenn ein Projektmitarbeiter die App löscht, wird diese aus dem Konto des Projektmitarbeiters entfernt, sie verbleibt aber im Konto des Besitzers. 

Wenn Sie mehrere Apps mit Projektmitarbeitern gemeinsam bearbeiten möchten, müssen Sie jeder App die E-Mail-Adressen der Projektmitarbeiter hinzufügen. 

## <a name="managing-multiple-authors"></a>Verwalten mehrerer Ersteller
Die [LUIS](luis-reference-regions.md#luis-website)-Website erlaubt derzeit keine Erstellung auf Transaktionsebene. Sie können Erstellern die Arbeit an unabhängigen Versionen einer Basisversion ermöglichen. In den folgenden Abschnitten werden zwei unterschiedliche Methoden beschrieben.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Verwalten mehrerer Versionen in derselben App
Zunächst [klonen](luis-how-to-manage-versions.md#clone-a-version) Sie für jeden Ersteller eine Version aus einer Basisversion. 

Jeder Ersteller ändert seine eigene Version der App. Sobald jeder Ersteller mit seinem Modell zufrieden ist, werden die neuen Versionen in JSON-Dateien exportiert.  

Die exportierten Apps sind JSON-formatierte Dateien, die auf Änderungen verglichen werden können. Kombinieren Sie die Dateien, um eine einzelne JSON-Datei der neuen Version zu erstellen. Ändern Sie die **versionId**-Eigenschaft in der JSON-Datei, um die neue zusammengeführte Version zu kennzeichnen. Importieren Sie diese Version in die ursprüngliche App. 

Diese Methode erlaubt eine aktive Version, eine Stagingversion und eine veröffentlichte Version. Im interaktiven Testbereich können Sie die Ergebnisse der drei Versionen vergleichen.

### <a name="manage-multiple-versions-as-apps"></a>Verwalten mehrerer Versionen als Apps
[Exportieren](luis-how-to-manage-versions.md#export-version) Sie die Basisversion. Jeder Ersteller importiert die Version. Die Person, die die App importiert, ist auch der Besitzer dieser Version. Wenn sie das Bearbeiten der App abgeschlossen hat, exportiert sie ihre Version. 

Die exportierten Apps sind JSON-formatierte Dateien, die mit dem Basisexport auf Änderungen verglichen werden können. Kombinieren Sie die Dateien, um eine einzelne JSON-Datei der neuen Version zu erstellen. Ändern Sie die **versionId**-Eigenschaft in der JSON-Datei, um die neue zusammengeführte Version zu kennzeichnen. Importieren Sie diese Version in die ursprüngliche App.

## <a name="next-steps"></a>Nächste Schritte

Grundlegendes zu den Konzepten der [Versionsverwaltung](luis-concept-version.md). 

Weitere Informationen zum Verwalten von Projektmitarbeitern in Ihrer LUIS-App finden Sie unter [App-Einstellungen](luis-how-to-collaborate.md).

Weitere Informationen zu den APIs für das Erstellen finden Sie unter [Add email to access list](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) (Hinzufügen von E-Mail-Adressen zu Zugriffslisten).