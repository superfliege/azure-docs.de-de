---
title: Verwenden von Visual Studio auf einem virtuellen Azure-Computer | Microsoft-Dokumentation
description: Verwenden von Visual Studio auf einem virtuellen Azure-Computer
services: virtual-machines-windows
documentationcenter: virtual-machines
author: PhilLee-MSFT
manager: sacalla
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.prod: vs-devops-alm
ms.date: 01/30/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: 813022f1778e2c7f3174e11192b845c2c33ad219
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a id="top"> </a> Visual Studio-Images in Azure
Die Verwendung von Visual Studio auf einem vorkonfigurierten virtuellen Azure-Computer (VM) ist die schnellste und einfachste Methode zum Erzielen einer einsatzfähigen Entwicklungsumgebung ohne Ausgangsvoraussetzungen.  Systemimages mit verschiedenen Visual Studio-Konfigurationen sind im [Azure Marketplace](https://portal.azure.com/) verfügbar. Starten Sie einfach einen virtuellen Computer, und beginnen Sie mit der Arbeit.

Neu bei Azure? [Erstellen Sie ein kostenloses Azure-Konto](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Welche Konfigurationen und Versionen sind verfügbar?
Im Azure Marketplace finden Sie Images für die letzten Hauptversionen: Visual Studio 2017 und Visual Studio 2015.  Für jede Hauptversion werden die ursprünglich veröffentlichte („RTW“) Version und die letzten aktualisierten Versionen aufgeführt.  Zu allen diesen verschiedenen Versionen finden Sie die Editionen Visual Studio Enterprise und Visual Studio Community.

|               Releaseversion              |          Editionen            |    Produktversion    |
|:------------------------------------------:|:----------------------------:|:---------------------:|
| Visual Studio 2017 – letzte (Version 15.5) |    Enterprise, Community     |     Version 15.5.3    |
|         Visual Studio 2017 – RTW           |    Enterprise, Community     |     Version 15.0.7    |
|   Visual Studio 2015 – letzte (Update 3)   |    Enterprise, Community     | Version 14.0.25431.01 |
|         Visual Studio 2015 – RTW           | Keine (für Wartung abgelaufen) |          ---          |

> [!NOTE]
> Im Einklang mit der Microsoft-Wartungsrichtlinie ist die ursprünglich veröffentlichte Version („RTW“) von Visual Studio 2015 für die Wartung abgelaufen.  Visual Studio 2015 Update 3 ist daher die einzige verbleibende Version, die für die Visual Studio 2015-Produktlinie angeboten wird.

Weitere Informationen finden Sie in der [Visual Studio-Wartungsrichtlinie](https://www.visualstudio.com/en-us/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Welche Features werden installiert?
Jedes Image enthält die empfohlenen Features für diese Edition von Visual Studio.  Im Allgemeinen umfasst die Installation:

* Sämtliche verfügbaren Workloads, einschließlich der empfohlenen optionalen Komponenten für diese Workload
* SDKs, unterstützte Pakete und Entwicklertools für .NET 4.6.2 und .NET 4.7
* Visual F#
* GitHub-Erweiterung für Visual Studio
* LINQ to SQL-Tools

Mit dieser Befehlszeile installieren wir Visual Studio zum Erstellen der Images:

   * vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
   * add Microsoft.Net.Component.4.7.SDK ^
   * add Microsoft.Net.Component.4.7.TargetingPack ^ 
   * add Microsoft.Net.Component.4.6.2.SDK ^
   * add Microsoft.Net.Component.4.6.2.TargetingPack ^
   * add Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
   * add Microsoft.VisualStudio.Component.FSharp ^
   * add Component.GitHub.VisualStudio ^
   * add Microsoft.VisualStudio.Component.LinqToSql

Wenn die Images eines der für Sie erforderlichen Visual Studio-Features nicht enthalten, geben Sie dies als Feedback mithilfe des Feedbacktools (rechts oben auf der Seite) an.

## <a name="what-size-vm-should-i-choose"></a>Welche VM-Größe sollte ich auswählen?
Das Bereitstellen eines neuen virtuellen Computers ist einfach, und Azure bietet eine umfassende Palette von VM-Größen.  Wie bei jeder Hardwareanschaffung sollten Sie Leistung und Kosten gegeneinander abwägen.  Da Visual Studio eine leistungsstarke Multithreadinganwendung ist, sollten Sie eine VM-Größe auswählen, die mindestens zwei Prozessoren und 7 GB Arbeitsspeicher einschließt.  Weitere Informationen zu den aktuellen VM-Größen finden Sie unter [Größen für virtuelle Windows-Computer in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Mit Azure ist Ihre erste Auswahl nicht endgültig – Sie können die VM-Größe ändern, um einen neuen Abgleich vorzunehmen.  Sie können einen neuen virtuellen Computer mit einer besser geeigneten Größe bereitstellen oder die Größe des vorhandenen virtuellen Computers an unterschiedliche zugrunde liegende Hardware anpassen.  Weitere Informationen finden Sie unter [Ändern der Größe einer Windows-VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm).

## <a name="after-i-get-the-vm-running-then-what"></a>Wie fahre ich fort, nachdem ich den virtuellen Computer ordnungsgemäß gestartet habe?
Visual Studio folgt in Azure dem Modell „Bring Your Own License“, bei dem Sie die Lizenz selbst beitragen müssen.  Daher besteht ähnlich wie bei einer Installation auf proprietärer Hardware einer der ersten Schritte in der Lizenzierung Ihrer Visual Studio-Installation.  Sie können Visual Studio entsperren, indem Sie sich entweder mit einem Microsoft-Konto, das einem Visual Studio-Abonnement zugeordnet ist, oder mit dem Product Key, den Sie bei Ihren ursprünglichen Kauf erhalten haben, anmelden.  Weitere Informationen finden Sie unter [Anmelden bei Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/signing-in-to-visual-studio) und [Entsperren von Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="after-i-build-out-the-dev-vm-how-do-i-save-capture-it-for-future-or-team-use"></a>Wie speichere (erfasse) ich die Entwicklungs-VM nach dem Ausbau, um sie zukünftig oder im Team verwenden zu können?

Das Spektrum der Entwicklungsumgebungen ist unüberschaubar, und für den Ausbau einer komplexeren Umgebung fallen hohe Kosten an.  Unabhängig von der Konfiguration Ihrer Umgebung kann diese Investition mit Azure jedoch einfach beibehalten werden, da Ihr perfekt konfigurierter virtueller Computer als „Basisimage“ für den zukünftigen Einsatz gespeichert/erfasst wird – sowohl für Sie selbst als auch für andere Mitglieder Ihres Teams.  Sie stellen dann einen neuen virtuellen Computer beim Starten vom Basisimage bereit und nicht vom Marketplace-Image.

Kurze Zusammenfassung: Sie müssen den virtuellen Computer mit Sysprep vorbereiten und dann herunterfahren und ihn anschließend über die Benutzeroberfläche des Azure-Portals *als Image erfassen (Abbildung 1)*.  Azure speichert die Datei `.vhd` mit dem Image im Speicherkonto Ihrer Wahl.  Anschließend wird das neue Image als Imageressource in der Liste der Ressourcen Ihres Abonnements angezeigt.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal’s UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Abbildung 1:) Erfassen eines Images über die Benutzeroberfläche des Azure-Portals*</center>

Weitere Informationen finden Sie unter [Erfassen eines virtuellen Computers in einem Image](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).

  **Zur Erinnerung:** Vergessen Sie nicht, den virtuellen Computer mit Sysprep vorzubereiten.  Wenn Sie diesen Schritt auslassen, kann Azure keinen virtuellen Computer aus dem Image bereitstellen.

> [!NOTE]
> Für das Speichern der Images können weiterhin gewisse Kosten anfassen, diese inkrementellen Kosten fallen jedoch wahrscheinlich im Vergleich zu den Personalkosten für das erneute Ausbauen eines virtuellen Computers von Grund auf – für jede Person in Ihrem Team, die einen virtuellen Computer benötigt – nicht ins Gewicht.  Beispielsweise kostet es wenige Euro, ein Image von 127 GB zu erstellen und einen Monat lang zu speichern, das von allen Mitgliedern des Teams wiederverwendet werden kann.  Diese Kosten sind jedoch im Vergleich zu den Stunden, die jeder Mitarbeiter für das Ausbauen und Überprüfen einer ordnungsgemäß konfigurierten Entwicklungsumgebung für den eigenen Gebrauch investiert, geringfügig.

Darüber hinaus müssen Ihre Entwicklungsaufgaben oder Technologien möglicherweise weiter skaliert werden, etwa in Form von Varianten von Entwicklungskonfigurationen oder der Konfiguration mehrerer Computer.  Mit Azure DevTest Labs können Sie _Rezepte_ erstellen, die die Erstellung eines für Ihre Zwecke optimierten Images automatisieren, und Richtlinien für die ausgeführten virtuellen Computer Ihres Teams verwalten.  [Verwenden von Azure DevTest Labs für Entwickler](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-developer-lab) ist die beste Quelle für weitere Informationen zu DevTest Labs.

## <a name="next-steps"></a>Nächste Schritte
Nun, da Sie sich mit vorkonfigurierten Visual Studio-Images beschäftigt haben, besteht der nächste Schritt im Erstellen eines neuen virtuellen Computers:

* [Erstellen eines virtuellen Computers über das Azure-Portal](quick-create-portal.md)
* [Übersicht über virtuelle Windows-Computer](overview.md)
