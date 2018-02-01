---
title: Erste Schritte mit Azure Automation | Microsoft-Dokumentation
description: "Dieser Artikel enthält eine Übersicht über den Azure Automation-Dienst. Er beschreibt Details von Entwurf und Implementierung zur Vorbereitung für das Onboarding des Angebots im Azure Marketplace."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: d6ee5c35ce9866f6106c7b5dbc51599b666c3eb1
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2018
---
# <a name="get-started-with-azure-automation"></a>Erste Schritte mit Azure Automation

In diesem Artikel werden wichtige Konzepte zur Bereitstellung von Azure Automation vorgestellt. Wenn Automatisierung in Azure neu für Sie ist oder wenn Sie Erfahrung mit Software zur Automatisierung von Workflows haben (z.B. System Center Orchestrator), erfahren Sie hier etwas über die Vorbereitung und das Onboarding von Automation. Nach dem Lesen dieses Artikels sind Sie in der Lage, Runbooks zur Unterstützung Ihrer Prozessautomatisierungsanforderungen zu entwickeln. 


## <a name="automation-architecture-overview"></a>Übersicht über die Automation-Architektur

![Azure Automation – Übersicht](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure Automation ist eine SaaS-Anwendung (Software-as-a-Service), die eine skalierbare und zuverlässige, mehrinstanzenfähige Umgebung bereitstellt, in der Sie Runbooks zum Automatisieren von Prozessen verwenden können. Sie können die Desired State Configuration (DSC) in Azure, in anderen Clouddiensten oder in einer lokalen Umgebung verwenden, um die Konfigurationsänderungen an Windows- und Linux-Systemen zu verwalten. In Ihrem Automation-Konto enthaltene Entitäten, z.B. Runbooks, Ressourcen und ausführende Konten, sind von anderen Automation-Konten in Ihrem Abonnement und anderen Abonnements isoliert.  

Runbooks, die Sie in Azure ausführen, werden in Automation-Sandboxen ausgeführt. Sandboxen werden in der Azure Plattform als PaaS-VMs (Platform-as-a-Service) gehostet. 

Automation-Sandboxen ermöglichen die Mandantenisolation für alle Aspekte der Runbookausführung, einschließlich Modulen, Speicher, Arbeitsspeicher, Netzwerkkommunikation und Auftragsdatenströmen. Diese Rolle wird vom Dienst verwaltet. Sie können aus Ihrem Azure- oder Automation-Konto nicht auf die Rolle zugreifen oder diese verwalten.         

Um die Bereitstellung und Verwaltung von Ressourcen in Ihrem lokalen Rechenzentrum oder anderen Clouddiensten zu automatisieren, können Sie nach dem Erstellen eines Automation-Kontos einen oder mehrere virtuelle Computer für die Ausführung der Rolle [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) festlegen. Jeder Hybrid Runbook Worker benötigt einen installierten Microsoft-Verwaltungs-Agent sowie ein Automation-Konto. Der Agent muss über eine Verbindung mit einem Azure Log Analytics-Arbeitsbereich verfügen. Sie können Log Analytics für einen Bootstrap der Installation, zum Verwalten des Microsoft-Verwaltungs-Agents und zum Überwachen der Funktion der Hybrid Runbook Worker verwenden. Die Übermittlung von Runbooks und die Anweisung zu ihrer Ausführung erfolgt durch Azure Automation.

Sie können mehrere Hybrid Runbook Worker bereitstellen. Verwenden Sie Hybrid Runbook Worker zur Gewährleistung von Hochverfügbarkeit für Ihre Runbooks und für einen Lastenausgleich zwischen den Runbookaufträgen. In einigen Fällen können Sie die Runbookaufträge bestimmten Workloads oder Umgebungen zuordnen. Der Microsoft Monitoring Agent auf dem Hybrid Runbook Worker initiiert die Kommunikation mit dem Automation-Dienst über den TCP-Port 443. Für Hybrid Runbook Worker gelten keine eingehenden Firewallanforderungen.  

Sie könnten ein Runbook, das auf einem Hybrid Runbook Worker ausgeführt wird, z.B. für Verwaltungsaufgaben für andere Computer oder Dienste in Ihrer Umgebung verwenden. In diesem Szenario benötigt das Runbook auch Zugriff auf andere Ports. Wenn Ihre IT-Sicherheitsrichtlinien Computern in Ihrem Netzwerk Verbindungen mit dem Internet erlauben, informieren Sie sich über das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md). Das OMS-Gateway (Operations Management Suite) fungiert als Proxy für den Hybrid Runbook Worker. Es erfasst den Auftragsstatus und empfängt Konfigurationsinformationen von Ihrem Automation-Konto.

