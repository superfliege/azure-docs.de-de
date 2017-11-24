---
title: Sicherheit in Azure IoT Edge | Microsoft-Dokumentation
description: "Sicherheit, Authentifizierung und Autorisierung von IoT Edge-Geräten"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 8a5bf1f35fcdd779cf27edeba7dfd5705cbae205
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="securing-azure-iot-edge---preview"></a>Sichern von Azure IoT Edge – Vorschau

Das intelligente Edge muss gesichert werden, um Vertrauen in den Betrieb einer End-to-End-IoT-Lösung aufzubauen. Azure IoT Edge wurde im Hinblick auf ein Sicherheitskonzept entwickelt, das sich auf verschiedene Risikoprofile und Bereitstellungsszenarien ausweiten lässt, und bietet denselben umfangreichen Schutz, den Sie von allen Azure-Diensten kennen.

Azure IoT Edge kann auf verschiedenen Hardwaregeräten ausgeführt werden, unterstützt sowohl Linux als auch Windows und lässt sich auf verschiedene Bereitstellungsszenarien anwenden.  Bei der Risikobewertung spielen verschiedene Faktoren eine Rolle, z.B. die Besitzrechte für die Lösung, die geografische Region für die Bereitstellung, die Vertraulichkeit der Daten, die Branche, in der die Anwendung eingesetzt wird, sowie gesetzliche Vorgaben.  Anstatt konkrete Lösungen für bestimmte Szenarien anzubieten, ist es sinnvoller, ein erweiterbares Sicherheitsframework zu entwerfen, das auf fundierten Prinzipien fußt und für den Einsatz im großen Maßstab konzipiert wird. 
 
Dieser Artikel bietet eine Übersicht über das Sicherheitsframework. Weitere Informationen finden Sie unter [Securing the intelligent edge][lnk-edge-blog] (Sichern des intelligenten Edge).

>[!NOTE]
>Das im Folgenden beschriebene Sicherheitsframework wird derzeit zum Produkt hinzugefügt und wird mit dem allgemein verfügbaren Release von Azure IoT Edge zur Verfügung stehen. Das Produkt befindet sich zurzeit in der öffentlichen Vorschau – dieses Release dient zur Entwicklung und zum Erstellen von Prototypen von Edgelösungen und ist nicht für vollständige Produktionsentwicklungen gedacht, für die das vollständige Sicherheitsframework erforderlich ist.   

## <a name="standards"></a>Standards

Standards fördern eine einfache Überprüfbarkeit und Implementierung – das Gütesiegel der Sicherheit.  Eine gut konzipierte Sicherheitslösung sollte zur Überprüfbarkeit beitragen, um Vertrauen aufzubauen, und darf kein Hindernis bei der Bereitstellung sein.  Das Design des Frameworks für die Sicherung von Azure IoT Edge basiert auf Sicherheitsprotokollen, die über lange Zeit getestet wurden und sich in der Branche bewährt haben – so profitieren Benutzer von vertrauten und wiederverwendbaren Features. 

## <a name="authentication"></a>Authentifizierung

Das sichere Wissen, welche Akteure, Geräte und Komponenten in einer End-to-End-IoT-Lösung an der die Schöpfung von Mehrwert beteiligt sind, ist von allergrößter Bedeutung, um Vertrauen aufzubauen.  Dieses Wissen sorgt für Verantwortlichkeit bei den Beteiligten und ist die Grundlage für die Gewährung des Zugriffs.  Azure IoT Edge erzielt dieses Wissen durch Authentifizierung.  Der primäre Authentifizierungsmechanismus für die Azure IoT Edge-Plattform ist die zertifikatbasierte Authentifizierung.  Dieser Mechanismus wurde aus einer Reihe von Standards abgeleitet, die die Public Key Infrastructure (PKiX) der Internet Engineering Task Force (IETF) regeln.     

Das Azure IoT Edge-Sicherheitsframework fordert eindeutige Zertifikatidentitäten für alle Geräte, Module (Container, die die Logik im Gerät kapseln) und Akteure, die mit dem Azure IoT Edge-Gerät interagieren – entweder physisch oder über eine Netzwerkverbindung.  Nicht jedes Szenario oder jede Komponente eignet sich für die zertifikatbasierte Authentifizierung. Für diese Szenarien und Komponenten bietet die Erweiterbarkeit des Sicherheitsframeworks andere sichere Möglichkeiten. 

## <a name="authorization"></a>Autorisierung

