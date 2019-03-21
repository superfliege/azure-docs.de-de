---
title: Sicherheitsleitfaden für Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erläutert die zahlreichen Methoden zum Schützen von Azure Storage, einschließlich, aber nicht beschränkt auf RBAC und Speicherdienstverschlüsselung.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 02/07/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 22b070e6d70208057c85ad6a2322cc440d12a0fa
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58008212"
---
# <a name="azure-data-lake-storage-gen2-security-guide"></a>Sicherheitsleitfaden zu Azure Data Lake Storage Gen2

Azure Data Lake Storage Gen2 besteht aus einer Reihe von Funktionen, die auf Azure Storage-Konten basieren. Daher beziehen sich alle Verweise in diesem Artikel auf ein Azure Storage-Konto mit aktiviertem hierarchischem Namespace (Data Lake Storage Gen2-Funktionen).

- Alle Daten werden automatisch mit [Storage Service Encryption (SSE)](storage-service-encryption.md) verschlüsselt, wenn sie in Azure Storage geschrieben werden. Weitere Informationen finden Sie unter [Announcing Default Encryption for Azure Blobs, Files, Table and Queue Storage](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/) (Ankündigung: Standardverschlüsselung für Azure Blobs, Files, Tables und Queue Storage).
- Azure Active Directory (Azure AD) und die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) werden für Azure Storage sowohl für Ressourcenverwaltungsvorgänge als auch für Datenvorgänge wie folgt unterstützt:
    - Sie können RBAC-Rollen, die auf das Speicherkonto beschränkt sind, Dienstprinzipalen zuweisen und Azure AD verwenden, um Ressourcenverwaltungsvorgänge, z.B. die Schlüsselverwaltung, zu autorisieren.
    - Die Azure AD-Integration wird in Datenvorgängen für Azure Storage unterstützt. Sie können RBAC-Rollen, die sich auf ein Abonnement, eine Ressourcengruppe, ein Speicherkonto oder ein einzelnes Dateisystem beziehen, einem Sicherheitsprinzipal oder einer verwalteten Identität für Azure-Ressourcen zuweisen. Weitere Informationen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory](storage-auth-aad.md).
- Delegierter Zugriff auf die Datenobjekte in Azure Storage kann mit [Shared Access Signatures](../storage-dotnet-shared-access-signature-part-1.md)erteilt werden.

Dieser Artikel bietet eine Übersicht über alle Sicherheitsfunktionen, die mit Azure Storage verwendet werden können. Links führen Sie zu Artikeln, die weitere Informationen zu den einzelnen Funktionen enthalten. So können Sie Ihre Kenntnisse zu jedem Thema problemlos vertiefen.

Folgende Themen werden in diesem Artikel abgedeckt:

* [Sicherheit auf Verwaltungsebene](#management-plane-security) – Sichern Ihres Speicherkontos

  Die Verwaltungsebene besteht aus den Ressourcen, die zum Verwalten Ihres Speicherkonto verwendet werden. In diesem Abschnitt erfahren Sie mehr über das Azure Resource Manager-Bereitstellungsmodell und wie Sie mit der rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) den Zugriff auf Ihre Speicherkonten verwalten. Zudem erfahren Sie mehr über die Verwaltung Ihrer Speicherkontoschlüssel und deren erneute Generierung.