Auf einem Hybrid Runbook Worker ausgeführte Runbooks werden auf dem Computer im Kontext des lokalen Systemkontos ausgeführt. Es wird ein Sicherheitskontext empfohlen, wenn Sie auf dem lokalen Windows-Computer administrative Aktionen ausführen. Wenn Sie mit dem Runbook Aufgaben für Ressourcen außerhalb des lokalen Computers ausführen möchten, müssen Sie möglicherweise sichere Anmeldeinformationsressourcen im Automation-Konto definieren. Sie können auf sichere Anmeldeinformationsressourcen vom Runbook aus zugreifen und diese für eine Authentifizierung bei der externen Ressource verwenden. Sie können die Ressourcen [Anmeldeinformationen](automation-credentials.md), [Zertifikat](automation-certificates.md) und [Verbindung](automation-connections.md) in Ihrem Runbook verwenden. Verwenden Sie die Ressourcen mit Cmdlets, mit denen Sie die Anmeldeinformationen für die Authentifizierung angeben können.

Sie können DSC-Konfigurationen, die in Azure Automation gespeichert sind, auf virtuelle Computer anwenden. Andere physische und virtuelle Computer können Konfigurationen vom Automation DSC-Pullserver anfordern. Sie müssen keine Infrastruktur zur Unterstützung des Automation DSC-Pullservers bereitstellen, um Konfigurationen Ihrer lokalen physischen oder virtuellen Windows- oder Linux-Systeme zu verwalten. Sie benötigen lediglich ausgehenden Internetzugriff von jedem System aus, das Sie mithilfe von Automation DSC verwalten werden. Die Kommunikation erfolgt über den TCP-Port 443 an den OMS-Dienst.   

## <a name="prerequisites"></a>Voraussetzungen

### <a name="automation-dsc"></a>Automation DSC
Sie können mit Automation DSC diese Computer verwalten:

* Virtuelle Azure-Computer (klassisch) mit Windows oder Linux
* Virtuelle Azure-Computer mit Windows oder Linux
* Virtuelle AWS-Computer (Amazon Web Services) mit Windows oder Linux
* Physische und virtuelle Windows-Computer, lokal oder in einer anderen Cloud als Azure oder AWS
* Physische und virtuelle Linux-Computer, lokal oder in einer anderen Cloud als Azure oder AWS

Für Windows-Computer muss die neueste Version von Windows Management Framework (WMF 5) installiert sein. Für Linux-Computer muss die neueste Version von [PowerShell DSC-Agent für Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150) installiert sein. Der PowerShell-DSC-Agent verwendet WMF 5 für die Kommunikation mit Automation. 

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker  
Wenn Sie einen Computer für die Ausführung von Hybrid Runbook-Aufträgen festlegen, muss der Computer die folgenden Voraussetzungen erfüllen:

