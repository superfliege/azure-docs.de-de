---
title: IoT Edge-Modulzertifizierung | Microsoft-Dokumentation
description: Zertifizieren Sie ein neues IoT Edge-Modul für den Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: c8056bd4912605a4cd3ee333b1be87d4f3a6d5ba
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198047"
---
# <a name="the-iot-edge-module-certification-process"></a>Prozess für die IoT Edge-Modulzertifizierung

In diesem Artikel werden die Schritte und Anforderungen zur Zertifizierung eines IoT Edge-Moduls für die Veröffentlichung im Azure Marketplace beschrieben. 

>[!Note]
>Dies ist ein temporäres Dokument. Der Marketplace für IoT Edge-Module wird parallel aufgebaut und dieser Artikel wird durch eine öffentliche Dokumentation ersetzt.

## <a name="understanding-an-iot-edge-module"></a>Grundlegendes zum IoT Edge-Modul

Modulterminologie:

-   Ein **Modulimage** ist ein Paket mit der Software, die ein Modul definiert.

-   Eine **Modulinstanz** ist die spezifische Recheneinheit, auf der das Modulimage auf einem IoT Edge-Gerät ausgeführt wird. Die Modulinstanz wird von der IoT Edge-Runtime gestartet.

Die Module können auch das IoT-Modul-SDK umfassen, das die folgende Terminologie verwendet:

-   Eine **Modulidentität** ist eine Information (einschließlich Sicherheitsanmeldeinformationen), die in IoT Hub gespeichert und den einzelnen Modulinstanzen zugeordnet wird.

-   Ein **Modulzwilling** ist ein in IoT Hub gespeichertes JSON-Dokument, das Statusinformationen für eine Modulinstanz enthält, einschließlich Metadaten, Konfigurationen und Bedingungen.

-   **SDKs** werden verwendet, um benutzerdefinierte Module in mehreren Sprachen zu entwickeln, z. B.: C\#, C, Python, Java und Node.JS.

## <a name="the-onboarding-process-for-an-iot-edge-module"></a>Onboardingprozess für ein IoT Edge-Modul

Der folgende Screenshot zeigt den Prozess, bei dem ein IoT Edge-Modul im Azure Marketplace öffentlich wird.

![Zertifizierungsprozess](./media/cloud-partner-portal-iot-edge-module-certification-process/onboarding-process.png)

### <a name="onboarding-step-details"></a>Details zum Onboardingschritt

