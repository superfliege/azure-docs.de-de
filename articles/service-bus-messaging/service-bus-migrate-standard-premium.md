---
title: Migrieren von vorhandenen Azure Service Bus-Standardnamespaces zum Premium-Tarif | Microsoft-Dokumentation
description: Anleitung zur Migration von vorhandenen Azure Service Bus-Standardnamespaces zu Premium
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: darosa
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2019
ms.author: aschhab
ms.openlocfilehash: 7b153c36e10f1d4e2be2a0cf42f998c31cb6473a
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896723"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-premium-tier"></a>Migrieren von vorhandenen Azure Service Bus-Standardnamespaces zum Premium-Tarif

Bisher bot der Azure Service Bus Namespaces nur im Standard-Tarif an. Dabei handelte es sich um mandantenfähige Setups, die für niedrige Durchsätze und Entwicklerumgebungen optimiert wurden.

In der jüngsten Vergangenheit wurde Azure Service Bus um den Premium-Tarif erweitert, der dedizierte Ressourcen pro Namespace für prognostizierbare Latenzzeiten und erhöhten Durchsatz zu einem Festpreis anbietet, der für hohe Durchsatzraten und Produktionsumgebungen optimiert ist, die zusätzliche Unternehmensfunktionen erfordern.

Die folgenden Tools ermöglichen die Migration bestehender Namespaces vom Standard-Tarif zum Premium-Tarif.

>[!WARNING]
> Mit der Migration wird ein ***Upgrade*** von Service Bus-Standardnamespaces zum Premium-Tarif durchgeführt.
>
> Ein Herabstufen wird von den Migrationstools ***nicht*** unterstützt.
>[!NOTE]
> Diese Migration wird ***lokal*** ausgeführt.
>
> Dies bedeutet, dass bestehende Sender- und Empfängeranwendungen keinen Code oder Konfigurationsänderungen erfordern.
>
> Die bestehende Verbindungszeichenfolge verweist automatisch auf den neuen Premiumnamespace.
>
> Zusätzlich werden alle Entitäten im Standardnamespace während des Migrationsprozesses in den Premiumnamespace **kopiert**.
>
>
> Wir unterstützen ***1000 Einheiten pro Messagingeinheit*** auf Premium. Um also zu ermitteln, wie viele Messagingeinheiten Sie benötigen, beginnen Sie bitte mit der Anzahl der Einheiten, die Sie in Ihrem aktuellen Standardnamespace haben.

## <a name="migration-steps"></a>Schritte bei der Migration

>[!IMPORTANT]
> Es gibt einige Einschränkungen im Zusammenhang mit dem Migrationsprozess. Wir bitten Sie, sich mit den Schritten zur Reduzierung der Fehlermöglichkeiten vertraut zu machen.

Der konkrete Schritt-für-Schritt-Migrationsprozess wird in den folgenden Leitfäden beschrieben.

Die logischen Schritte sind:

1. Erstellen eines neuen Premiumnamespace
2. Koppeln des Standard- und Premiumnamespace miteinander
3. Synchronisieren (Kopieren) der Entitäten vom Standard zum Premiumnamespace
4. Committen der Migration
5. Leeren der Entitäten im Standardnamespace unter Verwendung des Namens nach der Migration des Namespace
6. Löschen des Standardnamespace

>[!NOTE]
> Nach dem Committen der Migration ist es äußerst wichtig, auf den alten Standardnamespace zuzugreifen und die Warteschlangen und Abonnements zu leeren.
>
> Nach dem Entleeren der Nachrichten können diese an den neuen Premiumnamespace gesendet und von den Empfängeranwendungen verarbeitet werden.
>
> Nachdem die Warteschlangen und Abonnements geleert wurden, empfehlen wir, den alten Standardnamespace zu löschen. Sie benötigen ihn nicht mehr.

### <a name="migrate-using-azure-cli-or-powershell"></a>Migrieren mit der Azure CLI oder PowerShell

Informationen zur Migration Ihres Service Bus-Standardnamespace zu Premium mit der Azure CLI oder dem PowerShell-Tool finden Sie in der folgenden Anleitung.