Die Fähigkeit, die Autorität zu delegieren und den Zugriff zu steuern, ist unabdingbar, um ein grundlegendes Sicherheitsprinzip umzusetzen: das Prinzip der geringsten Berechtigung.  Geräte, Module und Akteure dürfen nur auf Ressourcen und Daten in ihrem Berechtigungsbereich zugreifen – und auch nur dann, wenn die Architektur es zulässt.  Das bedeutet, dass einige Berechtigungen mit ausreichenden Rechten konfigurierbar sind, andere dagegen architekturbedingt erzwungen werden.  Ein Beispiel: Ein Modul ist möglicherweise per Berechtigungskonfiguration dazu autorisiert, eine Verbindung mit Azure IoT Hub zu starten. Es gibt aber keinen Grund dafür, dass ein Modul in einem Azure IoT Edge-Gerät auf den Zwilling eines Moduls in einem anderen Azure IoT Edge-Gerät zugreifen sollte.  Aus diesem Grund wird Letzteres durch die Architektur ausgeschlossen. 

Weitere Autorisierungskonzepte umfassen Rechte zum Signieren von Zertifikaten und die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC).  Die Erweiterbarkeit des Sicherheitsframeworks gestattet die Übernahme anderer ausgereifter Autorisierungskonzepte. 

## <a name="attestation"></a>Nachweis

Ein Nachweis stellt die Integrität von Softwarebestandteilen sicher.  Er ist wichtig für die Erkennung und Verhinderung von Schadsoftware.  Das Azure IoT Edge-Sicherheitsframework klassifiziert Nachweise in drei Hauptkategorien:

* Statischer Nachweis
* Nachweis zur Laufzeit
* Softwarenachweis

### <a name="static-attestation"></a>Statischer Nachweis

Ein statischer Nachweis ist die Bestätigung der Integrität aller Softwarebestandteile, aller Laufzeiten und der Konfigurationsinformationen beim Gerätestart.  Er wird oft als sicherer Start bezeichnet.  Das Sicherheitsframework für Azure IoT Edge-Geräte wird auch von Hardwareherstellern angewendet und umfasst sichere, tief in die Hardware eingebettete Funktionen, um statische Nachweisprozesse sicherzustellen. Diese Prozesse umfassen sichere Prozesse für Gerätestart und Firmwareupgrades.  Eine enge Zusammenarbeit mit Hardwareherstellern verhindert überflüssige Firmwareschichten und minimiert damit die Angriffsfläche. 

### <a name="runtime-attestation"></a>Nachweis zur Laufzeit

Sobald ein System einen validierten Startprozess abgeschlossen hat und betriebsbereit ist, erkennen sorgfältig entwickelte Sicherheitssysteme Versuche, Schadsoftware über die Systemports und -schnittstellen einzuschleusen, und leiten entsprechende Gegenmaßnahmen ein.  Bei intelligenten Edgegeräten, die sich in physischem Besitz eines böswilligen Benutzers befinden, ist es möglich, schädliche Inhalte über andere Wege als die Geräteschnittstellen einzuschleusen, beispielsweise durch Manipulation und Seitenkanalangriffe.   Solche schädlichen Inhalte, die in Form von Schadsoftware oder nicht autorisierten Konfigurationsänderungen auftreten können, werden normalerweise vom sicheren Startprozess nicht entdeckt, weil sie erst nach dem Startprozess ins System gelangen.  Gegenmaßnahmen, die von der Gerätehardware angeboten oder erzwungen werden, tragen maßgeblich dazu bei, solche Bedrohungen abzuwehren.  Das Sicherheitsframework für Azure IoT Edge fordert explizit Erweiterungen zur Bekämpfung von Laufzeitbedrohungen.     

### <a name="software-attestation"></a>Softwarenachweis

Alle integren Systeme, einschließlich intelligenter Edgesysteme müssen für Patches und Upgrades zugänglich sein.  Sicherheit ist wichtig für die Updateprozesse, da diese sonst potenzielle Bedrohungsvektoren sein können.  Das Sicherheitsframework für Azure IoT Edge fordert, dass Updates über bewertete und signierte Pakete erfolgen, um die Integrität sicherzustellen und die Quelle der Pakete zu authentifizieren.  Dies gilt für alle Betriebssysteme und sämtliche Bestandteile von Softwareanwendungen. 

## <a name="hardware-root-of-trust"></a>Vertrauensanker in Hardware

