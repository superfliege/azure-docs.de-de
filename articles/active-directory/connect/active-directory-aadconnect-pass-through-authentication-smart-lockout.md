---
title: 'Azure AD Connect: Passthrough-Authentifizierung - Smart Lockout | Microsoft-Dokumentation'
description: "In diesem Artikel wird beschrieben, wie die Azure Active Directory-Passthrough-Authentifizierung (Azure AD) Ihre lokalen Konten vor Brute-Force-Angriffen auf Ihre Kennwörter in der Cloud schützt."
services: active-directory
keywords: "Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: billmath
ms.openlocfilehash: fc46fe1d68538757ba5a8c5aa1acb4b51f8a171b
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Azure Active Directory-Passthrough-Authentifizierung: Smart Lockout

## <a name="overview"></a>Übersicht

Active Directory (Azure AD) bietet Schutz gegen Brute-Force-Angriffe auf Kennwörter und verhindert, dass echte Benutzer aus ihrer Office 365-Anwendung oder aus SaaS-Anwendungen ausgeschlossen werden. Diese Funktion namens *Smart Lockout* wird unterstützt, wenn die Passthrough-Authentifizierung als Anmeldemethode verwendet wird. Smart Lockout ist standardmäßig für alle Mandanten aktiviert und schützt Ihre Benutzerkonten durchgängig.

Smart Lockout verfolgt fehlgeschlagene Anmeldeversuche nach. Nach einem bestimmten *Sperrschwellenwert* lässt Smart Lockout eine *Sperrdauer* wirksam werden. Smart Lockout weist während der Sperrdauer jeden Anmeldeversuch des Angreifers zurück. Wird der Angriff fortgesetzt, bewirken weitere gescheiterte Anmeldeversuche nach Ablauf der Sperrdauer längere Sperrdauern.

>[!NOTE]
>Der Standardsperrschwellenwert beträgt 10 fehlgeschlagene Anmeldeversuche. Die Standardsperrdauer beträgt 60 Sekunden.

Smart Lockout unterscheidet auch zwischen Anmeldeversuchen echter Benutzer und Anmeldeversuchen von Angreifern. In den meisten Fällen sperrt es nur die Angreifer. Diese Funktionalität verhindert, dass die echten Benutzer vorsätzlich von den Angreifern ausgesperrt werden. Um zwischen echten Benutzern und Angreifern zu unterscheiden, verwendet Smart Lockout das vergangene Anmeldeverhalten, die Geräte und Browser der Benutzer sowie sonstige Indikatoren. Die Algorithmen werden ständig verbessert.

Passthrough-Authentifizierung leitet Kennwortvalidierungsanforderungen an Ihr lokales Active Directory (AD) weiter. Daher müssen Sie Angreifer daran hindern, die Active Directory-Konten Ihrer Benutzer sperren zu können. Active Directory hat eigene Kontosperrungsrichtlinien, insbesondere die Richtlinien [Kontensperrungsschwelle](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) und [Zurücksetzungsdauer des Kontosperrungszählers](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx). Konfigurieren Sie den Azure AD-Sperrschwellenwert und die Azure AD-Sperrdauer entsprechend, um Angriffe in der Cloud herausfiltern, bevor sie das lokale Active Directory erreichen.

>[!NOTE]
>>Die Smart Lookout-Funktion ist kostenlos und ist standardmäßig für alle Kunden _aktiviert_. Zum Modifizieren der Werte der Kontensperrungsschwelle und der Sperrungsdauer von Azure AD mit der Graph-API muss Ihr Mandant für Azure AD Premium P2 aktiviert sein. 

Um sicherzustellen, dass die lokalen Active Directory-Konten Ihrer Benutzer gut geschützt sind, müssen Sie folgende Voraussetzungen gewährleisten:

   * Der Azure AD-Sperrschwellenwert ist _kleiner_ als der Schwellenwert für eine Active Directory-Kontosperrung. Legen Sie die Werte so fest, dass der Schwellenwert für eine Active Directory-Kontosperrung mindestens das Zwei- oder Dreifache des Azure AD-Sperrschwellenwerts beträgt.
   * Die Azure AD-Sperrdauer (ausgedrückt in Sekunden) ist _länger_ als die Active Directory-Zurücksetzungsdauer des Kontosperrungszählers (ausgedrückt in Minuten).

>[!IMPORTANT]
>Derzeit können die Cloudkonten der Benutzer nicht von einem Administrator entsperrt werden, wenn sie von Smart Lockout gesperrt wurden. Der Administrator muss warten, bis die Sperrdauer abgelaufen ist.

## <a name="verify-your-active-directory-account-lockout-policies"></a>Überprüfen Ihrer Active Directory-Kontosperrungsrichtlinien

Gehen Sie folgendermaßen vor, um Ihre Active Directory-Kontosperrungsrichtlinien zu überprüfen:

1.  Öffnen Sie das Gruppenrichtlinienverwaltungstool.
2.  Bearbeiten Sie die Gruppenrichtlinie, die auf alle Benutzer angewendet wird, beispielsweise die **Standarddomänenrichtlinie**.
3.  Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Kontorichtlinien** > **Kontosperrungsrichtlinien**.
4.  Überprüfen Sie Ihre Werte für **Kontensperrungsschwelle** und **Zurücksetzungsdauer des Kontosperrungszählers**.