1. Erstellen Sie einen neuen Service Bus-Premiumnamespace. Sie können auf die [Azure Resource Manager-Vorlagen](service-bus-resource-manager-namespace.md) verweisen oder das [Azure-Portal](service-bus-create-namespace-portal.md) verwenden. Wählen Sie für den **serviceBusSku**-Parameter „Premium“ aus.

2. Legen Sie die folgenden Umgebungsvariablen fest, um die Migrationsbefehle zu vereinfachen.
   ```
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the Premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the Standard namespace>
   ```

    >[!IMPORTANT]
    > Der Name nach der Migration (Post_migration_dns_name) wird verwendet, um nach der Migration auf den alten Standardnamespace zuzugreifen. Damit müssen Sie die Warteschlangen und die Abonnements entleeren und dann den Namespace löschen.

3. **Koppeln** Sie die Standard- und Premiumnamespaces und **Starten Sie die Synchronisierung** mit dem folgenden Befehl:

    ```
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


4. Überprüfen des Status der Migration mit dem folgenden Befehl:
    ```
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    Die Migration gilt als abgeschlossen, wenn:
    * MigrationState = "Active"
    * pendingReplicationsOperationsCount = 0
    * provisioningState = "Succeeded"

    Dieser Befehl zeigt außerdem die Migrationskonfiguration an. Bitte überprüfen Sie nochmals, ob die Werte wie zuvor angegeben eingestellt sind.

    Überprüfen Sie außerdem den Premiumnamespace im Portal, um sicherzustellen, dass alle Warteschlangen und Themen angelegt wurden und mit dem übereinstimmen, was im Standardnamespace vorhanden war.