* Windows Server 2012 oder höher
* Windows PowerShell 4.0 oder höher. Für mehr Zuverlässigkeit wird Windows PowerShell 5.0 empfohlen. Sie können [die neue Version](https://www.microsoft.com/download/details.aspx?id=50395) aus dem Microsoft Download Center herunterladen.
* .NET Framework 4.6.2 oder höher
* Mindestens zwei Kerne
* Mindestens 4 GB RAM

### <a name="permissions-required-to-create-an-automation-account"></a>Erforderliche Berechtigungen zum Erstellen eines Automation-Kontos
Zum Erstellen oder Aktualisieren eines Automation-Kontos und zum Abschließen der Aufgaben in diesem Artikel müssen Sie über die folgenden Berechtigungen verfügen:   
 
* Damit Sie ein Automation-Konto erstellen können, muss Ihr Azure AD-Benutzerkonto (Azure Active Directory) einer Rolle mit Berechtigungen hinzugefügt werden, die der Rolle „Besitzer“ für **Microsoft.Automation**-Ressourcen entspricht. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).  
* Wenn im Azure-Portal unter **Azure Active Directory** > **VERWALTEN** > **App-Registrierungen** die Option **-App-Registrierungen** auf **Ja** festgelegt ist, können Benutzer ohne Administratorrechte in Ihrem Azure AD-Mandanten [Active Directory-Anwendungen registrieren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Wenn **App-Registrierungen** auf **Nein** festgelegt ist, muss ein Benutzer, der diese Aktion ausführt, ein globaler Administrator in Azure AD sein. 

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, werden Sie, bevor Sie der Rolle „Globaler Administrator/Co-Administrator“ des Abonnements hinzugefügt werden, zunächst in Active Directory als Gast hinzugefügt. In diesem Szenario wird auf der Seite **Automation-Konto hinzufügen** diese Meldung angezeigt: „Sie haben keine Berechtigungen zum Erstellen“. 

Wird ein Benutzer zuerst der Rolle des globalen Administrators/Co-Administrators hinzugefügt, können Sie diesen aus der Active Directory-Instanz des Abonnements entfernen, um ihn dann erneut als Vollbenutzer in Active Directory hinzuzufügen.

So überprüfen Sie Benutzerrollen
1. Navigieren Sie im Azure-Portal zum Bereich **Azure Active Directory**.
2. Wählen Sie **Benutzer und Gruppen**.
3. Wählen Sie **Alle Benutzer**. 
4. Wählen Sie nach der Auswahl eines bestimmten Benutzers **Profil** aus. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.

## <a name="authentication-planning"></a>Planen der Authentifizierung
Mit Azure Automation können Sie Aufgaben für Ressourcen in Azure, lokal und in anderen Clouddiensten automatisieren. Damit ein Runbook die erforderlichen Aktionen ausführen kann, muss es über Berechtigungen für den sicheren Zugriff auf die Ressourcen verfügen. Es benötigt die minimalen Berechtigungen innerhalb des Abonnements.  

### <a name="what-is-an-automation-account"></a>Was ist ein Automation-Konto? 
Alle Automatisierungsaufgaben, die Sie für Ressourcen mit den Cmdlets in Azure Automation durchführen, werden gegenüber Azure mit Azure AD basierend auf den Anmeldeinformationen für die Organisationsidentität authentifiziert.  Ein Automation-Konto wird separat von dem Konto verwendet, das Sie zum Anmelden beim Portal zur Konfiguration und Verwendung von Azure-Ressourcen nutzen. 

Die folgenden Ressourcen sind in einem Automation-Konto enthalten:

* **Zertifikate**: Enthält ein Zertifikat für die Authentifizierung über ein Runbook oder eine DSC-Konfiguration. Sie können auch Zertifikate hinzufügen.
* **Verbindungen**. Enthält die erforderlichen Authentifizierungs- und Konfigurationsinformationen für die Herstellung einer Verbindung mit einem externen Dienst oder einer externen Anwendung aus einem Runbook oder einer DSC-Konfiguration.
* **Anmeldeinformationen:** Enthalten ein **PSCredential**-Objekt, das Sicherheitsanmeldeinformationen wie beispielsweise einen Benutzernamen und ein Kennwort umfasst. Die Anmeldeinformationen sind für die Authentifizierung von einem Runbook oder einer DSC-Konfiguration erforderlich.
* **Integrationsmodule:** Die in einem Automation-Konto enthaltenen PowerShell-Module. Verwenden Sie die PowerShell-Module zum Ausführen von Cmdlets in Runbooks und DSC-Konfigurationen.
* **Zeitpläne:** Enthalten Zeitpläne für das Starten oder Beenden eines Runbooks zu einem bestimmten Zeitpunkt, inklusive Wiederholungen.
* **Variablen:** Enthalten Werte, die in einem Runbook oder einer DSC-Konfiguration verfügbar sind.
* **DSC-Konfigurationen:** PowerShell-Skripts, die die Konfiguration einer Betriebssystemfunktion oder -einstellung oder die Installation einer Anwendung auf einem Windows- oder Linux-Computer beschreiben.  
* **Runbooks:** Eine Reihe von Aufgaben, mit denen in Automation auf Grundlage von Windows PowerShell ein automatisierter Prozess durchgeführt wird.    

Automatisierungsressourcen für jedes Automation-Konto sind einer einzelnen Azure-Region zugeordnet. Allerdings können Sie mit Automation-Konten alle Ressourcen in Ihrem Abonnement verwalten. Erstellen Sie Automation-Konten in unterschiedlichen Regionen, wenn Ihre Richtlinien eine Isolierung von Daten und Ressourcen innerhalb einer spezifischen Region vorsehen.

Wenn Sie ein Automation-Konto im Azure-Portal erstellen, werden automatisch zwei Authentifizierungsentitäten erstellt:

* **Ausführendes Konto:** Das Konto führt die folgenden Aufgaben aus:
  - Erstellen eines Dienstprinzipals in Azure AD
  - Erstellen eines Zertifikats
  - Zuweisen der Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC), die zum Verwalten von Azure Resource Manager-Ressourcen mit Runbooks verwendet wird
