---
title: Adaptive Anwendungssteuerungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, wie Sie Anwendungen, die auf virtuellen Azure-Computern ausgeführt werden, mithilfe der adaptiven Anwendungssteuerung in Azure Security Center einer Whitelist hinzufügen.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2018
ms.author: rkarlin
ms.openlocfilehash: 6dd971ff8cd51435978ab80db006f6494dff8a94
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389326"
---
# <a name="adaptive-application-controls-in-azure-security-center"></a>Adaptive Anwendungssteuerungen in Azure Security Center
In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie die Anwendungssteuerung in Azure Security Center verwenden.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Was sind adaptive Anwendungssteuerungen in Security Center?
Mit adaptiven Anwendungssteuerungen lässt sich steuern, welche Anwendungen auf Ihren virtuellen Computern in Azure ausgeführt werden können. Dadurch können Sie Ihre virtuellen Computer unter anderem besser vor Schadsoftware schützen. Security Center nutzt Machine Learning, um die auf dem virtuellen Computer ausgeführten Anwendungen zu analysieren, und unterstützt Sie beim Anwenden von Whitelistregeln, die auf diesen Daten basieren. Dadurch können Sie Anwendungswhitelists deutlich einfacher konfigurieren und verwalten und profitieren außerdem von folgenden Möglichkeiten:

- Blockierung der Ausführung schädlicher Anwendungen oder Ausgabe einer Warnung beim Versuch, eine schädliche Anwendung auszuführen (auch bei Anwendungen, die möglicherweise von Antischadsoftwarelösungen nicht erfasst werden)
- Einhaltung der Sicherheitsrichtlinie Ihrer Organisation, die die ausschließliche Verwendung von lizenzierter Software vorschreibt
- Vermeidung der Verwendung von unerwünschter Software in Ihrer Umgebung
- Vermeidung der Ausführung veralteter oder nicht unterstützter Apps
- Verhinderung der Verwendung bestimmter Softwaretools, die in Ihrer Organisation nicht zulässig sind
- Steuerung des Zugriffs auf sensible Daten im Rahmen der App-Nutzung durch die IT-Abteilung

## <a name="how-to-enable-adaptive-application-controls"></a>Aktivieren adaptiver Anwendungssteuerungen
Mit adaptiven Anwendungssteuerungen können Sie eine Gruppe von Anwendungen definieren, deren Ausführung in konfigurierten Gruppen zulässig ist. Dieses Feature steht nur für Windows-Computer (alle Versionen, klassisch oder Azure Resource Manager) zur Verfügung. Gehen Sie wie folgt vor, um das Anwendungswhitelisting in Security Center zu konfigurieren:

1. Öffnen Sie das Dashboard **Security Center**.
2. Wählen Sie im linken Bereich unter **Erweiterter Cloudschutz** die Option **Adaptive Anwendungssteuerung**.

    ![Verteidigung](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)

Die Seite **Adaptive Anwendungssteuerung** wird angezeigt.