5. Committen Sie die Migration, indem Sie den vollständigen Befehl unten ausführen.
   ```
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-using-azure-portal"></a>Migrieren über das Azure-Portal

Die Migration über das Azure-Portal hat den gleichen logischen Ablauf wie die Migration über die Befehle. Lesen Sie die folgende Anleitung für den schrittweisen Ablauf der Migration über das Portal.

1. Wählen Sie die Menüoption **Zu Premium migrieren** im Navigationsmenü auf der linken Seite. Klicken Sie auf die Schaltfläche **Erste Schritte**, um zur nächsten Seite zu gelangen.
    ![Landing Page der Migration][]

2. Schließen Sie das **Setup** ab.
   ![Setup für Namespace][]
   1. Erstellen Sie den Premiumnamespace, zu dem der vorhandene Standardnamespace migriert werden soll, und weisen Sie ihn zu.
        ![Setup des Namespace: erstellen des Premiumnamespace][]
   2. Wählen Sie **Name nach der Migration**, um nach Abschluss der Migration auf den Standardnamespace zuzugreifen.
        ![Setup des Namespace: Auswahl des Namens nach der Migration][]
   3. Klicken Sie zum Fortfahren auf **Weiter**.
3. **Synchronisieren** Sie die Entitäten zwischen dem Standard- und dem Premiumnamespace.
    ![Setup des Namespace - Synchronisieren von-Entitäten – Start][]

   1. Klicken Sie auf **Synchronisierung starten**, um mit dem Synchronisieren von Entitäten zu beginnen.
   2. Klicken Sie auf **Ja** im Popupfenster, um zu bestätigen, dass die Synchronisierung gestartet werden kann.
   3. Warten Sie, bis die **Synchronisierung** abgeschlossen ist. Der Status wird in der Statusleiste angezeigt.
        ![Setup des Namespace - Synchronisieren von-Entitäten – Fortschritt][]
        >[!IMPORTANT]
        > Wenn Sie den Vorgang aus irgendeinem Grund **abbrechen** müssen, lesen Sie bitte die entsprechende Anleitung im Abschnitt „FAQ“ in diesem Dokument.
   4. Sobald die Synchronisierung abgeschlossen ist, klicken Sie auf die Schaltfläche **Weiter** unten auf der Seite.

4. Überprüfen Sie die Änderungen auf der Seite „Zusammenfassung“.
    ![Namespace wechseln – Switch-Menü][]

5. Klicken Sie auf **Migration abschließen**, um den Namespace zu wechseln und die Migration abzuschließen.
    ![Namespace wechseln – Erfolg][]

## <a name="faqs"></a>Häufig gestellte Fragen

### <a name="what-happens-when-the-migration-is-committed"></a>Was geschieht, wenn die Migration committet ist?

Nach dem Committen der Migration zeigt die Verbindungszeichenfolge, die auf den Standardnamespace verweist, auf den Premiumnamespace.

Die Sender- und Empfängeranwendungen trennen die Verbindung zum Standardnamespace und verbinden sich automatisch wieder mit dem Premiumnamespace.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>Was mache ich, nachdem die Standard-zu-Premium-Migration abgeschlossen ist?

Die Standard-zu-Premium-Migration stellt sicher, dass die Metadaten der Entität (Themen, Abonnements, Filter, etc.) aus dem Standard- zu Premiumnamespace kopiert werden. Die Nachrichtendaten, die in den Standardnamespace übertragen wurden, werden nicht aus dem Standard- in den Premiumnamespace kopiert.

Aus diesem Grund kann der Standardnamespace einige Nachrichten enthalten, die während der laufenden Migration gesendet und committet wurden. Diese Nachrichten müssen manuell aus dem Standardnamespace entleert und manuell an den Premiumnamespace gesendet werden.

Dazu ***müssen***  Sie eine Konsolen-App oder ein Skript verwenden, die bzw. das die Entitäten des Standardnamespace mit dem **DNS-Namen nach der Migration**, den Sie in den Migrationsbefehlen angegeben haben, entleert und diese Nachrichten dann an den Premiumnamespace sendet, damit sie von den Empfängern verarbeitet werden können.

Nachdem die Nachrichten geleert wurden, löschen Sie bitte den Standardnamespace.

>[!IMPORTANT]
> Bitte beachten Sie, dass Sie nach der Entleerung der Nachrichten aus dem Standardnamespace diesen löschen **müssen**.
>
> Dies ist wichtig, da die Verbindungszeichenfolge, die zunächst auf den Standardnamespace verwies, nun tatsächlich auf den Premiumnamespace verweist. Sie benötigen den Standardnamespace nicht mehr.
>
> Wenn Sie den migrierten Standardnamespace löschen, kommt es später zu weniger Unklarheiten. 

### <a name="how-much-downtime-do-i-expect"></a>Wie viel Ausfallzeit kann ich erwarten?
Der oben beschriebene Migrationsprozess soll die zu erwartenden Ausfallzeiten der Anwendungen reduzieren. Dies geschieht durch die Verwendung der Verbindungszeichenfolge, die die Sender- und Empfängeranwendungen verwenden, um auf den neuen Premiumnamespace zu verweisen.

Die Ausfallzeit der Anwendung ist auf die Zeit beschränkt, die benötigt wird, um den DNS-Eintrag zu aktualisieren und auf den Premiumnamespace zu verweisen.

Dies kann ***ca. fünf Minuten*** dauern.

### <a name="do-i-have-to-make-any-configuration-changes-while-performing-the-migration"></a>Muss ich während der Migration irgendwelche Konfigurationsänderungen vornehmen?
Nein, es sind keine Code-/Konfigurationsänderungen erforderlich, um diese Migration durchzuführen. Die Verbindungszeichenfolge, die Sender- und Empfängeranwendungen für den Zugriff auf den Standardnamespace verwenden, wird automatisch als **Alias** für den Premiumnamespace abgebildet.

### <a name="what-happens-when-i-abort-the-migration"></a>Was geschieht, wenn ich die Migration abbreche?
Die Migration kann entweder mit dem Befehl „Abbrechen“ oder über das Azure-Portal abgebrochen werden. 

#### <a name="azure-cli-or-powershell"></a>Azure CLI oder PowerShell

    az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace

#### <a name="azure-portal"></a>Azure-Portal

![Abbruchablauf – Synchronisierung abbrechen][]
![Abbruchablauf – Abbruch abgeschlossen][]

Wenn der Migrationsprozess abgebrochen wird, wird der Kopiervorgang der Entitäten (Themen, Abonnements und Filter) vom Standard- in den Premiumnamespace ab- und die Kopplung unterbrochen.

Die Verbindungszeichenfolge wird **nicht** so aktualisiert, dass sie auf den Premiumnamespace verweist. Ihre bestehenden Anwendungen funktionieren weiterhin so, wie vor Beginn der Migration.

Allerdings werden **keine** Entitäten im Premiumnamespace oder der Premiumnamespace selbst gelöscht. Dies muss manuell erfolgen, wenn Sie sich entschieden haben, die Migration doch nicht fortzusetzen.

>[!IMPORTANT]
> Wenn Sie sich entscheiden, die Migration abzubrechen, löschen Sie bitte den Premiumnamespace, den Sie für die Migration bereitgestellt haben, damit Ihnen die Ressourcen nicht in Rechnung gestellt werden.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>Ich möchte die Nachrichten nicht entleeren. Wie gehe ich vor?

Es kann Nachrichten geben, die von den Senderanwendungen gesendet und auf den Speicher im Standardnamespace committet werden, während die Migration stattfindet, und zwar unmittelbar vor der Migration.

Da bei der Migration die eigentlichen Nachrichtendaten/-nutzlasten nicht von Standard zu Premium kopiert werden, müssen diese manuell entleert und dann an den Premiumnamespace gesendet werden.

Wenn Sie jedoch während eines geplanten Wartungs-/Housekeeptingfensters migrieren können und die Nachrichten nicht manuell entleeren und versenden möchten, führen Sie bitte die folgenden Schritte aus:

1. Halten Sie die Senderanwendungen an, und ermöglichen Sie den Empfängern, die Nachrichten zu verarbeiten, die sich derzeit im Standardnamespace befinden, und leeren Sie die Warteschlange.
2. Sobald die Warteschlangen und Abonnements im Standardnamespace leer sind, führen Sie die oben beschriebene Vorgehensweise durch, um die Migration vom Standard- zum Premiumnamespace durchzuführen.
3. Sobald die Migration abgeschlossen ist, können Sie die Senderanwendungen neu starten.
4. Der Absender und Empfänger werden jetzt automatisch mit dem Premiumnamespace verbunden.

    >[!NOTE]
    > Der Empfänger muss für die Migration nicht beendet werden.
    >
    > Nach Abschluss der Migration trennen die Empfänger die Verbindung zum Standardnamespace und verbinden sich automatisch mit dem Premiumnamespace.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [Unterschiede zwischen Standard- und Premiummessaging](./service-bus-premium-messaging.md).
* [Hier](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium) erfahren Sie mehr über Aspekte der Hochverfügbarkeit und Geo-Notfallwiederhierstellung für Service Bus Premium.

[Landing Page der Migration]: ./media/service-bus-standard-premium-migration/1.png
[Setup für Namespace]: ./media/service-bus-standard-premium-migration/2.png
[Setup des Namespace: erstellen des Premiumnamespace]: ./media/service-bus-standard-premium-migration/3.png
[Setup des Namespace: Auswahl des Namens nach der Migration]: ./media/service-bus-standard-premium-migration/4.png
[Setup des Namespace – Synchronisieren von-Entitäten – Start]: ./media/service-bus-standard-premium-migration/5.png
[Setup des Namespace – Synchronisieren von-Entitäten – Fortschritt]: ./media/service-bus-standard-premium-migration/8.png
[Namespace wechseln – Switch-Menü]: ./media/service-bus-standard-premium-migration/9.png
[Namespace wechseln – Erfolg]: ./media/service-bus-standard-premium-migration/12.png

[Abbruchablauf – Synchronisierung abbrechen]: ./media/service-bus-standard-premium-migration/abort1.png
[Abbruchablauf – Abbruch abgeschlossen]: ./media/service-bus-standard-premium-migration/abort3.png
