---
title: Microsoft Azure-Datenverschlüsselung ruhender Daten | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie einen Überblick über die Datenverschlüsselung ruhender Daten von Microsoft Azure, die Hauptfunktionen und allgemeine Überlegungen.
services: security
documentationcenter: na
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2018
ms.author: barclayn
ms.openlocfilehash: 40c69d996721b664fbea5cd539f60ecc6a521d24
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457645"
---
# <a name="azure-data-encryption-at-rest"></a>Azure-Datenverschlüsselung ruhender Daten

Microsoft Azure bietet Tools, um Daten den Sicherheits- und Konformitätsanforderungen Ihres Unternehmens entsprechend zu schützen. In diesem Artikel geht es um folgende Themen:

- Schutz ruhender Daten in der gesamten Microsoft Azure-Umgebung
- Verschiedene Komponenten bei der Implementierung des Datenschutzes
- Vor- und Nachteile der verschiedenen Ansätze für den Schutz der Schlüsselverwaltung 

Die Verschlüsselung ruhender Daten ist eine gängige Sicherheitsanforderung. In Azure können Organisationen die Verschlüsselung ruhender Daten erreichen, und zwar ohne die Kosten für Implementierung und Verwaltung und ohne das Risiko einer kundenspezifischen Schlüsselverwaltungslösung. Organisationen haben die Option, Azure die komplette Verschlüsselung ruhender Daten verwalten zu lassen. Zusätzlich haben Organisationen verschiedene Optionen, um die Verschlüsselung oder die Verschlüsselungsschlüssel genau zu verwalten.

## <a name="what-is-encryption-at-rest"></a>Was ist die Verschlüsselung ruhender Daten?

Die Verschlüsselung ruhender Daten ist die Codierung (Verschlüsselung) von Daten, sobald diese gespeichert werden. Die Entwürfe zur Verschlüsselung ruhender Daten in Azure verwenden die symmetrische Verschlüsselung zum schnellen Ver- und Entschlüsseln von großen Mengen an Daten anhand eines einfachen konzeptionellen Modells:

- Ein symmetrischer Verschlüsselungsschlüssel wird zur Verschlüsselung von Daten verwendet, wenn diese in Speicher geschrieben werden. 
- Derselbe Verschlüsselungsschlüssel wird verwendet, um diese Daten zu entschlüsseln, wenn diese auf den Gebrauch im Arbeitsspeicher vorbereitet werden.
- Daten können partitioniert werden, und für jede Partition können unterschiedliche Schlüssel verwendet werden.
- Schlüssel müssen an einem sicheren Speicherort mit Richtlinien für die Zugriffssteuerung gespeichert werden. Diese Richtlinien müssen den Zugriff auf bestimmte Identitäten einschränken und den Schlüsselgebrauch protokollieren. Datenverschlüsselungsschlüssel werden häufig mit asymmetrischer Verschlüsselung verschlüsselt, um den Zugriff weiter einzuschränken.

In der Praxis erfordern Szenarios der Schlüsselverwaltung und -steuerung sowie die Skalierungs- und Verfügbarkeitssicherstellung zusätzliche Konstrukte. Konzepte und Komponenten der Verschlüsselung ruhender Daten mit Microsoft Azure werden unten beschrieben.

## <a name="the-purpose-of-encryption-at-rest"></a>Welchen Zweck erfüllt die Verschlüsselung ruhender Daten?

Die Verschlüsselung ruhender Daten bietet Schutz für gespeicherte Daten (im Ruhezustand). Zu Angriffen auf ruhende Daten zählen Versuche, physischen Zugriff auf die Hardware zu erhalten, auf der die Daten gespeichert sind, und die enthaltenen Daten zu kompromittieren. In einem derartigen Angriff wurde die Festplatte eines Servers während der Wartung möglicherweise nicht angemessen behandelt, sodass ein Angreifer die Festplatte entfernen konnte. Dann kann der Angreifer die Festplatte später in einen Computer einbauen, den er steuert, und erhält Zugriff auf die Daten. 

Die Verschlüsselung ruhender Daten wurde entwickelt, um dem Angreifer den Zugriff auf die unverschlüsselten Daten zu verwehren, indem die Daten auf der Festplatte verschlüsselt sind. Wenn ein Angreifer nun eine Festplatte mit derartigen verschlüsselten Daten erhält, aber keinen Zugriff auf die Verschlüsselungsschlüssel hat, kann der Angreifer die Daten nicht ohne Weiteres kompromittieren. In einem derartigen Szenario müsste ein Angreifer versuchen, die verschlüsselten Daten anzugreifen, was jedoch deutlich komplexer und ressourcenaufwändiger als bei unverschlüsselten Daten auf einer Festplatte ist. Aus diesem Grund wird die Verschlüsselung ruhender Daten dringend empfohlen und stellt für viele Organisationen eine Anforderung höchster Priorität dar. 