* **Klassisches ausführendes Konto:** Dieses Konto lädt ein Verwaltungszertifikat hoch. Das Zertifikat wird zum Verwalten von klassischen Ressourcen mithilfe von Runbooks verwendet.

RBAC ist mit Resource Manager verfügbar und ermöglicht das Gewähren zulässiger Aktionen für ein Azure AD-Benutzerkonto und das ausführende Konto. Sie können RBAC auch zum Authentifizieren dieses Dienstprinzipals verwenden. Weitere Informationen und Anleitungen zum Entwickeln eines Modells zum Verwalten von Automation-Berechtigungen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).  

#### <a name="authentication-methods"></a>Authentifizierungsmethoden
In der folgenden Tabelle sind die Authentifizierungsmethoden für die einzelnen Umgebungen zusammengefasst, die von Azure Automation unterstützt werden.

| Methode | Environment 
| --- | --- | 
| Ausführendes Azure-Konto und klassisches ausführendes Konto |Azure Resource Manager- und klassische Azure-Bereitstellung |  
| Azure AD-Benutzerkonto |Azure Resource Manager- und klassische Azure-Bereitstellung |  
| Windows-Authentifizierung |Lokales Rechenzentrum oder anderer Clouddienstanbieter mit Hybrid Runbook Worker-Rolle |  
| Amazon Web Services-Anmeldeinformationen |Amazon Web Services |  

Die folgenden Artikel enthalten eine Übersicht und die Schritte zur Implementierung für das Konfigurieren der Authentifizierung für diese Umgebungen. Die Artikel beschreiben die Verwendung eines vorhandenen und eines neuen Kontos, das Sie der Umgebung zuordnen. 
* [Erstellen eines eigenständigen Azure Automation-Kontos](automation-create-standalone-account.md)
* [Authentifizieren von Runbooks mit klassischer Azure-Bereitstellung und Resource Manager-Bereitstellung](automation-create-aduser-account.md)
* [Authentifizieren von Runbooks mit Amazon Web Services](automation-config-aws-account.md)
* [Aktualisieren des ausführenden Automation-Kontos](automation-create-runas-account.md)

Für das ausführende Azure-Konto und das klassische ausführende Konto wird im Thema [Aktualisieren des ausführenden Automation-Kontos](automation-create-runas-account.md) beschrieben, wie Sie Ihr vorhandenes Automation-Konto mit den ausführenden Konten über das Portal aktualisieren. Außerdem wird die Verwendung von PowerShell beschrieben, falls die ursprüngliche Konfiguration nicht mit einem ausführenden bzw. klassischen ausführenden Konto durchgeführt wurde. Sie können ein ausführendes Konto und ein klassisches ausführendes Konto erstellen, indem Sie ein Zertifikat verwenden, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde. Unter [Aktualisieren des ausführenden Automation-Kontos](automation-create-runas-account.md) erfahren Sie, wie Sie die Konten mithilfe dieser Konfiguration erstellen.     
 
## <a name="network-planning"></a>Planen Ihres Netzwerks
Damit der Hybrid Runbook Worker eine Verbindung mit OMS herstellen und sich bei dem Dienst registrieren kann, benötigt er Zugriff auf die in diesem Abschnitt angegebene Portnummer und die URLs. Dies gilt zusätzlich zu den [für Microsoft Monitoring Agent erforderlichen Ports und URLs](../log-analytics/log-analytics-windows-agent.md) für die Herstellung einer Verbindung mit OMS. 