![Active Directory-Kontosperrungsrichtlinien](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-requires-a-premium-license"></a>Verwenden der Graph-API, um die Smart Lockout-Werte Ihres Mandanten zu verwalten (erfordert eine Premium-Lizenz)

>[!IMPORTANT]
>Das Verändern der Werte für die Azure AD-Sperrschwelle und die Azure AD-Sperrdauer durch Verwenden der Graph-API ist eine Azure AD Premium P2-Funktion. Sie müssen dafür auch ein globaler Administrator Ihres Mandanten sein.

Sie können [Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) verwenden, um die Werte von Azure AD Smart Lockout zu lesen, festzulegen und zu aktualisieren. Sie können diese Vorgänge auch programmgesteuert ausführen.

### <a name="view-smart-lockout-values"></a>Anzeigen der Smart Lockout-Werte

Um die Smart Lockout-Werte Ihres Mandanten anzuzeigen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei Graph-Tester als globaler Administrator Ihres Mandanten an. Wenn Sie dazu aufgefordert werden, gewähren Sie Zugriff für die angeforderten Berechtigungen.
2. Wählen Sie **Berechtigungen ändern** aus, und wählen Sie dann die Berechtigung **Directory.ReadWrite.All** aus.
3. Konfigurieren Sie die Graph-API-Anforderung wie folgt: Legen Sie die Version auf **BETA**, den Anforderungstyp auf **GET** und die URL auf `https://graph.microsoft.com/beta/<your-tenant-domain>/settings` fest.
4. Wählen Sie **Abfrage ausführen**, um die Smart Lockout-Werte Ihres Mandanten anzuzeigen. Wenn Sie die Werte Ihres Mandanten zuvor nicht festgelegt haben, sehen Sie einen leeren Bereich.

### <a name="set-smart-lockout-values"></a>Festlegen der Smart Lockout-Werte

Um die Smart Lockout-Werte Ihres Mandanten festzulegen, führen Sie die folgenden Schritte aus (nur beim ersten Mal erforderlich):

1. Melden Sie sich bei Graph-Tester als globaler Administrator Ihres Mandanten an. Wenn Sie dazu aufgefordert werden, gewähren Sie Zugriff für die angeforderten Berechtigungen.
2. Wählen Sie **Berechtigungen ändern** aus, und wählen Sie dann die Berechtigung **Directory.ReadWrite.All** aus.
3. Konfigurieren Sie die Graph-API-Anforderung wie folgt: Legen Sie die Version auf **BETA**, den Anforderungstyp auf **POST** und die URL auf `https://graph.microsoft.com/beta/<your-tenant-domain>/settings` fest.
4. Kopieren Sie die folgende JSON-Anforderung in das Feld **Anforderungstext**.
5. Wählen Sie **Abfrage ausführen** aus, um die Smart Lockout-Werte Ihres Mandanten festzulegen.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Falls Sie diese nicht verwenden, können Sie die Werte für **BannedPasswordList** und **EnableBannedPasswordCheck** leer (**""**) bzw. gleich **false** belassen.

Überprüfen Sie, ob Sie die Smart Lockout-Werte Ihres Mandanten korrekt festgelegt haben. Führen Sie dazu die Schritte in [Anzeigen der Smart Lockout-Werte](#view-smart-lockout-values) aus.

### <a name="update-smart-lockout-values"></a>Aktualisieren der Smart Lockout-Werte

Um die Smart Lockout-Werte Ihres Mandanten zu aktualisieren (sofern Sie diese zuvor festgelegt haben), führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei Graph-Tester als globaler Administrator Ihres Mandanten an. Wenn Sie dazu aufgefordert werden, gewähren Sie Zugriff für die angeforderten Berechtigungen.
2. Wählen Sie **Berechtigungen ändern** aus, und wählen Sie dann die Berechtigung **Directory.ReadWrite.All** aus.
3. [Um die Smart Lockout-Werte Ihres Mandanten anzuzeigen, führen Sie die folgenden Schritte aus](#view-smart-lockout-values). Kopieren Sie den `id`-Wert (eine GUID) des Elements mit **displayName** als **PasswordRuleSettings**.
4. Konfigurieren Sie die Graph-API-Anforderung wie folgt: Legen Sie die Version auf **BETA**, den Anforderungstyp auf **PATCH** und die URL auf `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` fest. Verwenden Sie die GUID aus Schritt 3 für `<id>`.
5. Kopieren Sie die folgende JSON-Anforderung in das Feld **Anforderungstext**. Ändern Sie die Smart Lockout-Werte geeignet.
6. Wählen Sie **Abfrage ausführen**, um die Smart Lockout-Werte Ihres Mandanten zu aktualisieren.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Überprüfen Sie, ob Sie die Smart Lockout-Werte Ihres Mandanten korrekt aktualisiert haben. Führen Sie dazu die Schritte in [Anzeigen der Smart Lockout-Werte](#view-smart-lockout-values) aus.

## <a name="next-steps"></a>Nächste Schritte
[UserVoice:](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Anfordern neuer Features über das Azure Active Directory-Forum