Die Verschlüsselung ruhender Daten ist ggf. auch zur Erfüllung der Anforderungen einer Organisation an Datengovernance und Konformität erforderlich. Branchenspezifische und gesetzliche Vorschriften wie HIPAA, PCI und FedRAMP geben spezifische Sicherheitsmechanismen für Datenschutz- und Verschlüsselungsanforderungen vor. Die Verschlüsselung ruhender Daten ist eine verpflichtende Maßnahme, die für die Erfüllung dieser Vorschriften erforderlich ist.

Zusätzlich zu Konformitäts- und rechtlichen Anforderungen sollte die Verschlüsselung ruhender Daten als eine Plattformfunktion für tiefgreifenden Schutz angesehen werden. Während Microsoft eine konforme Plattform für Dienste, Anwendungen, Daten, ein umfangreiches Hilfsmittel und physische Sicherheit, Datenzugriffssteuerung und -überwachung bereitstellt, ist es wichtig, zusätzliche „überlappende“ Sicherheitsmaßnahmen bereitzustellen, falls eine andere Sicherheitsmaßnahme fehlschlägt. Die Verschlüsselung ruhender Daten bietet einen derartigen zusätzlichen Sicherheitsmechanismus.

Microsoft möchte die Verschlüsselung ruhender Daten clouddienstübergreifend bereitstellen. Zudem soll Kunden eine angemessene Verwaltbarkeit der Verschlüsselungsschlüssel und der Zugriff auf Protokolle ermöglicht werden, die zeigen, wann Verschlüsselungsschlüssel verwendet werden. Darüber hinaus arbeitet Microsoft daran, alle ruhenden Kundendaten standardmäßig zu verschlüsseln.

## <a name="azure-encryption-at-rest-components"></a>Komponenten der Verschlüsselung ruhender Daten von Azure

Wie bereits beschrieben, ist das Ziel der Verschlüsselung ruhender Daten die Verschlüsselung von Daten, die auf einer Festplatte gespeichert werden, mithilfe eines geheimen Verschlüsselungsschlüssels. Um das Ziel der Erstellung sicherer Schlüssel zu erreichen, müssen Sie die Speicherung, Zugriffssteuerung und Verwaltung der Verschlüsselungsschlüssel gewährleisten. Auch wenn sich die Details unterscheiden können, können Implementierungen der Verschlüsselung ruhender Daten von Azure-Diensten anhand der Konzepte beschrieben werden, die im folgenden Diagramm veranschaulicht werden.