Wenn Sie einen Proxyserver für die Kommunikation zwischen dem Agent und dem OMS-Dienst verwenden, müssen Sie sicherstellen, dass auf die entsprechenden Ressourcen zugegriffen werden kann. Wenn Sie eine Firewall verwenden, um den Zugriff auf das Internet einzuschränken, müssen Sie die Firewall so konfigurieren, dass der Zugriff möglich ist.

Nachfolgend sind der Port und die URLs aufgeführt, die für die Kommunikation zwischen der Hybrid Runbook Worker-Rolle und Automation erforderlich sind:

* Port: Für ausgehenden Zugriff auf das Internet ist nur TCP 443 erforderlich.
* Globale URL: *.azure-automation.net.

Wenn eines Ihrer Automation-Konten für eine bestimmte Region definiert ist, können Sie die Kommunikation mit diesem regionalen Rechenzentrum einschränken. Die folgende Tabelle enthält den DNS-Eintrag für jede Region.

| **Region** | **DNS-Eintrag** |
| --- | --- |
| USA Süd Mitte |scus-jobruntimedata-prod-su1.azure-automation.net |
| USA (Ost) 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| USA, Westen-Mitte | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Europa, Westen |we-jobruntimedata-prod-su1.azure-automation.net |
| Nordeuropa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Kanada, Mitte |cc-jobruntimedata-prod-su1.azure-automation.net |
| Südostasien |sea-jobruntimedata-prod-su1.azure-automation.net |
| Indien, Mitte |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan, Osten |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australien, Südosten |ase-jobruntimedata-prod-su1.azure-automation.net |
| UK, Süden | uks-jobruntimedata-prod-su1.azure-automation.net |
| US Government, Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Wenn Sie eine Liste mit IP-Adressen der Regionen anstelle ihrer Namen benötigen, können Sie die XML-Datei mit den [IP-Adressen der Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) aus dem Microsoft Download Center herunterladen. 

> [!NOTE]
> Die XML-Datei mit den IP-Adressen der Azure-Rechenzentren enthält die IP-Adressbereiche, die in den Microsoft Azure-Rechenzentren verwendet werden. Die Datei enthält die Bereiche für Compute, SQL und Storage. 
>
>Eine aktualisierte Datei wird wöchentlich veröffentlicht. Die Datei enthält die derzeit bereitgestellten Bereichen und alle anstehenden Änderungen an den IP-Adressbereichen. In der Datei enthaltene neue Bereiche werden frühestens nach einer Woche in den Rechenzentren verwendet. 
>
> Sie sollten die neue XML-Datei jede Woche herunterladen. Führen Sie damit dann eine Aktualisierung an Ihrem Standort durch, um in Azure ausgeführte Dienste ordnungsgemäß zu identifizieren. Diese Datei zum Aktualisieren der BGP-Ankündigung (Border Gateway Protocol) von Azure-Bereichen wird Azure ExpressRoute-Benutzern jeweils in der ersten Woche des Monats angezeigt. 
> 

## <a name="creating-an-automation-account"></a>Erstellen eines Automation-Kontos

In der folgenden Tabelle werden die Methoden zum Erstellen von Automation-Konten im Azure-Portal vorgestellt. In der Tabelle sind die einzelnen Bereitstellungstypen und ihre Unterschiede aufgeführt.  