-   **Schritt 1**: Partner reichen ihre Angebote mit dem Angebotstyp **IoT Edge-Modul** im Cloud-Partnerportaltool über das Azure Marketplace-Team ein. Sie müssen ein offizieller MS-Partner sein, um auf das Cloud-Partnerportaltool zugreifen zu können. Weitere Informationen finden Sie im [Leitfaden des Herausgebers](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   **Schritt 2**: Der Marketplace führt automatisch Virenschutz- und Antischadsoftwareprüfungen durch. Das IoT-Team kann in dieser Phase seine benutzerdefinierten automatisierten Erfassungstests durchführen.

-   **Schritt 3**: Das Modul ist öffentlich verfügbar. Es ist im Marketplace gelistet und der Container kann anonym über eine URL abgerufen werden. Beispiel: *marketplace.azurecr.io/module-identifier*.

## <a name="iot-edge-module-certification-criteria"></a>Kriterien für die IoT Edge-Modulzertifizierung

Nachfolgend sind die wichtigsten Anforderungen aufgeführt, die erfüllt werden müssen, damit ein IoT Edge-Modul zertifiziert und im Azure Marketplace veröffentlicht werden kann.

>[!Note]
>Diese Anforderungen können sich ändern. Sie werden im Voraus benachrichtigt und erhalten Zeit, Ihre Container zu aktualisieren, damit sie den aktuellen Anforderungen entsprechen können.

### <a name="technical-requirements"></a>Technische Anforderungen

**Es muss sich um ein IoT Edge-Modul handeln**

-   Als IoT Edge-Module werden derzeit nur Docker-kompatible Container unterstützt. Das Modul muss unter [Moby](https://mobyproject.org/) ausgeführt werden. 

    >[!Note]
    >Docker-Container werden voraussichtlich mit Moby funktionieren, da Moby das zugrunde liegende Open Source-Projekt für Docker ist.

-   Der Partner muss die folgenden Standardeinstellungen bereitstellen: 

    -   Ein Standard-**Tag** (das nicht leer sein darf).

    -   Ein standardmäßiges **createOptions** (das leer sein darf).

    -   Wenn das IoT-Modul-SDK verwendet wird, ist ein standardmäßiges **twin** erforderlich (das leer sein darf).

    -   Wenn das IoT-Modul-SDK verwendet wird, ist ein standardmäßiges **routes** erforderlich (das leer sein darf).

**Plattformunterstützung**

-   Es müssen nur Plattformen unterstützt werden, die auf IoT Edge-Ebene 1 **allgemein verfügbar** sind (wie in [Azure IoT Edge-Support](https://docs.microsoft.com/azure/iot-edge/support) beschrieben). Dies bedeutet derzeit z. B. die Unterstützung der folgenden Kombinationen von Betriebssystem und Architektur:

    -   Ubuntu Server 18.04 für x64

    -   Ubuntu Server 16.04 für x64

    -   Raspbian-stretch für arm32 (armhf)

**Gerätedimensionierung**

-   Jedes Gerät mit den entsprechenden Größen (CPU/RAM/Speicher/GPU usw.) eines Raspberry Pi oder höher kann ein IoT Edge-Gerät sein. Wenn ein Modul nur innerhalb bestimmter Größenbeschränkungen arbeitet, müssen diese Beschränkungen in der Modulbeschreibung angegeben werden.

**Standardeinstellungen**

-   Ein Modul sollte sofort nach dem Auspacken für jede unterstützte Plattform (Betriebssystem und Architektur) mit den Standardparametern beginnen.

-   Konfigurationseinstellungen sollten eindeutig dokumentiert werden (Tags, Umgebungsvariablen, „twin“, „routes“). Im Rahmen der Auflistung können Partner eine Beschreibung für ihr Modul definieren, die einfaches HTML-Markup unterstützt oder auf eine externe Webseite verweist.

**Versionsverwaltung**

-   Kunden müssen die Wahl haben, ob sie ein Modul aus dem Marketplace automatisch aktualisieren oder eine bestimmte Version verwenden möchten, die sie getestet haben. Marketplace-Module müssen demselben Muster bei der Versionsverwaltung folgen wie die IoT Edge-Runtime. Beispiel: 

    -   Fortlaufende Tags wie „1.0“ können aktualisiert werden.

    -   Tags für Nebenversionen wie „1.3.24“ können nicht aktualisiert werden.

**Telemetrie**

-   Module, die das SDK für IoT-Module verwenden, müssen den vom Marketplace vergebenen eindeutige Modulbezeichner für Telemetriedatenzwecke festlegen. Auf diese Weise kann der Azure Marketplace die Anzahl der aktiven Modulinstanzen ermitteln. Dieser eindeutige Bezeichner ist der Containername, der vom Marketplace bei der Erfassung vergeben wird. Verwenden Sie die Methoden der folgenden SDKs, um diesen Bezeichner festzulegen:
    - [C\#](https://hub.docker.com/_/mysql/)
    - [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

-   Bei Modulen, die das SDK für IoT-Module nicht verwenden, sind weniger präzise Erkenntnisse im Cloud-Partnerportal verfügbar. Beispiel: die Anzahl von Downloads.

**Sicherheit**

-   Container sollten Zugriff mit möglichst geringen Berechtigungen auf den Host erhalten. Container mit „Berechtigungen“ sollten extrem selten sein.

-   Partner müssen ihr Modul im Rahmen des von ihnen geleisteten Supports schützen.

**Updates**

-   Die Partner müssen bei ihrem Modul dafür sorgen, dass es auf dem neuesten Stand und funktionsfähig ist. Sie werden im Voraus benachrichtigt, wenn es für die IoT Edge-Runtime geplante Änderungen gibt.

**SDK für IoT-Module**

-   Die Aufnahme des SDK für IoT-Module ist keine Voraussetzung für die Zertifizierung.
    Durch die Aufnahme des SDK für IoT-Module kann jedoch die Benutzerfreundlichkeit verbessert werden. Beispielsweise kann das Weiterleiten, Senden von Nachrichten an die Cloud usw. unterstützt werden. Das SDK für IoT-Module ist erforderlich, um Telemetriedaten über die Anzahl der ausgeführten Modulinstanzen zu erhalten.

**Subjektive Anforderung**

-   Muss mindestens einem realen IoT Edge-Anwendungsfall entsprechen. So sollte z. B. ein WordPress-Container nicht integriert werden, es sei denn, ein Kunde ist bereit, ihn von IoT Edge aus zu nutzen.

**Rechtliche Anforderungen (aus AMP-Richtlinie)**

-   Das Modul muss den Vereinbarungen und Richtlinien von Microsoft Azure Marketplace entsprechen. Weitere Informationen finden Sie in der beigefügten Herausgebervereinbarung und der [Teilnahmerichtlinie](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

-   Zusätzlich zu den Vereinbarungen und Richtlinien des Azure Marketplace gibt es möglicherweise zusätzliche gesetzliche Anforderungen, die speziell für einen Angebotstyp des IoT Edge-Moduls gelten. Dies wird derzeit geprüft.

-   Das Modul muss über *Nutzungsbedingungen* und eine *Datenschutzrichtlinie* verfügen.

-   Das Modul sollte auch einen Drittanbieterhinweis erhalten, wenn das Modul auf Dritte zurückgreift.

**Supportanforderungen (aus AMP-Richtlinie)**

-   Die Partner sind für die Unterstützung ihrer IoT Edge-Module verantwortlich. Sie stellen sicher, dass die in der Beschreibung des IoT Edge-Moduls angegebenen Supportoptionen verfügbar bleiben und dass mindestens eine Supportoption immer verfügbar ist. (Weitere Informationen finden Sie im Abschnitt 4 der AMP-Herausgebervereinbarung.)

**Kategorisierung**

-   Alle IoT Edge-Module werden unter der Kategorie **Internet der Dinge \>IoT Edge-Modul** angezeigt. Der Partner ist für die Auswahl der geeignetsten Unterkategorie für sein Produkt verantwortlich.