![controls](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

Der Abschnitt **Groups of VMs** (VM-Gruppen) enthält drei Registerkarten:

* **Konfiguriert**: Liste der Gruppen mit den virtuellen Computern, für die die Anwendungssteuerung bereits konfiguriert ist.
* **Empfohlen**: Liste der Gruppen, für die die Anwendungssteuerung empfohlen wird. Security Center nutzt Machine Learning, um geeignete virtuelle Computer für die Anwendungssteuerung basierend darauf zu ermitteln, ob die virtuellen Computer konsistent die gleichen Anwendungen ausführen.
* **Keine Empfehlung:** Liste der Gruppen mit virtuellen Computern, für die keine Anwendungssteuerungsempfehlungen vorliegen. Ein Beispiel wären etwa virtuelle Computer, auf denen sich die Anwendungen immer wieder ändern und die keinen konsistenten Zustand erreicht haben.

> [!NOTE]
> Das Sicherheitscenter verwendet für die Erstellung von VM-Gruppen einen proprietären Clustering-Algorithmus, der sicherstellt, dass ähnliche virtuelle Computer die optimale empfohlene Anwendungssteuerungsrichtlinie erhalten.
>
>

### <a name="configure-a-new-application-control-policy"></a>Konfigurieren einer neuen Anwendungssteuerungsrichtlinie
1. Klicken Sie auf die Registerkarte **Empfohlen**, um eine Liste der Gruppen mit Anwendungssteuerungsempfehlungen anzuzeigen:

  ![Empfohlen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

  Die Liste enthält Folgendes:

  - **NAME**: Der Name des Abonnements und der Gruppe.
  - **VMs**: Die Anzahl virtueller Computer in der Gruppe.
  - **ZUSTAND**: Der Zustand der Empfehlungen (in der Regel „Offen“).
  - **SCHWEREGRAD**: Der Schweregrad der Empfehlungen.

2. Wählen Sie eine Gruppe aus, um die Option **Regeln zur Anwendungssteuerung erstellen** zu öffnen.

  ![Anwendungssteuerungsregeln](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

3. Sehen Sie sich unter **VMs auswählen** die Liste mit den empfohlenen virtuellen Computern an, und heben Sie die Auswahl aller virtuellen Computer auf, auf die Sie die Anwendungssteuerung nicht anwenden möchten. Als Nächstes sehen Sie zwei Listen:

  - **Recommended applications** (Empfohlene Anwendungen): Eine Liste mit Anwendungen, die häufig auf den virtuellen Computern in dieser Gruppe vorhanden sind und für die deshalb Anwendungssteuerungsregeln empfohlen werden.
  - **More applications** (Weitere Anwendungen): Eine Liste mit Anwendungen, die weniger häufig auf den virtuellen Computern in dieser Gruppe vorhanden oder als Sicherheitslücken bekannt sind (siehe weiter unten) und vor der Anwendung der Regeln überprüft werden sollten.

4. Überprüfen Sie die Anwendungen in den einzelnen Listen, und deaktivieren Sie die Kontrollkästchen aller Anwendungen, die Sie nicht anwenden möchten. Die Listen enthalten jeweils Folgendes:

  - **NAME**: Die Zertifikatinformationen einer Anwendung oder der vollständige Anwendungspfad.
  - **DATEITYPEN**: Der Anwendungsdateityp. Mögliche Optionen sind „EXE“, „Skript“ und „MSI“.
  - **SICHERHEITSLÜCKE**: Ein Warnsymbol gibt an, ob die Anwendungen von einem Angreifer zur Umgehung des Anwendungswhitelistings verwendet werden können. Es empfiehlt sich, diese Anwendungen vor der Genehmigung zu überprüfen.
  - **BENUTZER**: Benutzer, denen das Ausführen einer Anwendung ermöglicht werden sollte.

5. Klicken Sie nach Abschluss Ihrer Auswahl auf **Erstellen**.


> [!NOTE]
> - Security Center zieht zur Erstellung einer Baseline sowie für die individuellen Empfehlungen pro VM-Gruppe die Daten eines Zeitraums von mindestens zwei Wochen heran. Bei neuen Kunden mit dem Standardtarif von Security Center werden VM-Gruppen zunächst auf der Registerkarte *Keine Empfehlung* angezeigt.
> - Adaptive Anwendungssteuerungen in Security Center unterstützen keine virtuellen Computer, für die entweder über ein Gruppenrichtlinienobjekt oder eine lokale Sicherheitsrichtlinie bereits eine AppLocker-Richtlinie aktiviert wurde.
> -  Aus Sicherheitsgründen versucht Security Center immer, eine Herausgeberregel für die Anwendungen zu erstellen, die in die Whitelist aufgenommen werden sollen. Eine Pfadregel für den vollständigen Pfad der spezifischen EXE-Datei wird nur erstellt, wenn eine Anwendung über keine Herausgeberinformationen verfügt (also nicht signiert ist).
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Bearbeiten und Überwachen einer mit Anwendungssteuerung konfigurierten Gruppe

1. Kehren Sie zur Seite **Adaptive Anwendungssteuerung** zurück, und klicken Sie unter **Groups of VMs** (VM-Gruppen) auf **KONFIGURIERT**, um eine mit Anwendungssteuerung konfigurierte Gruppe zu bearbeiten und zu überwachen:

  ![Gruppen](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

  Die Liste enthält Folgendes:

  - **NAME**: Der Name des Abonnements und der Gruppe.
  - **VMs**: Die Anzahl virtueller Computer in der Gruppe.
  - **MODUS**: Im Überwachungsmodus werden Versuche, eine nicht in der Whitelist enthaltene Anwendung auszuführen, protokolliert. Im Erzwingungsmodus wird die Ausführung von Anwendungen verhindert, die sich nicht in der Whitelist befinden.
  - **PROBLEME**: Alle derzeitigen Verstöße.

2. Wählen Sie eine Gruppe aus, um Änderungen auf der Seite **Richtlinie zur Anwendungssteuerung bearbeiten** vorzunehmen.

  ![Schutz](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. Unter **Schutzmodus** steht Folgendes zur Auswahl:

  - **Überwachen**: In diesem Modus werden die Regeln von der Anwendungssteuerungslösung nicht erzwungen und nur die Aktivitäten auf den geschützten virtuellen Computern überwacht. Dies empfiehlt sich, wenn Sie zuerst das allgemeine Verhalten untersuchen möchten, bevor Sie die Ausführung einer App auf dem virtuellen Zielcomputer blockieren.
  - **Erzwingen**: In diesem Modus werden die Regeln von der Anwendungssteuerungslösung erzwungen, und Anwendungen, deren Ausführung nicht zulässig ist, werden blockiert.

  Wie bereits erwähnt, werden neue Anwendungssteuerungsrichtlinien standardmäßig immer im *Überwachungsmodus* konfiguriert. Unter **Policy extension** (Richtlinienerweiterung) können Sie eigene Anwendungspfade hinzufügen, die in die Whitelist aufgenommen werden sollen. Nach dem Hinzufügen dieser Pfade erstellt Security Center zusätzlich zu den bereits vorhandenen Regeln die entsprechenden Regeln für diese Anwendungen.

  Im Abschnitt **Neue Probleme** werden alle aktuellen Verstöße aufgeführt.

  ![Probleme](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

  Diese Liste enthält:
  - **PROBLEME**: Alle protokollierten Verstöße. Mögliche Optionen:

      - **ViolationsBlocked**: Tritt auf, wenn sich die Lösung im Erzwingungsmodus befindet und versucht wird, eine Anwendung auszuführen, die nicht in der Whitelist enthalten ist.
      - **ViolationsAudited**: Tritt auf, wenn sich die Lösung im Überwachungsmodus befindet und eine Anwendung ausgeführt wird, die nicht in der Whitelist enthalten ist.

 - **Anzahl OF VMS** (ANZAHL VIRTUELLER COMPUTER): Die Anzahl virtueller Computer mit diesem Problemtyp.

  Durch Klicken auf die einzelnen Zeilen gelangen Sie zur Seite [Azure-Aktivitätsprotokoll](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Hier finden Sie Informationen zu allen virtuellen Computern mit dieser Art von Verstoß. Wenn Sie auf die drei Punkte am Zeilenende klicken, können Sie den entsprechenden Eintrag löschen. Der Abschnitt **Configured virtual machines** (Konfigurierte virtuelle Computer) enthält die virtuellen Computer, für die diese Regeln gelten.

  ![Konfigurierte virtuelle Computer](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

  Unter **Regeln für Herausgeberwhitelists** enthält die Liste Folgendes:

  - **REGEL**: Anwendungen, für die eine Herausgeberregel erstellt wurde – basierend auf den Zertifikatinformationen, die für die jeweilige Anwendung gefunden wurden.
  - **DATEITYP**: Die Dateitypen, die durch eine bestimmte Herausgeberregel abgedeckt sind. Mögliche Optionen: „EXE“, „Skript“ oder „MSI“.
  - **BENUTZER**: Anzahl von Benutzern, die zum Ausführen der einzelnen Anwendungen berechtigt sind.

  Weitere Informationen finden Sie unter [Understanding the publisher rule condition in AppLocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) (Grundlegendes zur Herausgeberregelbedingung in AppLocker).

  ![Whitelistregeln](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

  Wenn Sie auf die drei Punkte am Zeilenende klicken, können Sie die entsprechende Regel löschen oder die berechtigten Benutzer bearbeiten.

  Im Abschnitt **Regeln für Pfadwhitelists** wird der vollständige Anwendungspfad (einschließlich des spezifischen Dateityps) für die Anwendungen aufgeführt, die nicht mit einem digitalen Zertifikat signiert, aber in den Whitelistregeln immer noch aktuell sind.

  > [!NOTE]
  > Aus Sicherheitsgründen versucht Security Center standardmäßig, eine Herausgeberregel für die ausführbaren Dateien zu erstellen, die in die Whitelist aufgenommen werden sollen. Eine Pfadregel für den vollständigen Pfad der spezifischen EXE-Datei wird nur erstellt, wenn eine ausführbare Datei über keine Herausgeberinformationen verfügt (also nicht signiert ist).

  ![Regeln für Pfadwhitelists](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

  Die Liste enthält Folgendes:
  - **NAME**: Der vollständige Patch der ausführbaren Datei.
  - **DATEITYP**: Die Dateitypen, die durch eine bestimmte Pfadregel abgedeckt sind. Mögliche Optionen: „EXE“, „Skript“ oder „MSI“.
  - **BENUTZER**: Anzahl von Benutzern, die zum Ausführen der einzelnen Anwendungen berechtigt sind.

  Wenn Sie auf die drei Punkte am Zeilenende klicken, können Sie die entsprechende Regel löschen oder die berechtigten Benutzer bearbeiten.

4. Klicken Sie auf die Schaltfläche **Speichern**, nachdem Sie die Änderungen auf der Seite **Adaptive Anwendungssteuerung** vorgenommen haben. Klicken Sie auf **Verwerfen**, falls Sie die Änderungen nicht anwenden möchten.

### <a name="not-recommended-list"></a>Liste für Elemente ohne Empfehlung

Security Center empfiehlt das Anwendungswhitelisting nur für virtuelle Computer, die einen stabilen Satz von Anwendungen ausführen. Wenn sich die Anwendungen auf den zugeordneten virtuellen Computern immer wieder ändern, werden keine Empfehlungen erstellt.

![Empfehlung](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Die Liste enthält Folgendes:
- **NAME**: Der Name des Abonnements und der Gruppe.
- **VMs**: Die Anzahl virtueller Computer in der Gruppe.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Anwendungen, die auf virtuellen Azure-Computern ausgeführt werden, mithilfe der adaptiven Anwendungssteuerung in Azure Security Center einer Whitelist hinzufügen. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Enthält häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
