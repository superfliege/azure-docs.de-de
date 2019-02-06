---
title: 'Tutorial:  Einrichten von PHS als Sicherung für AD FS in Azure AD Connect | Microsoft-Dokumentation'
description: In diesem Artikel wird veranschaulicht, wie Sie die Kennworthashsynchronisierung als Sicherung für AD FS aktivieren.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 01/30/2019
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 7e7c9ca30659fd1e99989bb77406b653a8ed9e7f
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302979"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Tutorial:  Einrichten der Kennworthashsynchronisierung als Sicherung für AD FS in Azure AD Connect

Im folgenden Tutorial werden Sie durch die Einrichtung der Kennworthashsynchronisierung als Sicherung und Failover für AD FS geleitet.  In diesem Dokument wird außerdem gezeigt, wie Sie die Kennworthashsynchronisierung als primäre Authentifizierungsmethode aktivieren, falls AD FS ausgefallen oder nicht verfügbar ist.

## <a name="prerequisites"></a>Voraussetzungen
Dieses Tutorial baut auf dem [Tutorial: Erstellen eines Verbunds einer Umgebung mit einer AD-Gesamtstruktur mit der Cloud](tutorial-federation.md) auf und ist Voraussetzung für die Bearbeitung des vorliegenden Tutorials.  Sofern noch nicht geschehen, bearbeiten Sie zunächst das genannte Tutorial, bevor Sie das vorliegende Dokument bearbeiten.

## <a name="enable-phs-in-azure-ad-connect"></a>Aktivieren der Kennworthashsynchronisierung in Azure AD Connect
Nachdem eine Azure AD Connect-Umgebung mit Verbund eingerichtet wurde, besteht der erste Schritt darin, die Kennworthashsynchronisierung zu aktivieren und Azure AD Connect das Synchronisieren der Hashes zu gestatten.

Gehen Sie wie folgt vor:

1.  Doppelklicken Sie auf das Azure AD Connect-Symbol, das auf dem Desktop erstellt wurde.
2.  Klicken Sie auf **Konfigurieren**.
3.  Wählen Sie auf der Seite „Weitere Aufgaben“ die Option **Synchronisierungsoptionen anpassen** aus, und klicken Sie auf **Weiter**.
4.  Geben Sie den Benutzernamen und das Kennwort für den globalen Administrator ein.  Sie haben dieses Konto [hier](tutorial-federation.md#create-a-global-administrator-in-azure-ad) im vorherigen Tutorial erstellt.
5.  Klicken Sie auf dem Bildschirm **Verzeichnisse verbinden** auf **Weiter**.
6.  Klicken Sie auf dem Bildschirm **Filtern von Domänen und Organisationseinheiten** auf **Weiter**.
7.  Wählen Sie auf dem Bildschirm **Optionale Features** die Option **Kennworthashsynchronisierung** aus, und klicken Sie auf **Weiter**.
![Auswählen](media/tutorial-phs-backup/backup1.png)</br>
8.  Klicken Sie auf dem Bildschirm **Bereit zur Konfiguration** auf **Konfigurieren**.
9.  Klicken Sie nach Abschluss der Konfiguration auf **Beenden**.
10. Das ist alles!  Fertig.  Die Kennworthashsynchronisierung wird jetzt durchgeführt und kann als Sicherung verwendet werden, falls AD FS nicht verfügbar ist.

## <a name="switch-to-password-hash-synchronization"></a>Umstellung auf die Kennworthashsynchronisierung
Nachfolgend wird veranschaulicht, wie Sie auf die Kennworthashsynchronisierung umstellen können, falls dies erforderlich ist.  Gehen Sie wie folgt vor:

1. Doppelklicken Sie auf das Azure AD Connect-Symbol, das auf dem Desktop erstellt wurde.
2.  Klicken Sie auf **Konfigurieren**.
3.  Wählen Sie **Benutzeranmeldung ändern**, und klicken Sie auf **Weiter**.
![Änderung](media/tutorial-phs-backup/backup2.png)</br>
4.  Geben Sie den Benutzernamen und das Kennwort für den globalen Administrator ein.  Sie haben dieses Konto [hier](tutorial-federation.md#create-a-global-administrator-in-azure-ad) im vorherigen Tutorial erstellt.
5.  Wählen Sie auf dem Bildschirm **Benutzeranmeldung** die Option **Kennworthashsynchronisierung** aus, und setzen Sie ein Häkchen im Feld **Benutzerkonten nicht konvertieren**.  
6.  Lassen Sie die Einstellung **Einmaliges Anmelden aktivieren** aktiviert, und klicken Sie auf **Weiter**.
7.  Klicken Sie auf dem Bildschirm **Einmaliges Anmelden aktivieren** auf **Weiter**.
8.  Klicken Sie auf dem Bildschirm **Bereit zur Konfiguration** auf **Konfigurieren**.
9.  Klicken Sie nach Abschluss der Konfiguration auf **Beenden**.
10. Benutzer können jetzt ihre Kennwörter verwenden, um sich bei Azure und Azure-Diensten anzumelden.

## <a name="test-signing-in-with-one-of-our-users"></a>Testanmeldung mit einem unserer Benutzer

1.  Rufen Sie [http://myapps.microsoft.com](http://myapps.microsoft.com) auf.
2. Melden Sie sich mit einem Benutzerkonto an, das in unserem neuen Mandanten erstellt wurde.  Sie müssen sich mit folgendem Format anmelden: (user@domain.onmicrosoft.com). Verwenden Sie dasselbe Kennwort, mit dem sich der Benutzer lokal anmeldet.</br>
![Überprüfen](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>Nächste Schritte


- [Voraussetzungen für Azure AD Connect](how-to-connect-install-prerequisites.md) 
- [Express-Einstellungen](how-to-connect-install-express.md)
- [Implementieren der Kennworthashsynchronisierung mit Azure AD Connect](how-to-connect-password-hash-synchronization.md)|