|Methode | BESCHREIBUNG |
|-------|-------------|
| Auswählen von **Automation & Control** über den Azure Marketplace | Bei diesem Azure Marketplace-Angebot werden ein Automation-Konto und ein OMS-Arbeitsbereich, die miteinander verknüpft sind, in derselben Ressourcengruppe und Region erstellt. Die Integration mit OMS bietet als Vorteil auch die Verwendung von Log Analytics zum Überwachen und Analysieren des Runbook-Auftragsstatus und von Auftragsstreams über die Zeit. Sie können auch die erweiterten Features in Log Analytics verwenden, um Probleme zu eskalieren oder zu untersuchen. Das Angebot stellt die Lösungen für die **Änderungsnachverfolgung** und **Updateverwaltung** bereit, die standardmäßig aktiviert sind. |
| Auswählen von **Automation** über den Marketplace | Bei dieser Methode wird ein Automation-Konto in einer neuen oder vorhandenen Ressourcengruppe erstellt, das nicht mit einem OMS-Arbeitsbereich verknüpft ist. Es enthält keine verfügbaren Lösungen aus dem Angebot **Automation & Control**. Diese Methode stellt eine einfache Konfiguration dar, die einen Einstieg in Automation ermöglicht. Sie können damit lernen, wie Sie Runbooks und DSC-Konfigurationen schreiben und die Funktionen des Diensts verwenden. |
| Auswählen von **Verwaltungslösungen** | Bei Auswahl einer **Verwaltungslösung** wie [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md), [VMs außerhalb der Geschäftszeiten starten/beenden](automation-solution-vm-management.md) oder [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md) werden Sie von der Lösung aufgefordert, ein vorhandenes Automation-Konto und einen OMS-Arbeitsbereich auszuwählen. Die Lösung bietet Ihnen die Möglichkeit, ein Automation-Konto und einen OMS-Arbeitsbereich zu erstellen, damit die Lösung in Ihrem Abonnement bereitgestellt werden kann. |

### <a name="create-an-automation-account-thats-integrated-with-oms"></a>Erstellen eines in OMS integrierten Automation-Kontos
Für die Integration von Automation wird das Angebot **Automation & Control** aus dem Marketplace empfohlen. Bei dieser Methode wird ein Automation-Konto erstellt, und die Integration in einen OMS-Arbeitsbereich wird ebenfalls durchgeführt. Wenn Sie diese Methode verwenden, haben Sie auch die Möglichkeit, die Verwaltungslösungen zu installieren, die Teil des Angebots sind.  

Im Thema [Erstellen eines eigenständigen Automation-Kontos](automation-create-standalone-account.md) wird die Vorgehensweise bei der Erstellung eines Automation-Kontos und OMS-Arbeitsbereichs unter Verwendung des Angebots **Automation & Control** beschrieben. Sie erfahren, wie Sie ein eigenständiges Automation-Konto für Tests oder eine Vorschau des Diensts erstellen.  

