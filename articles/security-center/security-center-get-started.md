---
title: Schnellstarthandbuch zu Azure Security Center | Microsoft Docs
description: "Dieser Artikel enthält hilfreiche Informationen zum schnellen Einstieg in Azure Security Center. Sie erhalten eine Einführung in die Sicherheitsüberwachungs- und Richtlinienverwaltungs-Komponenten und werden zu weiterführenden Schritten geleitet."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: c28f92af96f31d1c386cf072f83fc142b9a7f588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Schnellstarthandbuch zu Azure Security Center
Dieser Artikel enthält hilfreiche Informationen zum schnellen Einstieg in Azure Security Center. Sie erhalten eine Einführung in die Sicherheitsüberwachungs- und Richtlinienverwaltungs-Komponenten.

## <a name="prerequisites"></a>Voraussetzungen
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Der Free-Tarif von Security Center ist für alle Azure-Abonnements automatisch aktiviert und umfasst die Sicherheitsrichtlinie, die kontinuierliche Bewertung der Sicherheit und umsetzbare Sicherheitsempfehlungen für den Schutz Ihrer Azure-Ressourcen.

Sie können über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)auf Security Center zugreifen. Weitere Informationen zum Azure-Portal finden Sie in der [Dokumentation zum Portal](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Berechtigungen
In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist. Informationen zu Rollen und zulässigen Aktionen in Security Center finden Sie unter [Permissions in Azure Security Center](security-center-permissions.md) (Berechtigungen in Azure Security Center).

## <a name="data-collection"></a>Datensammlung
Security Center sammelt Daten von Ihren virtuellen Azure-Computern (VMs) und Azure-fremden Computern, um sie hinsichtlich Sicherheitslücken und Bedrohungen zu überwachen. Die Daten werden mithilfe von Microsoft Monitoring Agent gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle auf dem Computer und kopiert die Daten zur Analyse in Ihren Arbeitsbereich. Security Center stellt den Microsoft Monitoring Agent auf allen vorhandenen unterstützten virtuellen Azure-Computern sowie auf allen neuen virtuellen Computern bereit, die erstellt werden. Weitere Informationen zur Funktionsweise der Datensammlung finden Sie unter [Aktivieren der Datensammlung](security-center-enable-data-collection.md).

Die automatische Bereitstellung wird dringend empfohlen und für Abonnements des Standard-Tarifs von Security Center vorausgesetzt. Wenn Sie die automatische Bereitstellung deaktivieren, schränkt dies die Sicherheitsüberwachung für Ihre Ressourcen ein.

Weitere Informationen zu den Tarifen Standard und Free finden Sie unter [Azure Security Center-Preise](security-center-pricing.md).

In den folgenden Schritten wird beschrieben, wie Sie auf die Komponenten von Security Center zugreifen und diese verwenden.

> [!NOTE]
> Im Artikel wird der Dienst anhand einer Beispielbereitstellung vorgestellt. Es handelt sich nicht um eine Schritt-für-Schritt-Anleitung.
>
>

## <a name="access-security-center"></a>Zugreifen auf Security Center
Führen Sie im Portal folgende Schritte aus, um auf Security Center zuzugreifen:

1. Wählen Sie im Menü **Microsoft Azure** die Option **Security Center**.

   ![Azure-Menü][1]
2. Wenn Sie zum ersten Mal auf Security Center zugreifen, wird das Blatt **Willkommen** geöffnet. Wählen Sie **Security Center starten** aus, um **Security Center** zu öffnen.
   ![Bildschirm „Willkommen“][10]
3. Nachdem Sie Security Center über das Blatt „Willkommen“ gestartet oder im Microsoft Azure-Menü die Option „Security Center“ gewählt haben, wird **Security Center** geöffnet. Um zukünftig einfacher auf das Blatt **Security Center** zugreifen zu können, wählen Sie die Option **Blatt an Dashboard anheften** (oben rechts) aus.
   ![Option „Blatt an Dashboard anheften“][2]

## <a name="use-security-center"></a>Verwenden von Security Center
Sie können die Sicherheitsrichtlinien für Ihre Azure-Abonnements und Ressourcengruppen konfigurieren. Wir konfigurieren nun eine Sicherheitsrichtlinie für Ihr Abonnement:

1. Klicken Sie im Hauptmenü von Security Center auf **Sicherheitsrichtlinie**.
2. Wählen Sie unter **Security Center – Sicherheitsrichtlinie** ein Abonnement aus.
3. Unter **Sicherheitsrichtlinie – Datensammlung** ist **Automatische Bereitstellung** aktiviert. Security Center stellt den Microsoft Monitoring Agent auf allen vorhandenen unterstützten virtuellen Azure-Computern sowie auf allen neuen virtuellen Computern bereit, die erstellt werden.

    ![Sicherheitsrichtlinie][12]

4. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** die Richtlinienkomponente **Sicherheitsrichtlinie** aus.

     ![Sicherheitsrichtlinie][11]

5. Aktivieren Sie unter **Empfehlungen anzeigen für** die Empfehlungen, die im Rahmen der Sicherheitsrichtlinie verwendet werden sollen. Beispiele:

   * Durch Festlegen von **Systemupdates** auf **Ein** werden alle unterstützten VMs auf fehlende Betriebssystemupdates überprüft.
   * Durch Festlegen von **Sicherheitsrisiken des Betriebssystems** auf **Ein** werden alle unterstützten VMs untersucht, um Betriebssystemkonfigurationen zu ermitteln, die den virtuellen Computer anfälliger für Angriffe machen können.

6. Wählen Sie **Speichern** aus.

### <a name="view-recommendations"></a>Anzeigen von Empfehlungen
1. Kehren Sie zum Blatt **Security Center** zurück, und klicken Sie auf die Kachel **Empfehlungen**. Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Wenn von Security Center potenzielle Sicherheitsrisiken identifiziert werden, werden die Empfehlungen auf dem Blatt **Empfehlungen** angezeigt.
   ![Empfehlungen in Azure Security Center][5]
2. Wählen Sie eine Empfehlung auf dem Blatt **Empfehlungen** aus, um weitere Informationen anzuzeigen und Maßnahmen zum Beheben des Problems zu ergreifen.

### <a name="view-the-security-state-of-your-resources"></a>Anzeigen des Sicherheitsstatus Ihrer Ressourcen
1. Kehren Sie zum Blatt **Security Center** zurück. Der Abschnitt **Prävention** des Dashboards enthält Indikatoren, die den Sicherheitsstatus für VMs, Netzwerk, Daten und Anwendungen anzeigen.
2. Wählen Sie **Compute**, um weitere Informationen anzuzeigen. Das Blatt **Compute** wird mit drei Registerkarten geöffnet:

  - **Übersicht**: Enthält Empfehlungen zur Überwachung und zu VMs.
  - **VMs und Computer:** Liste mit dem aktuellen Sicherheitszustand aller virtueller und physischen Computer.
  - **Clouddienste**: Liste mit den von Security Center überwachten Web- und Workerrollen

    ![Computesicherheitsintegrität][6]

3. Wählen Sie auf der Registerkarte **Übersicht** eine Empfehlung aus, um weitere Informationen anzuzeigen und/oder Maßnahmen zur Konfiguration der erforderlichen Steuerelemente zu ergreifen.
4. Wählen Sie auf der Registerkarte **VMs und Computer** eine Ressource aus, um zusätzliche Details anzuzeigen.

### <a name="view-security-alerts"></a>Anzeigen von Sicherheitswarnungen
1. Kehren Sie zum Blatt **Security Center** zurück, und klicken Sie auf die Kachel **Sicherheitswarnungen**. Das Blatt **Sicherheitswarnungen** wird mit einer Liste angezeigt, in der die Warnungen aufgeführt sind. Die Warnungen werden bei der Security Center-Analyse Ihrer Sicherheitsprotokolle und der Netzwerkaktivität generiert. Die Liste enthält Warnungen integrierter Partnerlösungen.
   ![Sicherheitswarnungen in Azure Security Center][7]

2. Wählen Sie eine Warnung aus, um weitere Informationen anzuzeigen. Bei diesem Beispiel wählen wir die Option **Erkennung der Änderung einer Systembinärdatei in Dumpfilter**. Es werden Blätter geöffnet, die weitere Details zur Warnung enthalten.
   ![Details zu Sicherheitswarnungen in Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Anzeigen der Integrität Ihrer Partnerlösungen
1. Kehren Sie zum Blatt **Security Center** zurück. Auf der Kachel **Sicherheitslösungen** können Sie auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen, die in Ihr Azure-Abonnement integriert sind.
2. Wählen Sie die Kachel **Sicherheitslösungen** aus. Ein Blatt mit einer Liste der Partnerlösungen, die mit dem Security Center verbunden sind, wird geöffnet.
   ![Partnerlösungen][9]
3. Wählen Sie eine Partnerlösung aus. Auf dem geöffneten Blatt werden der Status der Partnerlösung und die zugeordneten Ressourcen der Lösung angezeigt. Wählen Sie die Option **Lösungskonsole** , um die Verwaltungsoberfläche des Partners für diese Lösung zu öffnen.

   ![Partnerlösungen][13]

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden die Security Center-Komponenten für die Sicherheitsüberwachung und Richtlinienverwaltung vorgestellt. Nachdem Sie sich jetzt mit Security Center vertraut gemacht haben, können Sie die folgenden Schritte ausprobieren:

* Informationen dazu, wie eine Sicherheitsrichtlinie für Ihr Azure-Abonnement konfiguriert wird, finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md).
* Unter [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md) erfahren Sie, wie die Empfehlungen in Security Center Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
* Unter [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) finden Sie Informationen zum Überprüfen und Verwalten Ihrer aktuellen Sicherheitswarnungen.
- Unter [Partner- und Lösungsintegration](security-center-partner-integration.md) erfahren Sie mehr zur Integration mit Partnern zum Verbessern der allgemeinen Sicherheit.
- Unter [Azure Security Center-Datensicherheit](security-center-data-security.md) erfahren Sie, wie Daten in Security Center verwaltet und geschützt werden.
* Informieren Sie sich über die [Funktionen der erweiterten Bedrohungserkennung](security-center-detection-capabilities.md), die Teil des [Standard-Tarifs](security-center-pricing.md) von Security Center sind. Der Standard-Tarif wird für die ersten 60 Tage kostenlos angeboten.
* Antworten auf Fragen zur Verwendung von Security Center finden Sie unter [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
[11]: ./media/security-center-get-started/show-recommendations-for.png
[12]: ./media/security-center-get-started/automatic-provisioning.png
[13]: ./media/security-center-get-started/partner-solutions-detail.png
