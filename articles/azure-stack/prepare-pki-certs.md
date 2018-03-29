---
title: Vorbereiten von PKI-Zertifikaten für Azure Stack für die Bereitstellung von in Azure Stack integrierten Systemen | Microsoft-Dokumentation
description: Beschreibt die Vorgehensweise zum Vorbereiten von Azure Stack-PKI-Zertifikaten für in Azure Stack integrierte Systeme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: dadb443f8b7739e3a18c0d3beb558d8c42e9d19c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Vorbereiten von Azure Stack-PKI-Zertifikaten für die Bereitstellung
Die [von der Zertifizierungsstelle Ihrer Wahl bezogenen](azure-stack-get-pki-certs.md) Zertifikatdateien müssen mit Eigenschaften importiert und exportiert werden, die mit den Zertifikatanforderungen von Azure Stack in Einklang stehen.


## <a name="prepare-certificates-for-deployment"></a>Vorbereiten von Zertifikaten für die Bereitstellung
Gehen Sie wie folgt vor, um die Azure Stack-PKI-Zertifikate vorzubereiten und zu überprüfen: 

1.  Kopieren Sie die ursprünglichen Zertifikatsversionen, [die Sie von der Zertifizierungsstelle Ihrer Wahl bezogen haben](azure-stack-get-pki-certs.md), in ein Verzeichnis auf dem Bereitstellungshost. 
  > [!WARNING]
  > Kopieren Sie keine Dateien, die bereits importiert/exportiert wurden oder aus einem anderen Grund nicht mehr den Dateien entsprechen, die direkt von der Zertifizierungsstelle bereitgestellt wurden.

2.  Importieren Sie die Zertifikate in den Zertifikatspeicher des lokalen Computers:

    a.  Klicken Sie mit der rechten Maustaste auf das Zertifikat, und klicken Sie anschließend auf **PFX installieren**.

    b.  Wählen Sie im **Zertifikatimport-Assistenten** als Importspeicherort die Option **Lokaler Computer** aus. Klicken Sie auf **Weiter**.

    ![Importspeicherort „Lokaler Computer“](.\media\prepare-pki-certs\1.png)

    c.  Klicken Sie auf der Seite **Zu importierende Datei auswählen** auf **Weiter**.

    d.  Geben Sie auf der Seite **Schutz für den privaten Schlüssel** das Kennwort für Ihre Zertifikatdateien ein, und aktivieren Sie die Option **Schlüssel als exportierbar markieren. Dadurch können Sie Ihre Schlüssel später sichern oder transportieren**. Klicken Sie auf **Weiter**.

    ![Markieren des Schlüssels als exportierbar](.\media\prepare-pki-certs\2.png)

    e.  Wählen Sie **Alle Zertifikate in folgendem Speicher speichern** und anschließend **Enterprise Trust** als Speicherort aus. Klicken Sie auf **OK**, um das Dialogfeld für die Zertifikatspeicherauswahl zu schließen, und klicken Sie anschließend auf **Weiter**.

    ![Konfigurieren des Zertifikatspeichers](.\media\prepare-pki-certs\3.png)

  f.    Klicken Sie auf **Fertig stellen**, um den Zertifikatimport-Assistenten abzuschließen.

  g.    Wiederholen Sie den Vorgang für alle Zertifikate, die Sie für Ihre Bereitstellung bereitstellen.

3. Exportieren Sie das Zertifikat in das PFX-Dateiformat mit den Azure Stack-Anforderungen:

  a.    Öffnen Sie die MMC-Konsole des Zertifikat-Managers, und stellen Sie eine Verbindung mit dem Zertifikatspeicher des lokalen Computers her.

  b.    Wechseln Sie zum Verzeichnis **Enterprise Trust**.

  c.    Wählen Sie eines der Zertifikate aus, die Sie weiter oben in Schritt 2 importiert haben.

  d.    Klicken Sie auf der Taskleiste der Zertifikat-Manager-Konsole auf **Aktionen** > **Alle Aufgaben** > **Exportieren**.

  e.    Klicken Sie auf **Weiter**.

  f.    Wählen Sie **Ja, privaten Schlüssel exportieren** aus, und klicken Sie dann auf **Weiter**.

  g.    Wählen Sie im Abschnitt für das Exportdateiformat **Alle erweiterten Eigenschaften exportieren** aus, und klicken Sie dann auf **Weiter**.

  h.    Klicken Sie auf **Kennwort**, und geben Sie ein Kennwort für die Zertifikate an. Merken Sie sich dieses Kennwort. Es wird als Bereitstellungsparameter verwendet. Klicken Sie auf **Weiter**.

  i.    Wählen Sie einen Dateinamen und einen Speicherort für die zu exportierende PFX-Datei. Klicken Sie auf **Weiter**.

  j.    Wählen Sie **Fertig stellen** aus.

  k.    Wiederholen Sie diesen Vorgang für alle Zertifikate, die Sie weiter oben in Schritt 2 für Ihre Bereitstellung importiert haben.

## <a name="next-steps"></a>Nächste Schritte
[Überprüfen von Azure Stack-PKI-Zertifikaten](validate-pki-certs.md)