So erstellen Sie ein Automation-Konto und einen OMS-Arbeitsbereich mithilfe des Marketplace-Angebots **Automation & Control**

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Wählen Sie **Neu**aus.<br><br> ![Die Option „Neu“ im Azure-Portal auswählen](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Suchen Sie nach **Automation**. Wählen Sie in den Suchergebnissen **Automation & Control** aus.<br><br> ![„Automation & Control“ im Azure Marketplace suchen und auswählen](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png)<br>   
4. Lesen Sie die Beschreibung des Angebots, und wählen Sie dann **Erstellen** aus.  
5. Wählen Sie unter **Automation & Control** die Option **OMS-Arbeitsbereich** aus. Wählen Sie unter **OMS-Arbeitsbereich** einen OMS-Arbeitsbereich aus, der mit demselben Azure-Abonnement verknüpft ist, zu dem das Automation-Konto gehört. Wählen Sie die Option **Neuen Arbeitsbereich erstellen** aus, wenn kein OMS-Arbeitsbereich vorhanden ist. Gehen Sie unter **OMS-Arbeitsbereich** folgendermaßen vor: 
  1. Geben Sie für **OMS-Arbeitsbereich** einen Namen für den neuen Arbeitsbereich ein.
  2. Wählen Sie für **Abonnement** ein Abonnement aus, mit dem eine Verknüpfung hergestellt werden soll. Wenn die Standardauswahl nicht das Abonnement ist, das Sie verwenden möchten, wählen Sie das Abonnement in der Dropdownliste aus.
  3. Unter **Ressourcengruppe** können Sie eine Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen.  
  4. Wählen Sie für **Standort** eine Region aus. Weitere Informationen finden Sie auf der Seite zur [Verfügbarkeit von Azure Automation in den einzelnen Regionen](https://azure.microsoft.com/regions/services/). Lösungen sind für zwei Tarife verfügbar: „Free“ und „Pro Knoten (OMS)“. Beim Free-Tarif gelten Beschränkungen für die Datenmenge, die pro Tag erfasst werden kann, die Aufbewahrungsdauer und die Laufzeit von Runbookaufträgen in Minuten. Für den Tarif „Pro Knoten (OMS)“ gilt keine Beschränkung für die täglich erfasste Datenmenge.  
  5. Wählen Sie die Option **Automation-Konto**.  Wenn Sie einen neuen OMS-Arbeitsbereich erstellen, müssen Sie auch ein Automation-Konto erstellen, das mit dem neuen OMS-Arbeitsbereich verknüpft ist. Schließen Sie Ihr Azure-Abonnement, die Ressourcengruppe und die Region ein. 
    1. Wählen Sie **Automation-Konto erstellen** aus.
    2. Geben Sie unter **Automation-Konto** im Feld **Name** den Namen des Automation-Kontos ein.
    Alle anderen Optionen werden basierend auf dem OMS-Arbeitsbereich automatisch aufgefüllt. Sie können diese Optionen nicht ändern. 
    3. Ein ausführendes Azure-Konto ist die standardmäßig verwendete Authentifizierungsmethode für das Angebot. Nach dem Auswählen von **OK** werden die Konfigurationsoptionen überprüft, und das Automation-Konto wird erstellt. Wählen Sie zum Nachverfolgen des Fortschritts im Menü die Option **Benachrichtigungen** aus. 
    4. Wählen Sie andernfalls ein vorhandenes ausführendes Automation-Konto aus. Das ausgewählte Konto darf nicht bereits mit einem anderen OMS-Arbeitsbereich verknüpft sein. Andernfalls wird eine Meldung angezeigt. Wenn das Konto bereits mit einem OMS-Arbeitsbereich verknüpft ist, müssen Sie ein anderes ausführendes Automation-Konto auswählen oder ein Konto erstellen.
    5. Wählen Sie nach dem Eingeben oder Auswählen der erforderlichen Informationen **Erstellen** aus. Die Informationen werden überprüft, und das Automation-Konto und die ausführenden Konten werden erstellt. Sie gelangen automatisch wieder zurück zum Bereich **OMS-Arbeitsbereich**.  
6. Wählen Sie nach dem Eingeben oder Auswählen der erforderlichen Informationen im Bereich **OMS-Arbeitsbereich** die Option **Erstellen** aus.  Die Informationen werden überprüft, und der Arbeitsbereich wird erstellt. Wählen Sie zum Nachverfolgen des Fortschritts im Menü die Option **Benachrichtigungen** aus. Sie gelangen zurück zum Bereich **Lösung hinzufügen**.  
7. Bestätigen Sie unter **Automation & Control**, dass Sie die empfohlenen vorab ausgewählten Lösungen installieren möchten. Wenn Sie eine der Standardoptionen ändern, können Sie die Lösungen einzeln zu einem späteren Zeitpunkt installieren.  
8. Wählen Sie **Erstellen** aus, um mit dem Onboarding von Automation und eines OMS-Arbeitsbereichs zu beginnen. Alle Einstellungen werden überprüft, und anschließend versucht Azure, das Angebot in Ihrem Abonnement bereitzustellen. Dieser Vorgang kann mehrere Sekunden dauern. Wählen Sie zum Nachverfolgen des Fortschritts im Menü die Option **Benachrichtigungen** aus. 

Nach dem Onboarding des Angebots können Sie die folgenden Aufgaben ausführen:
* Beginnen Sie mit dem Erstellen von Runbooks.
* Arbeiten Sie mit den Verwaltungslösungen, die Sie aktiviert haben.
* Stellen Sie eine [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)-Rolle bereit.
* Beginnen Sie mit der Arbeit mit [Log Analytics](https://docs.microsoft.com/azure/log-analytics), um Daten zu sammeln, die von Ressourcen in Ihrer Cloud oder Ihrer lokalen Umgebung generiert werden.   

## <a name="next-steps"></a>Nächste Schritte
* Überprüfen Sie, ob Ihr neues Automation-Konto eine Authentifizierung für Azure-Ressourcen durchführen kann. Weitere Informationen finden Sie unter [Testen der Authentifizierung für das ausführende Azure Automation-Konto](automation-verify-runas-authentication.md).
* Lernen Sie die ersten Schritte beim Erstellen von Runbooks sowie die zugehörigen Überlegungen kennen. Weitere Informationen finden Sie unter [Automation-Runbooktypen](automation-runbook-types.md).