In vielen Bereitstellungen intelligenter Edgegeräte – insbesondere bei Geräten, die an Orten bereitgestellt werden, an denen potenziell böswillige Benutzer physischen Zugriff auf das Gerät haben – ist die Sicherheit, die durch die Gerätehardware geboten wird, die letzte Verteidigungslinie.  Aus diesem Grund ist es von entscheidender Bedeutung für solche Bereitstellungen, das Vertrauen in manipulationssicherer Hardware zu verankern.  Um die Anforderungen des Sicherheitsframeworks für Azure IoT Edge zu erfüllen, arbeiten Anbieter von sicherer Hardware zusammen und stellen verschiedene Arten von Hardwarevertrauensankern für die verschiedensten Risikoprofile und Bereitstellungsszenarien bereit. Hierzu gehört der Einsatz von sicherer Hardware, die allgemeine Sicherheitsprotokollstandards wie das Trusted Platform Module (ISO/IEC 11889) und das Device Identifier Composition Engine (DICE) der Trusted Computing Group einhält.  Hierzu gehören auch sichere Enklaventechnologien wie TrustZones and Software Guard Extensions (SGX). 

## <a name="certification"></a>Zertifizierung

Damit Kunden beim Erwerb von Azure IoT Edge-Geräten fundierte Entscheidungen für ihre Bereitstellung treffen können, enthält das Azure IoT Edge-Framework Zertifizierungsanforderungen.  Die Grundlage dieser Anforderungen sind Zertifizierungen in Bezug auf Sicherheitserfordernisse und Zertifizierungen in Bezug auf die Validierung der Sicherheitsimplementierung.  Eine Zertifizierung in Bezug auf Sicherheitserfordernisse informiert beispielsweise darüber, dass für das IoT Edge-Gerät sichere Hardware eingesetzt wird, die Startangriffe bekanntermaßen abwehrt. Eine Validierungszertifizierung informiert darüber, dass die sichere Hardware ordnungsgemäß implementiert wurde, sodass dieses Feature im Gerät wirklich genutzt werden kann.  Im Einklang mit dem Prinzip der Einfachheit bietet das Framework die Vision, den Aufwand für Zertifizierungen so gering wie möglich zu halten.   

## <a name="extensibility"></a>Erweiterbarkeit

Erweiterbarkeit ist ein sehr wichtiger Aspekt im Azure IoT Edge-Sicherheitsframework.  Angesichts der Tatsache, dass die IoT-Technologie verschiedene Formen von geschäftlichen Transformationen fördert, leuchtet es ein, dass Sicherheitsmaßnahmen parallel und nahtlos weiterentwickelt werden müssen, um neue Szenarien abzudecken.  Das Azure IoT Edge-Sicherheitsframework startet mit einem soliden Fundament, auf dem die Erweiterbarkeit in verschiedene Dimensionen aufgebaut wird, damit folgende Aspekte berücksichtigt werden: 

* Sicherheitsdienste des Erstanbieters, wie etwa den Device Provisioning-Dienst für Azure IoT Hub.
* Drittanbieterdienste wie z.B. verwaltete Sicherheitsdienste für verschiedene Branchen (z.B. Industrie oder Gesundheitswesen) oder technologische Fokusbereiche (z.B. Sicherheitsüberwachung in Meshnetzwerken oder Hardwarenachweisdienste) über ein umfassendes Partnernetzwerk.
* Ältere Systeme, die mit alternativen Sicherheitsstrategien umgerüstet werden müssen, indem z.B. eine andere Sicherheitstechnologie als Zertifikate für die Authentifizierung und Identitätsverwaltung verwendet wird.
* Sichere Hardware für die nahtlose Einführung neuer sicherer Hardwaretechnologien und Beiträge von Hardwarepartnern.

Dies sind nur einige Beispiele für mögliche Erweiterungsdimensionen, und das Azure IoT Edge-Sicherheitsframework ist von Grund auf für Sicherheit konzipiert, um diese Erweiterbarkeit zu unterstützen. 

Schlussendlich resultiert der größte Erfolg beim Sichern des intelligenten Edge aus gemeinsamen Beiträgen einer offenen Community, die vom gemeinsamen Interesse an der Sicherheit des Internets der Dinge angetrieben wird.  Bei diesen Beiträgen kann es sich um sichere Technologien oder sichere Dienste handeln.  Das Azure IoT Edge-Sicherheitsframework bietet ein solides Fundament für die Sicherheit, das erweiterbar ist, um im intelligenten Edge das gleiche Maß an Vertrauen und Integrität bereitzustellen wie in der Azure-Cloud.  

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Sicherheitsprinzipien von Azure IoT Edge im Blog [Securing the Intelligent Edge][lnk-edge-blog].

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/ 