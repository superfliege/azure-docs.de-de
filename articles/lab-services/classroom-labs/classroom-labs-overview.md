---
title: Informationen zu Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Lab Services das Erstellen, Verwalten und Schützen von Labs mit virtuellen Computern vereinfachen können, die von Entwicklern, Testern, Lehrkräften, Kursteilnehmern und anderen genutzt werden können.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/21/2018
ms.author: spelluru
ms.openlocfilehash: e9c3cae7c7129cc489ddd38b5b2de18dd6f52e58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660528"
---
# <a name="introduction-to-classroom-labs"></a>Einführung in Classroom-Labs
Mithilfe von Azure Lab Services können Sie schnell eine Classroom-Lab-Umgebung in der Cloud einrichten. Ein Lehrer/Dozent erstellt ein Classroom-Lab, stellt virtuelle Windows- oder Linux-Computer bereit, installiert die erforderlichen Programm- und Tool-Labs in der Klasse und macht sie für Schüler/Studenten verfügbar. Die Schüler/Studenten stellen eine Verbindung mit virtuellen Computern (virtual machines, VMs) im Lab her und nutzen sie für ihre Projekte, Aufgaben oder Classroom-Übungen. 

Die Classroom-Labs sind verwaltete Labs und werden von Azure verwaltet. Der Dienst übernimmt die gesamte Infrastrukturverwaltung für ein verwaltetes Lab – vom Hochfahren der virtuellen Computer (virtual machines, VMs) über die Fehlerbehandlung bis hin zur Skalierung der Infrastruktur. Sie geben an, welche Art von Infrastruktur Sie benötigen, und installieren alle Tools und Programme, die für die Klasse benötigt werden. Verwaltete Labs befinden sich derzeit in der Vorschauphase.  

## <a name="scenarios"></a>Szenarien
Classroom-Labs von Azure Lab Services unterstützen das folgende Hauptszenario: 

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Einrichtung eines Computerlabors mit anpassbarer Größe in der Cloud für Ihren Classroom  

- Erstellen Sie ein verwaltetes Classroom-Lab. Geben Sie einfach an, was Sie benötigen. Der Dienst erstellt und verwaltet daraufhin automatisch die Infrastruktur, sodass Sie sich auf das Unterrichten konzentrieren können und sich nicht um die technischen Details eines Labs kümmern müssen. 
- Stellen Sie den Teilnehmern ein Lab mit virtuellen Computern zur Verfügung, die exakt für ihre Anforderungen konfiguriert sind. Weisen Sie jedem Teilnehmer eine begrenzte Anzahl von Stunden für die Nutzung der VMs für die Unterrichtsarbeit zu.  
- Verlagern Sie das physische Computerlabor Ihrer Einrichtung in die Cloud. Skalieren Sie die Anzahl der VMs automatisch nur bis zur maximalen Auslastung und Kostenschwelle, die Sie für das Lab festgelegt haben. 
- Löschen Sie das Lab mit nur einem Mausklick, wenn Sie es nicht mehr benötigen. 

## <a name="user-profiles"></a>Benutzerprofile
In diesem Artikel werden verschiedene Benutzerprofile in Azure Lab Services beschrieben. 

### <a name="lab-account-owner"></a>Lab-Kontobesitzer
Typischerweise fungiert der IT-Administrator der Cloudressourcen des Unternehmens, der das Azure-Abonnement besitzt, als Besitzer eines Lab-Kontos und erledigt die folgenden Aufgaben:   

- Einrichten eines Lab-Dienstkontos für Ihre Organisation
- Verwalten und Konfigurieren von Richtlinien in allen Labs
- Erteilen von Berechtigungen an Personen in der Organisation zum Erstellen eines Lab unter dem Lab-Konto

### <a name="educator"></a>Lehrer/Dozent
Üblicherweise erstellen Benutzer (etwa eine Lehrkraft oder ein Onlinetrainer) Classroom-Labs unter einem Labkonto. Ein Lehrer/Dozent führt die folgenden Aufgaben aus: 

- Erstellen eines Classroom-Labs
- Erstellen virtueller Computer im Lab 
- Installieren der auf virtuellen Computern benötigten Software
- Angeben, wer auf das Lab zugreifen darf
- Bereitstellen eines Registrierungslinks zum Lab für Schüler/Studenten

### <a name="student"></a>Student
Ein Schüler/Student führt die folgenden Aufgaben aus:

- Klicken auf den Registrierungslink, den der Lab-Benutzer vom Lab-Ersteller erhält, um sich beim Lab zu registrieren 
- Herstellen einer Verbindung mit einem virtuellen Computer im Lab zur Verwendung für Klassentätigkeiten, Aufgaben und Projekte 

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit dem Einrichten eines Labkontos vertraut, das benötigt wird, um ein Classroom-Lab unter Verwendung von Azure Lab Services zu erstellen:

- [Einrichten eines Lab-Kontos](tutorial-setup-lab-account.md)