* [Sicherheit auf Datenebene](#data-plane-security) – Sichern des Zugriffs auf Ihre Daten

  In diesem Abschnitt betrachten wir die Gewährung des Zugriffs auf die tatsächlichen Datenobjekte in Ihrem Speicherkonto (z.B. Dateien und Verzeichnisse) mit SAS und gespeicherten Zugriffsrichtlinien. Wir betrachten SAS sowohl auf Dienst- als auch auf Kontoebene. Wir behandeln auch die Beschränkung des Zugriffs auf eine bestimmte IP-Adresse (oder einen Bereich von IP-Adressen), die Beschränkung des verwendeten HTTPS-Protokolls und das Widerrufen einer SAS, ohne ihren Ablauf abzuwarten.
* [Verschlüsselung während der Übertragung](#encryption-in-transit)

In diesem Abschnitt wird erläutert, wie Sie Daten sichern, wenn Sie sie in ein oder aus einem Speicherkonto übertragen, wenn Data Lake Storage Gen2 aktiviert ist. Wir erläutern die empfohlene Verwendung von HTTPS.

## <a name="management-plane-security"></a>Sicherheit auf Verwaltungsebene

Auf der Verwaltungsebene finden Vorgänge statt, die sich auf das Speicherkonto selbst auswirken. Angenommen, Sie können ein Speicherkonto erstellen oder löschen, eine Liste von Speicherkonten in einem Abonnement abrufen, die Speicherkontoschlüssel abrufen oder die Speicherkontoschlüssel erneut generieren.

Dieser Leitfaden konzentriert sich auf das Resource Manager-Modell der Bereitstellung, das das Mittel der Wahl zum Erstellen von Speicherkonten mit Data Lake Storage Gen2-Funktionen ist. Mit den Resource Manager-Speicherkonten können Sie, anstatt Zugriff auf das gesamte Abonnement zu gewähren, den Zugriff auf die Verwaltungsebene mit der rollenbasierten Zugriffssteuerung präziser steuern.

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Sichern Ihres Speicherkontos mit rollenbasierter Zugriffssteuerung (RBAC)

Was ist RBAC, und wie können Sie sie verwenden? Jedes Azure-Abonnement hat ein Azure Active Directory. Benutzern, Gruppen und Anwendungen aus diesem Verzeichnis kann Verwaltungszugriff auf Ressourcen im zugehörigen Azure-Abonnement gewährt werden, die das Resource Manager-Bereitstellungsmodell verwenden. Dieser Sicherheitstyp wird als rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) bezeichnet. Verwenden Sie zum Verwalten dieses Zugriffs **Zugriffssteuerung (IAM)** im Azure-Portal.

Mit dem Resource Manager-Modell platzieren Sie das Speicherkonto in einer Ressourcengruppe und steuern den Zugriff auf die Verwaltungsebene dieses bestimmten Speicherkontos mithilfe von Active Directory. Beispielsweise können Sie bestimmten Benutzern den Zugriff auf die Speicherkontoschlüssel gewähren, während andere Benutzer Informationen über das Speicherkonto anzeigen, jedoch nicht auf die Speicherkontoschlüssel zugreifen können.

#### <a name="granting-access"></a>Gewähren von Zugriff

Der Zugriff wird gewährt, indem Benutzern, Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle auf der richtigen Ebene zugewiesen wird. Um Zugriff auf das gesamten Abonnement zu gewähren, weisen Sie eine Rolle auf Abonnementebene zu. Sie können den Zugriff auf alle Ressourcen in einer Ressourcengruppe gewähren, indem Sie Berechtigungen für die Ressourcengruppe selbst gewähren. Sie können auch bestimmten Ressourcen, z. B. Speicherkonten, bestimmte Rollen zuweisen.

Hier sind die wichtigsten Punkte, die Sie über die Verwendung von RBAC für den Zugriff auf die Verwaltungsvorgänge eines Azure Storage-Kontos wissen müssen:

* Um einer Person die Berechtigung zum Zugriff auf die Datenobjekte im Speicherkonto zu gewähren, können Sie ihr die Berechtigung zum Lesen der Speicherkontoschlüssel gewähren, und dieser Benutzer kann dann mit diesen Schlüsseln auf die Dateien und Verzeichnisse zugreifen.
* Rollen können einem bestimmten Benutzerkonto, einer Gruppe von Benutzern oder einer bestimmten Anwendung zugewiesen werden.
* Jede Rolle verfügt über eine Liste zulässiger Aktionen und nicht zulässiger Aktionen. Beispielsweise verfügt die Rolle „Mitwirkender für virtuelle Computer“ über die Aktion „listKeys“, die das Lesen der Speicherkontoschlüssel ermöglicht. Zu den nicht zulässigen Aktionen des Mitwirkenden zählt z.B. die Aktualisierung des Zugriffs für Benutzer in Active Directory.
* Zu den Rollen für Speicher zählen (unter anderem) die folgenden Rollen:

  * Besitzer – Sie können alles, einschließlich des Zugriffs, verwalten.
  * Mitwirkende – Sie können alles, was der Besitzer kann, abgesehen vom Zuweisen des Zugriffs. Benutzer mit dieser Rolle können Speicherkontoschlüssel anzeigen und erneut generieren. Mit den Speicherkontoschlüsseln können sie auf die Datenobjekte zugreifen.
  * Leser – Sie können Informationen über das Speicherkonto, mit Ausnahme geheimer Schlüssel, anzeigen. Wenn Sie z.B. einer Person eine Rolle mit Leseberechtigungen für das Speicherkonto zuweisen, kann sie die Eigenschaften des Speicherkontos anzeigen, aber nicht die Eigenschaften ändern oder den Schlüssel des Speicherkontos anzeigen.
  * Speicherkontomitwirkende – Sie können das Speicherkonto verwalten – Ressourcengruppen und Ressourcen des Abonnements lesen, außerdem Abonnementressourcengruppen-Bereitstellungen erstellen und verwalten. Sie können ebenfalls auf die Schlüssel des Speicherkontos zugreifen, was wiederum bedeutet, dass sie auf die Datenebene zugreifen können.
  * Benutzerzugriffsadministrator – Sie können den Benutzerzugriff auf das Speicherkonto verwalten. Beispielsweise können sie einem bestimmten Benutzer Leserzugriff gewähren.
  * Mitwirkende für virtuelle Computer – Sie können virtuelle Computer verwalten, jedoch nicht das Speicherkonto, mit dem sie verbunden sind. Diese Rolle kann den Schlüssel des Speicherkontos auflisten, was bedeutet, dass der Benutzer, dem Sie diese Rolle zuweisen, die Datenebene aktualisieren kann.

    Damit ein Benutzer einen virtuellen Computer erstellen kann, muss er die entsprechende VHD-Datei in einem Speicherkonto erstellen können. Zu diesem Zweck muss er den Speicherkontoschlüssel abrufen und der API übergeben können, die die VM erstellt. Daher benötigt er diese Berechtigung, damit er den Schlüssel des Speicherkontos auflisten kann.
* Die Möglichkeit zum Definieren benutzerdefinierter Rollen ist ein Feature, mit dem Sie aus einer Liste verfügbarer Aktionen einen Satz von Aktionen auswählen können, die an Azure-Ressourcen ausgeführt werden dürfen.
* Der Benutzer muss in Ihrem Azure Active Directory eingerichtet werden, damit Sie ihm eine Rolle zuweisen können.
* Sie können einen Bericht darüber erstellen, wer wem welche Art des Zugriffs – und in welchem Bereich – mit PowerShell oder der Azure-Befehlszeilenschnittstelle gewährt/entzogen hat.

#### <a name="resources"></a>Ressourcen

* [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../../role-based-access-control/role-assignments-portal.md)

  Dieser Artikel beschreibt die rollenbasierte Steuerung des Zugriffs auf Azure Active Directory, und wie sie funktioniert.
* [RBAC: Integrierte Rollen](../../role-based-access-control/built-in-roles.md)

  In diesem Artikel werden alle in RBAC verfügbaren integrierten Rollen ausführlich beschrieben.
* [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../../azure-resource-manager/resource-manager-deployment-model.md)

### <a name="managing-your-storage-account-keys"></a>Verwalten Ihres Speicherkontoschlüssels

Speicherkontoschlüssel sind von Azure erstellte 512-Bit-Zeichenfolgen, die zusammen mit dem Speicherkontonamen für den Zugriff auf die im Speicherkonto gespeicherten Datenobjekte, z.B. Blobs, Entitäten innerhalb einer Tabelle, Warteschlangennachrichten und Dateien in einer Azure-Dateifreigabe verwendet werden können. Mit dem Steuern des Zugriffs auf die Speicherkontoschlüssel wird der Zugriff auf die Datenebene für das Speicherkonto gesteuert.

Jedes Speicherkonto verfügt über zwei Schlüssel, die im [Azure-Portal](https://portal.azure.com/) und in den PowerShell-Cmdlets „Key 1“ und „Key 2“ genannt werden. Diese können mit verschiedenen Methoden manuell erneut generiert werden, beispielsweise im [Azure-Portal](https://portal.azure.com/), mithilfe von PowerShell oder über die Azure-Befehlszeilenschnittstelle. Die Neugenerierung kann auch programmgesteuert mithilfe der .NET-Speicherclientbibliothek bzw. der REST-API der Azure Storage-Dienste erfolgen.

Es gibt verschiedene Gründe, Ihre Speicherkontoschlüssel erneut zu generieren.

* Möglicherweise generieren Sie sie in regelmäßigen Abständen aus Sicherheitsgründen erneut.
* Sie würden Ihre Speicherkontoschlüssel z. B. dann erneut generieren, wenn jemand eine Anwendung gehackt und den Schlüssel abgerufen hat, der hartcodiert oder in einer Konfigurationsdatei gespeichert war, sodass er jetzt vollständigen Zugriff auf Ihr Speicherkonto hat.
* Ein weiterer Fall für die Neugenerierung des Schlüssels ist, wenn Ihr Team eine Storage-Explorer-Anwendung verwendet, die den Speicherkontoschlüssel beibehält, und eines der Mitglieder verlässt das Team. Die Anwendung würde nach seinem Ausscheiden auch weiterhin funktionieren und ihm den Zugriff auf Ihr Speicherkonto gewähren. Dies ist tatsächlich der Hauptgrund dafür, SAS auf Kontoebene zu erstellen – Sie können SAS auf Kontoebene verwenden, statt die Zugriffsschlüssel in einer Konfigurationsdatei zu speichern.

#### <a name="key-regeneration-plan"></a>Plan zur erneuten Schlüsselgenerierung

Sie sollten den Schlüssel, den Sie verwenden, nicht ohne Planung erneut generieren. Andernfalls besteht die Gefahr, dass Sie jeglichen Zugriff auf dieses Speicherkonto abschneiden und so eine größere Unterbrechung verursachen. Darum gibt es zwei Schlüssel. Sie sollten jeweils nur einen Schlüssel erneut generieren.

Bevor Sie Ihre Schlüssel erneut generieren, halten Sie eine Liste aller Anwendungen bereit, die auf das Speicherkonto angewiesen sind, sowie aller anderen Dienste, die Sie in Azure verwenden. Falls Sie z.B. Azure Media Services verwenden, die auf Ihr Speicherkonto angewiesen sind, müssen Sie die Zugriffsschlüssel nach dem erneuten Generieren der Schlüssel mit Ihrem Mediendienst erneut synchronisieren. Wenn Sie Anwendungen wie z. B. einen Storage-Explorer verwenden, müssen Sie die neuen Schlüssel diesen Anwendungen ebenfalls bereitstellen. Wenn Sie über VMs verfügen, deren VHD-Dateien im Speicherkonto gespeichert werden, bleiben sie vom erneuten Generieren der Speicherkontoschlüssel unbeeinflusst.

Sie können Ihre Schlüssel im Azure-Portal erneut generieren. Nachdem Schlüssel neu generiert wurden, kann es bis zu 10 Minuten dauern, bis sie in den Speicherdiensten synchronisiert sind.

Wenn Sie so weit sind, sollten Sie Ihre Schlüssel mit dem im Folgenden beschriebenen allgemeinen Verfahren ändern. In diesem Fall wird davon ausgegangen, dass Sie derzeit „Key 1“ verwenden, und Sie stattdessen alles zur Verwendung von „Key 2“ ändern möchten.

1. Generieren Sie „Key 2“ erneut, um sicherzustellen, dass es sicher ist. Dies kann im Azure-Portal erfolgen.
2. Ändern Sie in allen Anwendungen, in denen der Speicherschlüssel gespeichert ist, den Speicherschlüssel zur Verwendung des neuen Werts von „Key 2“. Testen und veröffentlichen Sie die Anwendung.
3. Wenn alle Anwendungen und Dienste erfolgreich ausgeführt werden, generieren Sie „Key 1“ erneut. Dadurch wird sichergestellt, dass jeder, dem Sie nicht ausdrücklich den neuen Schlüssel gegeben haben, nicht mehr auf das Speicherkonto zugreifen kann.

Wenn Sie derzeit „Key 2“ verwenden, verwenden Sie dieselbe Vorgehensweise, aber mit umgekehrten Schlüsselnamen.

Sie können ein paar Tage lang migrieren und jede Anwendung so ändern, dass sie den neuen Schlüssels verwendet und veröffentlicht. Anschließend sollten Sie den alten Schlüssel erneut generieren, damit er nicht mehr funktioniert.

Eine weitere Option ist, dass Sie den Speicherkontoschlüssel als geheimen Schlüssel in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) platzieren und Ihre Anwendungen den Schlüssel von dort abrufen lassen. Wenn Sie den Schlüssel dann erneut generieren und Azure Key Vault aktualisieren, müssen die Anwendungen nicht erneut bereitgestellt werden, da sie den neuen Schlüssel automatisch aus Azure Key Vault abrufen. Sie können festlegen, dass die Anwendung den Schlüssel jedes Mal liest, wenn Sie ihn benötigen, oder Sie können ihn im Arbeitsspeicher zwischenspeichern, und wenn bei seiner Verwendung ein Fehler auftritt, den Schlüssel erneut aus Azure Key Vault abrufen.

Die Verwendung von Azure Key Vault stellt auch eine weitere Sicherheitsstufe für Ihre Speicherschlüssel dar. Wenn Sie diese Methode verwenden, benötigen Sie keinen hartcodierten Speicherschlüssel in einer Konfigurationsdatei, sodass niemand mehr ohne ausdrückliche Berechtigung Zugriff auf die Schlüssel erhalten kann.

Ein weiterer Vorteil der Verwendung von Azure Key Vault ist, dass Sie auch den Zugriff auf die Schlüssel mithilfe von Azure Active Directory steuern können. Dies bedeutet, dass Sie Zugriff auf die Reihe von Anwendungen gewähren können, die die Schlüssel aus Azure Key Vault abrufen müssen, und wissen, dass andere Anwendungen nicht auf die Schlüssel zugreifen können, ohne dass Sie ihnen die ausdrückliche Berechtigung gewähren.

> [!NOTE]
> Es wird empfohlen, in allen Ihren Anwendungen jeweils nur einen Schlüssel gleichzeitig zu verwenden. Wenn Sie „Key 1“ an einigen Stellen und „Key 2“ an anderen verwenden, können Sie die Verwendung der Schlüssel nicht wechseln, ohne dass einige Anwendungen den Zugriff verlieren.

#### <a name="resources"></a>Ressourcen

* [Verwalten von Speicherkontoeinstellungen im Azure-Portal](storage-account-manage.md)
* [Azure Storage Resource Provider REST-API-Referenz](https://msdn.microsoft.com/library/mt163683.aspx)

## <a name="data-plane-security"></a>Sicherheit auf Datenebene
Sicherheit auf Datenebene bezieht sich auf die Methoden zum Schützen der in Azure Storage gespeicherten Datenobjekte. Wir haben Methoden zum Verschlüsseln der Daten und die Sicherheit bei der Übertragung der Daten kennengelernt, aber wie steuern Sie den Zugriff auf die Objekte?

Sie haben drei Optionen zur Autorisierung des Zugriffs auf Datenobjekte in Azure Storage:

- Verwenden Sie Azure AD, um den Zugriff auf Dateisysteme und Warteschlangen zu autorisieren. Azure AD bietet gegenüber anderen Ansätzen zur Autorisierung Vorteile, z.B. Wegfall der Speicherung von Geheimnissen in Ihrem Code. Weitere Informationen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory](storage-auth-aad.md). 
- Verwenden Sie Ihre Speicherkontoschlüssel, um den Zugriff per gemeinsam verwendetem Schlüssel zu autorisieren. Für die Autorisierung per gemeinsam verwendetem Schlüssel ist das Speichern Ihrer Speicherkontoschlüssel in Ihrer Anwendung erforderlich, und Microsoft empfiehlt stattdessen nach Möglichkeit die Nutzung von Azure AD. Nutzen Sie für Produktionsanwendungen oder zum Autorisieren des Zugriffs auf Azure-Tabellen und -Dateien weiterhin den gemeinsam verwendeten Schlüssel, während sich die Azure AD-Integration in der Vorschauphase befindet.
- Verwenden Sie Shared Access Signatures, um bestimmten Datenobjekten für einen bestimmten Zeitraum kontrollierte Berechtigungen zu gewähren.

Zusätzlich zum Beschränken des Zugriffs durch Autorisierung können Sie auch [Firewalls und virtuelle Netzwerke](storage-network-security.md) verwenden, um den Zugriff auf das Speicherkonto basierend auf Netzwerkregeln einzuschränken.  Mit diesem Ansatz können Sie den Zugriff auf öffentlichen Internetdatenverkehr verweigern und nur den Zugriff auf bestimmte virtuelle Azure-Netzwerke oder IP-Adressbereiche im öffentlichen Internet gewähren.

### <a name="storage-account-keys"></a>Speicherkontoschlüssel

Speicherkontoschlüssel sind von Azure erstellte 512-Bit-Zeichenfolgen, die zusammen mit dem Speicherkontonamen für den Zugriff auf die im Speicherkonto gespeicherten Datenobjekte verwendet werden können.

Beispielsweise können Sie Dateien lesen. Viele dieser Aktionen können über das Azure-Portal oder mithilfe einer der vielen Storage-Explorer-Anwendungen ausgeführt werden. Sie können auch Code zum Verwenden der REST-API für diese Vorgänge schreiben.

Wie im Abschnitt zur [Sicherheit auf Verwaltungsebene](#management-plane-security) erläutert wurde, kann der Zugriff auf die Speicherschlüssel für ein Speicherkonto mit dem Azure Resource Manager-Modell über rollenbasierte Zugriffssteuerung (RBAC) gesteuert werden.

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Delegieren des Zugriffs auf Objekte in Ihrem Konto mithilfe von SAS und gespeicherter Zugriffsrichtlinien

Eine Shared Access Signature ist eine Zeichenfolge mit einem Sicherheitstoken, die an einen URI angefügt werden kann. Mit diesem URI können Sie den Zugriff auf Speicherobjekte delegieren und Einschränkungen festlegen, wie z.B. Berechtigungen und den Datums- und Uhrzeitbereich für den Zugriff.

Sie können den Zugriff auf Dateien oder Verzeichnisse gewähren.

Sie könnten einem Kunden ein SAS-Token übergeben, um Bilder in ein Dateisystem im Blobspeicher hochzuladen, und einer Webanwendung die Berechtigung gewähren, diese Bilder zu lesen. In beiden Fällen liegt eine Trennung der Belange vor: Jede Anwendung kann genau den Zugriff erhalten, den sie benötigt, um ihre Aufgabe auszuführen. Dies macht die Verwendung von Shared Access Signatures möglich.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Gründe für die Verwendung von Shared Access Signatures?

Warum sollten Sie eine SAS verwenden, anstatt einfach Ihren Speicherkontoschlüssel zu übergeben, was doch viel einfacher ist? Wenn Sie den Schlüssel Ihres Speicherkontos übergeben, ist dies so, als würden Sie die Schlüssel zu Ihrem „Speicherreich“ freigeben. Sie gewähren damit vollständigen Zugriff. Jemand könnte Ihre Schlüssel verwenden und seine gesamte Musikbibliothek in Ihr Speicherkonto hochladen. Er könnte auch Ihre Dateien durch virusinfizierte Versionen ersetzen oder Ihre Daten stehlen. Die Vergabe von uneingeschränktem Zugriff auf Ihr Speicherkonto dürfen Sie nicht auf die leichte Schulter nehmen.

Mit Shared Access Signatures können Sie einem Client genau die erforderlichen Berechtigungen für einen begrenzten Zeitraum gewähren. Wenn z.B. jemand eine Datei in Ihr Konto hochlädt, können Sie ihm für die zum Hochladen der Datei benötigte Zeit Schreibzugriff gewähren (natürlich der Größe der Datei entsprechend). Und wenn Sie Ihre Meinung ändern, können Sie die Zugriffsberechtigung widerrufen.

Darüber hinaus können Sie angeben, dass Anforderungen, die mit einer SAS erfolgen, auf eine bestimmte IP-Adresse oder einen bestimmten IP-Adressbereich außerhalb von Azure beschränkt sind. Sie können auch fordern, dass die Anforderungen mit einem bestimmten Protokoll erfolgen (HTTPS oder HTTP/HTTPS). Dies bedeutet: Wenn Sie nur HTTPS-Datenverkehr zulassen möchten, können Sie HTTPS als erforderliches Protokoll festlegen, sodass HTTP-Datenverkehr blockiert wird.

#### <a name="definition-of-a-shared-access-signature"></a>Definition einer Shared Access Signature

Eine Shared Access Signature ist ein Satz von Abfrageparametern, die an die URL angehängt werden, die auf die Ressource verweist,

die Informationen über den erlaubten Zugriff und die Zeitspanne bieten, für die der Zugriff zulässig ist. Der URI in diesem Beispiel bietet für fünf Minuten Lesezugriff auf ein Blob. Die SAS-Abfrageparameter müssen URL-codiert sein, d.h. %3A steht für den Doppelpunkt (:) und %20 für ein Leerzeichen.

```
http://mystorage.dfs.core.windows.net/myfilesystem/myfile.txt (URL to the file)
?sv=2015-04-05 (storage service version)
&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
&sr=b (resource is a blob)
&sp=r (read access)
&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
&spr=https (only allow HTTPS requests)
&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)
```

#### <a name="how-the-shared-access-signature-is-authorized-by-the-azure-storage-service"></a>Autorisierung der Shared Access Signature durch den Azure Storage-Dienst

Wenn der Speicherdienst die Anforderung empfängt, erstellt er aufgrund der Eingabeabfrageparameter mit der gleichen Methode wie das aufrufende Programm eine Signatur. Anschließend werden die zwei Signaturen verglichen. Wenn sie übereinstimmen, kann der Speicherdienst die Speicherdienstversion prüfen, um die Gültigkeit sicherzustellen sowie festzustellen, ob aktuelles Datum und aktuelle Uhrzeit im angegebenen Fenster liegen, ob der angeforderte Zugriff der Anforderung entspricht usw.

Würde z. B. die obige URL statt auf ein Blob auf eine Datei weisen, würde diese Anforderung fehlschlagen, weil sie angibt, dass die Shared Access Signature für ein Blob gilt. Wenn der aufgerufene REST-Befehl versuchen würde, ein Blob zu aktualisieren, würde er fehlschlagen, da die Shared Access Signature angibt, dass nur Lesezugriff zulässig ist.

#### <a name="types-of-shared-access-signatures"></a>Typen von Shared Access Signatures

* Eine SAS auf Dienstebene kann für den Zugriff auf bestimmte Ressourcen in einem Speicherkonto verwendet werden. Einige Beispiele hierfür sind das Abrufen einer Liste von Dateien in einem Dateisystem oder das Herunterladen einer Datei.
* Eine SAS auf Kontoebene kann für den Zugriff auf alles verwendet werden, für das eine SAS auf Dienstebene verwendet werden kann. Darüber hinaus kann sie Optionen für Ressourcen bieten, für die mit SAS auf Dienstebene keine Berechtigung besteht, z.B. die Fähigkeit zum Erstellen von Dateisystemen.

#### <a name="creating-a-sas-uri"></a>Erstellen eines SAS-URIs

1. Sie können einen URI bei Bedarf erstellen und dabei jedes Mal alle Abfrageparameter definieren.

   Mit diesem Ansatz sind Sie flexibel, aber wenn Sie jedes Mal einen logischen Satz gleicher Parameter haben, sollten Sie besser eine SAS-Richtlinie verwenden.
2. Sie können eine gespeicherte Zugriffsrichtlinie für ein ganzes Dateisystem, eine Dateifreigabe, eine Tabelle oder eine Warteschlange erstellen. Dann können Sie diese als Grundlage für die SAS-URIs verwenden, die Sie erstellen. Auf gespeicherten Zugriffsrichtlinien basierende Berechtigungen können leicht widerrufen werden. Pro Dateisystem, Warteschlange, Tabelle oder Dateifreigabe können Sie bis zu fünf Richtlinien definieren.

   Wenn z.B. viele Personen die Blobs in einem bestimmten Dateisystem lesen, könnten Sie eine gespeicherte Zugriffsrichtlinie mit der Aussage „Lesezugriff gewähren“ und beliebigen anderen Einstellungen erstellen, die jedes Mal gleich sind. Anschließend können Sie mithilfe der Einstellungen der gespeicherten Zugriffsrichtlinie einen SAS-URI erstellen und das Datum und die Uhrzeit des Ablaufs angeben. Dies hat den Vorteil, dass Sie nicht jedes Mal alle Abfrageparameter angeben müssen.

#### <a name="revocation"></a>Widerruf

Stellen Sie sich vor, dass Ihre SAS kompromittiert wurde, oder Sie sie aus Gründen der Unternehmenssicherheit oder wegen gesetzlicher Vorschriften und Richtlinien ändern möchten. Wie widerrufen Sie den Zugriff auf eine Ressource, die diese SAS verwendet? Das hängt davon ab, wie Sie den SAS-URI erstellt haben.

Wenn Sie Ad-hoc-URIs verwenden, können Sie zwischen drei Optionen wählen. Sie können SAS-Token mit kurzen Ablaufrichtlinien ausgeben und warten, bis die SAS abläuft. Sie können die Ressource umbenennen oder löschen (sofern das Token für ein einzelnes Objekt definiert wurde). Sie können die Schlüssel des Speicherkontos ändern. Diese letzte Option kann weitreichende Auswirkungen haben, je nachdem, wie viele Dienste dieses Speicherkonto verwenden, und Sie sollten dies nicht ohne ausreichende Planung tun.

Wenn Sie eine SAS verwenden, die von einer gespeicherten Zugriffsrichtlinie abgeleitet ist, können Sie die Zugriffsrechte durch Widerrufen der gespeicherten Zugriffsrichtlinie aufheben – Sie können sie einfach so ändern, dass sie bereits abgelaufen ist, oder sie vollständig entfernen. Dies wird sofort wirksam und erklärt jede mithilfe dieser gespeicherten Zugriffsrichtlinie erstellte SAS für ungültig. Das Aktualisieren oder Entfernen der gespeicherten Zugriffsrichtlinie kann möglicherweise Konsequenzen für die Personen haben, die auf dieses spezifische Dateisystem, die Dateifreigabe, die Tabelle oder die Warteschlange über SAS zugreifen. Wenn die Clients jedoch so geschrieben werden, dass sie eine neue SAS anfordern, wenn die alte ungültig wird, funktioniert dies hervorragend.

Da Sie bei Verwendung einer SAS, die von einer gespeicherten Zugriffsrichtlinie abgeleitet ist, die SAS sofort widerrufen können, sollten Sie nach Möglichkeit immer die gespeicherte Zugriffsrichtlinie verwenden.

#### <a name="resources"></a>Ressourcen

Weitere ausführliche Informationen zur Verwendung von SAS und gespeicherten Zugriffsrichtlinien mit Beispielen finden Sie in den folgenden Artikeln:

* Dies sind die Referenzartikel.

  * [Beispiele für SAS (Shared Access Signatures)](https://msdn.microsoft.com/library/dn140256.aspx)

    Dieser Artikel enthält Beispiele für die Verwendung einer Dienstebenen-SAS mit Blobs, Warteschlangennachrichten, Tabellenbereichen und Dateien.
  * [Constructing a service SAS (Erstellen einer Dienstebenen-SAS)](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Constructing an account SAS (Erstellen einer Kontoebenen-SAS)](https://msdn.microsoft.com/library/mt584140.aspx)
* Hierbei handelt es sich um Tutorials für die Verwendung der .NET-Clientbibliothek zum Erstellen von SAS und gespeicherten Zugriffsrichtlinien.

  * [Verwenden von Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md)
  * [Shared Access Signatures, Teil 2: Erstellen und Verwenden einer SAS mit dem Blobdienst](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Dieser Artikel enthält eine Erläuterung des SAS-Modells, Beispiele für SAS und Empfehlungen bewährter Methoden für die SAS-Verwendung. Auch der Widerruf der Berechtigung wird hier erörtert.

* Authentication

  * [Authentifizierung für Azure Storage-Dienste](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Erste-Schritte-Tutorial für Shared Access Signatures

  * [SAS-Erste-Schritte-Tutorial](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung

### <a name="transport-level-encryption--using-https"></a>Verschlüsselung auf Transportebene – mithilfe von HTTPS

Ein weiterer Schritt, mit dem Sie die Sicherheit Ihrer Azure Storage-Daten sicherstellen sollten, ist das Verschlüsseln der Daten bei der Übertragung zwischen dem Client und Azure Storage. Erstens sollten Sie immer das [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -Protokoll verwenden, denn es gewährleistet die sichere Kommunikation über das öffentliche Internet.

Beim Abrufen von REST-APIs oder Zugreifen auf Objekte im Speicher sollten Sie immer HTTPS verwenden, um einen sicheren Kommunikationskanal zu gewährleisten. Mit **Shared Access Signatures**, die zum Delegieren des Zugriffs auf Azure Storage-Objekte verwendet werden können, können Sie außerdem festlegen, dass bei Verwendung von Shared Access Signatures nur das HTTPS-Protokoll verwendet werden darf. So können Sie sicherstellen, dass jeder, der Links mit SAS-Token sendet, das richtige Protokoll verwendet.

Sie können die Verwendung von HTTPS beim Aufruf von REST-APIs für den Zugriff auf Objekte in Speicherkonten erzwingen, indem Sie die Option [Sichere Übertragung erforderlich](../storage-require-secure-transfer.md) aktivieren. Sobald diese Option aktiviert ist, werden Verbindungen über HTTP abgelehnt.

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

### <a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

SSE ist für alle Speicherkonten aktiviert und kann nicht deaktiviert werden. SSE verschlüsselt Ihre Daten beim Schreiben in Azure Storage automatisch. Wenn Sie Daten aus Azure Storage lesen, werden sie von Azure Storage vor der Rückgabe entschlüsselt. Mit SSE können Sie Daten schützen, ohne dass Sie in Anwendungen Code ändern oder hinzufügen müssen.

Sie können von Microsoft verwaltete Schlüssel oder eigene benutzerdefinierte Schlüssel verwenden. Microsoft generiert verwaltete Schlüssel und verwaltet die sichere Speicherung der Schlüssel sowie die reguläre Rotation gemäß der internen Microsoft-Richtlinie. Weitere Informationen zur Verwendung benutzerdefinierter Schlüssel finden Sie unter [Speicherdienstverschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).