![Komponenten](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

Der Speicherort der Verschlüsselungsschlüssel und der Zugriffssteuerung dieser Schlüssel ist für das Modell der Verschlüsselung ruhender Daten wesentlich. Die Schlüssel müssen stark gesichert aber dennoch von angegebenen Benutzern verwaltbar sein. Darüber hinaus müssen spezifische Dienste darauf zugreifen können. Azure Key Vault ist die für Azure-Dienste empfohlene Schlüsselspeicherlösung. Es bietet eine dienstübergeifende Verwaltungserfahrung. Schlüssel werden in Schlüsseltresoren gespeichert und verwaltet. Der Zugriff auf diesen Schlüsseltresor kann Benutzern und Diensten gewährt werden. Azure Key Vault unterstützt das Erstellen von Schlüsseln durch Kunden und das Importieren von Kundenschlüsseln für den Gebrauch in von Kunden verwalteten Verschlüsselungsschlüsselszenarios.

### <a name="azure-active-directory"></a>Azure Active Directory

Berechtigungen zum Speichern der Schlüssel in Azure Key Vault, um sie zu verwalten oder um auf sie zur Ver- und Entschlüsselung ruhender Daten zuzugreifen, können Azure AD-Konten gewährt werden. 

### <a name="key-hierarchy"></a>Schlüsselhierarchie

Bei der Implementierung einer Verschlüsselung ruhender Daten wird mehr als ein Schlüssel verwendet. Die asymmetrische Verschlüsselung ist beim Etablieren des Vertrauens und der Authentifizierung, die für den Zugriff und die Verwaltung des Schlüssels erforderlich sind, hilfreich. Die symmetrische Verschlüsselung ist für Massenverschlüsselungen und -entschlüsselungen effizienter, sodass eine stärkere Verschlüsselung und bessere Leistung gewährleistet wird. Das Einschränken des Gebrauchs eines einzelnen Verschlüsselungsschlüssels mindert das Risiko, das die Sicherheit des Schlüssels gefährdet wird. Zudem werden die Kosten für die erneute Verschlüsselung gesenkt, wenn ein Schlüssel ersetzt werden muss. Azure-Modelle für die Verschlüsselung ruhender Daten arbeiten mit einer Schlüsselhierarchie, die sich aus den folgenden Typen von Schlüsseln zusammensetzt:

- **Datenverschlüsselungsschlüssel (DEK)**: Ein symmetrischer AES256-Schlüssel zum Verschlüsseln einer Partition oder eines Datenblocks.  Eine einzelne Ressource kann aus viele Partitionen bestehen und deshalb auch viele DEKs haben. Das Verschlüsseln jedes Datenblocks mit einem anderen Schlüssel erschwert Kryptoanalyseangriffe. Der Ressourcenanbieter oder die Anwendungsinstanz, die einen spezifischen Block ver- oder entschlüsselt, muss Zugriff auf die DEKs gewähren. Wenn ein DEK durch einen neuen Schlüssel ersetzt wird, müssen nur die Daten im verknüpften Block erneut mit dem neuen Schlüssel verschlüsselt werden.
- **Schlüsselverschlüsselungsschlüssel (KEK)**: Ein asymmetrischer Verschlüsselungsschlüssel zur Verschlüsselung eines Datenverschlüsselungsschlüssels. Mit einem KEK können die DEKs selbst verschlüsselt und gesteuert werden. Die Entität, die auf den KEK zugreifen kann, kann sich von der Entität unterscheiden, die den DEK erfordert. Eine Entität kann den Zugriff auf die DEK aushandeln, um den Zugriff jedes DEK auf eine bestimmte Partition zu begrenzen. Da der KEK erforderlich ist, um DEKs zu entschlüsseln, ist der KEK ein Punkt, an dem DEKs gelöscht werden können, indem der KEK gelöscht wird.

Die DEKs, die mit KEKs verschlüsselt wurden, werden separat gespeichert. Nur eine Entität mit Zugriff auf den KEK kann auf mit diesem Schlüssel verschlüsselte DEKs zugreifen. Es werden verschiedene Schlüsselspeichermodelle unterstützt. Diese Modelle werden weiter unten in diesem Abschnitt ausführlicher besprochen.

## <a name="data-encryption-models"></a>Datenverschlüsselungsmodelle

Damit Sie verstehen, wie die verschiedenen Ressourcenanbieter in Azure die ruhende Verschlüsselung implementieren, ist es wichtig, dass Sie die unterschiedlichen Verschlüsselungsmodelle und deren Vor- und Nachteile kennen. Diese Definitionen sind in allen Ressourcenanbietern in Azure gleich, um die gleiche Sprache und Taxonomie zu gewährleisten. 

Es gibt drei Szenarios für die serverseitige Verschlüsselung:

- Serverseitige Verschlüsselung mit vom Dienst verwalteten Schlüsseln
    - Azure-Ressourcenanbieter führen die Verschlüsselungs- und Entschlüsselungsvorgänge durch
    - Microsoft verwaltet die Schlüssel
    - Vollständige Cloud-Funktionen

- Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault
    - Azure-Ressourcenanbieter führen die Verschlüsselungs- und Entschlüsselungsvorgänge durch
    - Der Kunde steuert Schlüssel per Azure Key Vault
    - Vollständige Cloud-Funktionen

- Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln auf kundengesteuerter Hardware
    - Azure-Ressourcenanbieter führen die Verschlüsselungs- und Entschlüsselungsvorgänge durch
    - Der Kunde steuert die Schlüssel auf kundengesteuerter Hardware
    - Vollständige Cloud-Funktionen

Ziehen Sie für die clientseitige Verschlüsselung folgendes in Betracht:

- Azure-Dienste können entschlüsselte Daten nicht erkennen
- Die Kunden bewahren die Schlüssel lokal auf (oder in anderen sicheren Speichern). Schlüssel sind für Azure-Dienste nicht verfügbar
- Reduzierte Cloud-Funktionen

Die in Azure unterstützten Verschlüsselungsmodelle können in zwei Hauptgruppen aufgeteilt werden: „Kundenverschlüsselung“ und „serverseitige Verschlüsselung“, wie bereits erwähnt. Beachten Sie, dass Azure-Dienste immer den Einsatz eines sicheren Datentransports wie TLS oder HTTPS empfehlen, unabhängig vom Verschlüsselungsmodell für ruhende Daten. Deshalb sollte das Datentransportprotokoll die Verschlüsselung während des Datentransport behandeln und sollte kein ausschlaggebender Faktor bei der Entscheidung für oder gegen ein Verschlüsselungsmodell für ruhende Daten sein.

### <a name="client-encryption-model"></a>Clientverschlüsselungsmodell

Als Clientverschlüsselungsmodell wird eine Verschlüsselung bezeichnet, die außerhalb des Ressourcenanbieters oder außerhalb von Azure von der Dienst- oder der aufrufenden Anwendung durchgeführt wird. Die Verschlüsselung kann von der Dienstanwendung in Azure durchgeführt werden oder von einer Anwendung, die im Rechenzentrum des Kunden ausgeführt wird. In beiden Fällen erhält der Azure-Ressourcenanbieter einen verschlüsselten Datenblob, ohne die Daten entschlüsseln oder auf die Verschlüsselungsschlüssel zugreifen zu können, wenn Sie dieses Verschlüsselungsmodell einsetzen. In diesem Modell erfolgt die Schlüsselverwaltung über den aufrufenden Dienst bzw. die aufrufende Anwendung und ist für den Azure-Dienst nicht transparent.

![Client](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Serverseitiges Verschlüsselungsmodell

Als Modelle für die serverseitige Verschlüsselung werden Verschlüsselungen bezeichnet, die vom Azure-Dienst durchgeführt werden. In diesem Modell führt der Ressourcenanbieter den Ver- und Entschlüsselungsvorgang durch. Azure Storage erhält z.B. Daten in Nur-Text-Vorgängen und führt die Ver- und Entschlüsselung intern durch. Der Ressourcenanbieter verwendet möglicherweise Verschlüsselungsschlüssel, die je nach Konfiguration von Microsoft oder dem Kunden verwaltet werden. 

![Server](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Verwaltungsmodelle für die serverseitige Verschlüsselungsschlüssel

Jedes Modell zur serverseitigen Verschlüsselung impliziert eindeutige Merkmale für die Schlüsselverwaltung. Dazu zählt u.a., wo und wie Verschlüsselungsschlüssel erstellt und gespeichert werden, sowie die Zugriffsmodelle und die Schlüsselrotationsprozeduren. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Serverseitige Verschlüsselung mit vom Dienst verwalteten Schlüsseln

Für viele Kunden ist die wichtigste Anforderung, sicherzustellen, dass die Daten immer dann verschlüsselt sind, wenn sie ruhen. Dieses Modell ist durch die serverseitige Verschlüsselung mit vom Dienst verwalteten Schlüsseln möglich, da diese es Kunden ermöglicht, die spezifische Ressource (Storage-Konto. SQL-Datenbank usw.) für die Verschlüsselung zu kennzeichnen und alle Schlüsselverwaltungsaspekte wie die Schlüsselausstellung, -rotation und -sicherung Microsoft zu überlassen. Die meisten Azure-Dienste, die die Verschlüsselung ruhender Daten unterstützen, unterstützen üblicherweise auch dieses Modell der Übergeben der Verwaltung der Verschlüsselungsschlüssel an Azure. Der Azure-Ressourcenanbieter erstellt die Schlüssel, speichert sie an einem sicheren Speicherort und ruft sie bei Bedarf ab. Dies bedeutet, dass der Dienst Vollzugriff auf die Schlüssel hat, und dass er die Verwaltung der Lebensdauer der Anmeldeinformationen vollständig steuert.

![verwaltet](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Die serverseitige Verschlüsselung mit vom Dienst verwalteten Schlüsseln reagiert deshalb schnell auf das Bedürfnis, eine Verschlüsselung ruhender Daten mit geringem Mehraufwand für den Kunden zu haben. Wenn sie zur Verfügung steht, kann der Kunde das Azure-Portal für das Zielabonnement und den Zielressourcenanbieter öffnen und ein Kontrollkästchen aktivieren, das angibt, dass die Daten verschlüsselt werden sollen. Bei einigen Ressourcen-Managern ist die serverseitige Verschlüsselungen mit vom Dienst verwalteten Schlüssel standardmäßig aktiviert.

Die serverseitige Verschlüsselung mit von Microsoft verwalteten Schlüsseln impliziert nicht, dass der Dienst Vollzugriff zum Speichern und Verwalten der Schlüssel hat. Während einige Kunden möglicherweise die Schlüssel verwalten möchten, weil sie das Gefühl haben, dass sie so eine höhere Sicherheit erreichen können, sollten die Kosten und das Risiko, die mit einer kundenspezifischen Schlüsselspeicherlösung verbunden sind, bei der Bewertung eines Modells berücksichtigt werden. In vielen Fällen stellen Organisationen ggf. fest, dass Ressourcenengpässe oder Risiken einer lokalen Lösung größer sein können als das Risiko einer Cloudverwaltung der Schlüssel für die Verschlüsselung ruhender Daten.  Dieses Modell kann allerdings für Organisation unzureichend sein, die erfordern, dass sie die Erstellung oder die Lebensdauer der Verschlüsselungsschlüssel steuern können, oder dass anderes Personal die Verschlüsselungsschlüssel eines Diensts verwaltet als das Personal, das den Dienst verwaltet (d.h. die Trennung der Schlüsselverwaltung vom allgemeinen Verwaltungsmodell des Diensts).

##### <a name="key-access"></a>Schlüsselzugriff

Wenn die serverseitige Verschlüsselung mit vom Dienst verwalteten Schlüsseln verwendet wird, werden die Erstellung, das Speichern und der Dienstzugriff des Schlüssels alle vom Dienst verwaltet. Üblicherweise speichert der grundlegende Azure-Ressourcenanbieter die DEKs in einem Speicher, der sich in der Nähe der Daten befindet und schnell verfügbar und zugreifbar ist, während die KEKs in einem sicheren internen Speicher gespeichert werden.

**Vorteile**

- Einfache Einrichtung
- Microsoft verwaltet die Rotation, Sicherung und Redundanz der Schlüssel.
- Der Kunde muss die Kosten nicht tragen, die mit einer Implementierung oder dem Risiko eines kundenspezifischen Schlüsselverwaltungsschema in Verbindung stehen.

**Nachteile**

- Keine Kundensteuerung der Verschlüsselungsschlüssel (Schlüsselspezifizierung, Lebensdauern, Widerruf usw.)
- Die Schlüsselverwaltung kann nicht vom allgemeinen Verwaltungsmodell für den Dienst getrennt werden

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault 

In Szenarios, in denen es erforderlich ist, die ruhenden Daten zu verschlüsseln und die Verschlüsselungsschlüssel zu steuern, können Kunden die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln in Key Vault verwenden. Einige Diensten speichern möglicherweise nur den Stamm-KEK in Azure Key Vault und speichern den verschlüsselten DEK in einem internen Speicherort, der sich näher an den Daten befindet. In diesem Szenario können Kunden Ihre eigenen Schlüssel in Key Vault mitbringen (BYOK: Bring Your Own Key) oder neue Schlüssel generieren und diese verwenden, um die gewünschten Ressourcen zu verschlüsseln. Während der Ressourcenanbieter die Ver- und Entschlüsselungsvorgänge durchführt, verwendet er den konfigurierten Schlüssel als Stammschlüssel für alle Verschlüsselungsvorgänge. 

##### <a name="key-access"></a>Schlüsselzugriff

Beim Modell der serverseitigen Verschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault greift der Dienst auf die Schlüssel zu, um nach Bedarf Ver- und Entschlüsselungen durchzuführen. Schlüssel für die Verschlüsselung ruhender Daten werden einem Dienst über eine Richtlinie zur Zugriffssteuerung verfügbar gemacht. Diese Richtlinie gewährt dem Dienst Identitätszugriff, um Schlüssel zu erhalten. Ein Azure-Dienst, der für ein verknüpftes Abonnement ausgeführt wird, kann mit einer Identität in diesem Abonnement konfiguriert werden. Der Dienst kann die Authentifizierung mit Azure AD durchführen und ein Authentifizierungstoken erhalten, dass ihn als diesen Dienst identifiziert, der im Namen des Abonnements handelt. Dieses Token kann dann Key Vault gezeigt werden, um einen Schlüssel zu erhalten, auf das es Zugriff hat.

Für Vorgänge mit Verschlüsselungsschlüsseln kann einer Dienstidentität Zugriff auf jeden der folgenden Vorgänge gewährt werden: decrypt, encrypt, unwrapKey, wrapKey, verify, sign, get, list, update, create, import, delete, backup, and restore.

Um einen Schlüssel zum Ver- oder Entschlüsseln von ruhenden Daten abzurufen, muss die Dienstidentität, als die die Dienstinstanz des Ressourcen-Managers ausgeführt wird, „UnwrapKey“ (zum Abrufen den Schlüssels für die Entschlüsselung) und „WrapKey“ (um einen Schlüssel beim Erstellen eines neuen Schlüssels in den Schlüsseltresor einzufügen) aufweisen.


>[!NOTE] 
>Weitere Informationen zur Authentifizierung bei Key Vault finden Sie auf der Seite „Sichern Ihres Schlüsseltresors“ in der [Dokumentation zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Vorteile**

- Vollständige Steuerung der verwendeten Schlüssel: Verschlüsselungsschlüssel werden im Key Vault des Kunden und mit dessen Steuerung verwaltet.
- Die Fähigkeit, mehrere Dienste für einen Master zu verschlüsseln
- Die Schlüsselverwaltung kann vom allgemeinen Verwaltungsmodell für den Dienst getrennt werden.
- Der Dienst- und Schlüsselspeicherort kann regionsübergreifend definiert werden.

**Nachteile**

- Der Kunde trägt die volle Verantwortung für die Schlüsselzugriffsverwaltung
- Der Kunde trägt die volle Verantwortung für die Lebensdauerverwaltung des Schlüssels
- Zusätzlicher Aufwand für die Einrichtung und die Konfiguration

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Serverseitige Verschlüsselung mit vom Dienst verwalteten Schlüsseln auf kundengesteuerter Hardware

Einige Azure-Dienste ermöglichen das Schlüsselverwaltungsmodell HYOK (Host Your Own Key, Eigenen Schlüssel hosten). Dieser Verwaltungsmodus ist in Szenarien nützlich, in denen es notwendig ist, die ruhenden Daten zu verschlüsseln und die Schlüssel in einem geschützten Repository außerhalb der Kontrolle von Microsoft zu verwalten. In diesem Modell muss der Dienst den Schlüssel von einer externen Website abrufen. Leistungs- und Verfügbarkeitsgarantien werden beeinträchtigt, und die Konfiguration ist komplexer. Zusätzlich sind die allgemeinen Sicherheitsgewährungen dieses Modell ähnlich wie wenn die Schlüssel vom Kunden in Azure Key Vault verwaltet werden, da der Dienst während Verschlüsselungs- und Entschlüsselungsvorgängen Zugriff auf den DEK hat.  Daraus folgt, dass das Modell für die meisten Organisationen ungeeignet ist, es sei denn, sie haben besondere Schlüsselverwaltungsanforderungen. Aufgrund dieser Einschränkungen unterstützen die meisten Azure-Dienste die serverseitige Verschlüsselung mit vom Server verwalteten Schlüsseln auf von Kunden verwalteter Hardware nicht.

##### <a name="key-access"></a>Schlüsselzugriff

Wenn die serverseitige Verschlüsselung mit vom Server verwalteten Schlüsseln auf von Kunden verwalteter Hardware verwendet wird, werden die Schlüssel in einem System verwaltet, das vom Kunden konfiguriert wird. Azure-Dienste, die dieses Modell unterstützen, bieten eine Möglichkeit, eine sichere Verbindung mit einem von Kunden bereitgestellten Schlüsselspeicher herzustellen.

**Vorteile**

- Volle Steuerung des verwendeten Stammschlüssels: Verschlüsselungsschlüssel werden von einem vom Kunden bereitgestellten Speicher verwaltet.
- Die Fähigkeit, mehrere Dienste für einen Master zu verschlüsseln
- Die Schlüsselverwaltung kann vom allgemeinen Verwaltungsmodell für den Dienst getrennt werden.
- Der Dienst- und Schlüsselspeicherort kann regionsübergreifend definiert werden.

**Nachteile**

- Volle Verantwortung für den Speicher, die Sicherheit, Leistung und Verfügbarkeit der Schlüssel
- Volle Verantwortung für die Schlüsselzugriffsverwaltung
- Volle Verantwortung für die Lebensdauerverwaltung des Schlüssels
- Erhebliche Kosten für die Einrichtung, Konfiguration und fortlaufende Wartung
- Verstärkte Abhängigkeit von der Netzwerkverfügbarkeit zwischen den Rechenzentrum des Kunden und den Azure-Rechenzentren

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Verschlüsselung ruhender Daten in Microsoft-Clouddiensten

Microsoft-Clouddienste werden in drei Cloudmodellen verwendet: IaaS, PaaS und SaaS. Unten finden Sie Beispiele, wie diese in das jeweilige Modell passen:

- Softwaredienste, sogenannte Software-as-a-Servive (SaaS), denen Anwendungen über die Cloud bereitgestellt werden, z.B. Office 365.
- Plattformdienste, mit denen Kunden die Cloud in ihren Anwendungen nutzen, z.B. für Speicherung, Analysen und Service Bus-Funktionalität.
- IaaS-Dienste (Infrastructure-as-a-Service), in denen Kunden Betriebssysteme und Anwendungen bereitstellen, die in der Cloud gehostet werden. Zudem haben sie die Möglichkeit, auch andere Clouddienste zu nutzen.

### <a name="encryption-at-rest-for-saas-customers"></a>Verschlüsselung ruhender Daten für SaaS-Kunden

Software-as-a-Service-Kunden haben die Verschlüsselung ruhender Daten üblicherweise aktiviert oder sie steht zumindest in jedem Dienst zur Verfügung. Office 365 bietet mehrere Optionen, mit denen Kunden die Verschlüsselung ruhender Daten verifizieren oder aktivieren können. Informationen zu Office 365-Diensten finden Sie unter „Data Encryption Technologies for Office 365 (Datenverschlüsselungtechnologien für Office 365)“.

### <a name="encryption-at-rest-for-paas-customers"></a>Verschlüsselung ruhender Daten for PaaS-Kunden

Die Daten von PaaS-Kunden befinden sich üblicherweise in einer Umgebung zur Anwendungsausführung und in jedem Azure-Ressourcenanbieter, der zum Speichern von Kundendaten verwendet wird. Um sich die Optionen für die Verschlüsselung ruhender Daten, die Ihnen zur Verfügung stehen, anzusehen, schauen Sie sich die unten stehende Tabelle zu den von Ihnen verwendeten Speicher- und Anwendungsplattformen an. Wenn dies unterstützt wird, werden Links zu Anweisungen zum Aktivieren der Verschlüsselung ruhender Daten für jeden Ressourcenanbieter angegeben. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Verschlüsselung ruhender Daten für IaaS-Kunden

IaaS-Kunden können mehrere Dienste und Anwendungen verwenden. IaaS-Dienste können die Verschlüsselung ruhender Daten auf ihrer in Azure gehosteten VM und ihren in Azure gehosteten VHDs mithilfe von Azure Disk Encryption aktivieren. 

#### <a name="encrypted-storage"></a>Verschlüsselter Speicher

Genauso wie PaaS können auch IaaS-Lösungen andere Azure-Dienste nutzen, um ruhende verschlüsselte Daten zu speichern. In diesen drei Fällen können Sie die Unterstützung der Verschlüsselung ruhender Daten wie von jeden genutzten Azure-Dienst bereitgestellt ermöglichen. In der unten stehenden Tabelle sind die Hauptspeicher, -dienste und -anwendungsplattformen sowie das unterstützte Modell zur Verschlüsselung ruhender Daten aufgelistet. Wenn dies unterstützt wird, werden Links zu Anweisungen zum Aktivieren der Verschlüsselung ruhender Daten angegeben. 

#### <a name="encrypted-compute"></a>Verschlüsselte Berechnung

Eine vollständige Lösung zur Verschlüsselung ruhender Daten erfordert, dass die Daten niemals in unverschlüsselter Form gespeichert werden. Während die Daten auf einem Server, verwendet werden der die Daten in den Speicher lädt, können sie lokal auf verschiedene Weisen gespeichert werden: mit der Windows-Auslagerungsdatei, einem Absturzabbild und durch Protokolle, die die Anwendung möglicherweise durchführt. Um sicherzustellen, dass diese Daten im Ruhezustand verschlüsselt sind, können IaaS-Anwendungen Azure Disk Encryption auf einem virtuellen Azure-IaaS-Computer (Windows oder Linux) und einem virtuellen Datenträger verwenden. 

#### <a name="custom-encryption-at-rest"></a>Kundenspezifische Verschlüsselung ruhender Daten

Es wird empfohlen, dass IaaS-Anwendungen möglichst die Optionen für Azure Disk Encryption und die Verschlüsselung ruhender Daten nutzen, die von jedem verwendeten Azure-Dienst bereitgestellt werden. In einigen Fällen, wie z.B. bei unregelmäßigen Verschlüsselungsanforderungen oder einem nicht Azure-basierten Speicher, muss der Entwickler einer IaaS-Anwendung möglicherweise selbst die Verschlüsselung ruhender Daten implementieren. Entwickler von IaaS-Lösungen können besser auf die Erwartungen der Azure-Verwaltung und von Kunden eingehen, wenn sie gewisse Azure-Komponenten nutzen. Dies bedeutet, dass Entwickler den Azure Key Vault-Dienst verwenden sollten, um einen sicheren Schlüsselspeicher bereitzustellen sowie ihren Kunden Optionen zur Schlüsselverwaltung zur Verfügung stellen zu können, die denen von anderen Azure-Plattformdiensten entsprechen. Darüber hinaus sollten kundenspezifische Lösungen von Azure verwalteten Dienstidentitäten verwenden, um Dienstkonten den Zugriff auf Verschlüsselungsschlüssel zu ermöglichen. Entwicklerinformationen zu Azure Key Vault und verwalteten Dienstidentitäten finden Sie in den jeweiligen SDKs.

## <a name="azure-resource-providers-encryption-model-support"></a>Unterstützung für Verschlüsselungsmodelle von Azure-Ressourcenanbietern

Microsoft Azure-Dienste unterstützen alle mindestens ein Modell zur Verschlüsselung ruhender Daten. Für einige Dienste stehen jedoch nicht alle Verschlüsselungsmodelle zur Verfügung. Für Dienste, die vom Kunden verwaltete Schlüsselszenarien unterstützen, unterstützen sie ggf. nur eine Teilmenge der Schlüsseltypen, die Azure Key Vault für wichtige Verschlüsselungsschlüssel unterstützt. Zudem kann es sein, dass die Unterstützung dieser Szenarien und Schlüsseltypen anhand unterschiedlicher Zeitpläne in den Diensten hinzugefügt wird. In diesem Abschnitt wird die Unterstützung für die Verschlüsselung ruhender Daten für die Haupt-Datenspeicherdienste von Azure zum Zeitpunkt der Veröffentlichung dieses Artikels beschrieben.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Jeder Kunde, der Funktionen von Azure-IaaS verwendet, kann die Verschlüsselung ruhender Daten für seine IaaS-VMs und -Datenträger mit Azure Disk Encryption erreichen. Weitere Informationen zu Azure Disk Encryption finden Sie in der [Dokumentation zu Azure Disk Enryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Azure-Speicher

Alle Azure Storage-Dienste (Blob Storage, Queue Storage, Table Storage und Azure Files) unterstützen die serverseitige Verschlüsselung im Ruhezustand. Manche Dienste unterstützen auch von Kunden verwaltete Schlüssel und clientseitige Verschlüsselung.  

- Serverseitig: Alle Azure Storage-Dienste ermöglichen standardmäßig die serverseitige Verschlüsselung mithilfe von dienstverwalteten Schlüsseln, was für die Anwendung transparent ist. Weitere Informationen finden Sie unter [Azure Storage Service Encryption für ruhende Daten](https://docs.microsoft.com/azure/storage/storage-service-encryption). Azure Blob Storage und Azure Files unterstützen auch von Kunden verwaltete RSA-Schlüssel mit 2048 Bit in Azure Key Vault. Weitere Informationen finden Sie unter [Azure Storage Service Encryption mit von Kunden verwalteten Schlüsseln in Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).
- Clientseitig: Azure-Blobs, -Tabellen und -Warteschlangen unterstützen die clientseitige Verschlüsselung. Beim Verwenden der clientseitigen Verschlüsselung verschlüsseln Kunden die Daten und laden die Daten als verschlüsselte Blobs hoch. Der Kunde ist für die Schlüsselverwaltung verantwortlich. Weitere Informationen finden Sie unter [Clientseitige Verschlüsselung und Azure Key Vault für Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).


#### <a name="azure-sql-database"></a>Azure SQL-Datenbank

Azure SQL-Datenbank unterstützt aktuell die Verschlüsselung ruhender Daten für von Microsoft verwaltete serverseitige und clientseitige Verschlüsselungsszenarien.

Die Unterstützung für die Serververschlüsselung wird aktuell mit dem SQL-Feature „Transparent Data Encryption“ bereitgestellt. Sobald ein Azure SQL-Datenbank-Kunde TDE aktiviert, werden Schlüssel automatisch für ihn erstellt und verwaltet. Die Verschlüsselung ruhender Daten kann auf Datenbank- und Serverebene aktiviert werden. Seit Juni 2017 ist [Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) standardmäßig in neu erstellten Datenbanken aktiviert. Die Azure SQL-Datenbank unterstützt von Kunden verwaltete RSA-Schlüssel mit 2048 Bit in Azure Key Vault. Weitere Informationen finden Sie unter [Transparent Data Encryption mit BYOK-Unterstützung (Bring Your Own Key) für Azure SQL-Datenbank und Data Warehouse](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql?view=azuresqldb-current).

Die clientseitige Verschlüsselung von Daten von Azure SQL-Datenbank wird durch die Funktion [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) unterstützt. Always Encrypted verwendet einen Schlüssel, der vom Client erstellt und gespeichert wird. Kunden können den Masterschlüssel in einem Windows-Zertifikatspeicher, Azure Key Vault oder einem lokalen Hardwaresicherheitsmodul speichern. Mit SQL Server Management Studio können SQL-Benutzer auswählen, welchen Schlüssel sie zum Verschlüsseln welcher Spalte verwenden möchten.

|                                  |                    | **Verschlüsselungsmodell und Schlüsselverwaltung** |                   |                    |
|----------------------------------|--------------------|--------------------|--------------------|--------------------|
|                                  | **Serverseitig mit vom Dienst verwaltetem Schlüssel**     | **Serverseitig mit vom Kunden verwaltetem Key Vault**             |  **Serverseitig mit vom Kunden lokal verwaltetem Schlüssel**                  | **Client mit clientverwaltetem Schlüssel**      |
| **Speicher und Datenbanken**        |                    |                    |                    |                    |                    |
| Datenträger (IaaS)                      | -                  | Ja, RSA 2048 Bit  | JA               | -                  |
| SQL Server (IaaS)                | JA                | Ja, RSA 2048 Bit  | JA                | JA                |
| Azure SQL-Datenbank/Azure SQL Data Warehouse | JA                | Ja, RSA 2048 Bit  | -                  | JA                |
| Azure Storage (Block-/Seitenblobs) | JA                | Ja, RSA 2048 Bit  | -                  | JA                |
| Azure Storage (Dateien)            | JA                | Ja, RSA 2048 Bit  | -                  | -                  |
| Azure Storage (Tabellen, Warteschlangen)   | JA                | -                  | -                  | JA                |
| Cosmos DB (Document DB)          | JA                | -                  | -                  | -                  |
| StorSimple                       | JA                | -                  | -                  | JA                |
| Backup                           | -                  | -                  | -                  | JA                |
| **Informationen und Analyse**   |                    |                    |                    |                    |
| Azure Data Factory               | JA                | -                  | -                  | -                  |
| Azure Machine Learning           | -                  | RSA 2048 Bit, Vorschauversion | -                  | -                  |
| Azure Stream Analytics           | JA                | -                  | -                  | -                  |
| HDInsight (Azure Blob Storage)   | JA                | -                  | -                  | -                  |
| HDInsight (Data Lake Storage)    | JA                | -                  | -                  | -                  |
| Azure Data Lake Store            | JA                | Ja, RSA 2048 Bit  | -                  | -                  |
| Azure Data Catalog               | JA                | -                  | -                  | -                  |
| Power BI                         | JA                | -                  | -                  | -                  |
| **IoT-Dienste**                 |                    |                    |                    |                    |
| IoT Hub                          | -                  | -                  | -                  | JA                |
| Service Bus                      | JA                | -                  | -                  | JA                |
| Event Hubs                       | JA                | -                  | -                  | -                  |
| Event Grid                       | JA                | -                  | -                  | -                  |


## <a name="conclusion"></a>Zusammenfassung

Der Schutz von Kundendaten, die in Azure-Diensten gespeichert sind, ist für Microsoft von größter Bedeutung. Alle in Azure gehosteten Dienste sollen Optionen für die Verschlüsselung ruhender Daten bereitstellen. Grundlegende Dienste wie Azure Storage, Azure SQL-Datenbank und wichtige Analyse- und Informationsdienste bieten bereits Optionen für die Verschlüsselung ruhender Daten. Einige dieser Dienste unterstützen entweder vom Kunden gesteuerte Schlüssel und clientseitige Verschlüsselung oder vom Dienst verwaltete Schlüssel und serverseitige Verschlüsselung. Microsoft Azure-Dienste verbessern die Verfügbarkeit der Verschlüsselung ruhender Daten deutlich. Zudem sind für die nächsten Monate bereits neue Optionen für die Vorschau und die allgemeinen Verfügbarkeit geplant.
