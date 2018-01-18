---
title: "Anpassen von Sicherheitskonfigurationen für Betriebssysteme in Azure Security Center [Vorschau] | Microsoft-Dokumentation"
description: In diesem Artikel erfahren Sie, wie Sie Security Center-Bewertungen anpassen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 2fa63515d290e6700fbe4a90ae509f4635b19f29
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2018
---
# <a name="customizing-os-security-configurations-in-azure-security-center-preview"></a>Anpassen von Sicherheitskonfigurationen von Betriebssystemen in Azure Security Center [Vorschau]

In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie Bewertungen für Sicherheitskonfigurationen für Betriebssysteme in Azure Security Center anpassen.

## <a name="what-are-os-security-configurations"></a>Was sind Sicherheitskonfigurationen für Betriebssysteme?

Azure Security Center überwacht Sicherheitskonfigurationen anhand von über 150 empfohlenen Regeln für die Härtung des Betriebssystems, einschließlich Regeln für Firewalls, Überwachung, Kennwortrichtlinien und mehr. Wenn festgestellt wird, dass ein Computer eine angreifbare Konfiguration hat, wird eine Sicherheitsempfehlung generiert.

Eine Anpassung der Regeln kann Organisationen helfen zu steuern, welche Konfigurationsoptionen für ihre Umgebung besser geeignet sind. Diese Funktion ermöglicht Benutzern, eine angepasste Bewertungsrichtlinie festzulegen und diese auf alle im Abonnement enthaltenen Computer anzuwenden.

> [!NOTE]
> - Derzeit steht die Anpassung der Sicherheitskonfiguration von Betriebssystemen nur für Windows Server 2008, 2008 R2, 2012 und 2012 R2 zur Verfügung.
- Die Konfiguration gilt für alle VMs und Computer, die mit allen Arbeitsbereichen im gewählten Abonnement verbunden sind.
- Die Anpassung der Sicherheitskonfiguration für Betriebssysteme ist nur im Security Center Security Center-Tarif „Standard“ verfügbar.
>
>

Wie werden die Regeln für die Sicherheitskonfiguration für Betriebssysteme angepasst?

Sie können die Regeln der Sicherheitskonfiguration für Betriebssysteme anpassen, indem Sie eine bestimmte Regel aktivieren und deaktivieren, die gewünschte Einstellung für eine bestehende Regel ändern und eine neue Regel basierend auf den unterstützten Regeltypen (Registrierung, Überwachungsrichtlinie und Sicherheitsrichtlinie) hinzufügen. Derzeit muss die gewünschte Einstellung ein genauer Wert sein.

Neue Regeln müssen das gleiche Format und die gleiche Struktur wie andere bestehende Regeln desselben Typs haben.

> [!NOTE]
> Um Sicherheitskonfigurationen für Betriebssysteme anpassen zu können, muss Ihnen die Rolle „Abonnementbesitzer“, „Mitwirkender“ am Abonnement oder „Sicherheitsadministrator“ zugewiesen sein.
>
>

## <a name="customize-security-configuration"></a>Anpassen der Sicherheitskonfiguration

So passen Sie in Security Center die standardmäßige Sicherheitskonfiguration für Betriebssysteme an

1.  Öffnen Sie das Dashboard **Security Center**.

2.  Klicken Sie im Hauptmenü von Security Center auf **Sicherheitsrichtlinie**.  Der Bereich **Security Center – Sicherheitsrichtlinie** wird geöffnet.

3.  Wählen Sie das Abonnement aus, das Sie anpassen möchten.

    ![](media/security-center-customize-os-security-config/open-security-policy.png)

4. Wählen Sie unter **RICHTLINIENKOMPONENTEN** die Option **Sicherheitskonfigurationen bearbeiten** aus.

6.  Der Bereich **Sicherheitskonfigurationen bearbeiten** wird geöffnet. Befolgen Sie die auf dem Bildschirm markierten Schritte, um die geänderte Datei herunterzuladen, zu bearbeiten und hochzuladen.

    ![](media/security-center-customize-os-security-config/blade.png)

  > [!NOTE]
  > Standardmäßig hat die Konfigurationsdatei, die Sie herunterladen, das *JSON*-Format. Anleitungen zum Ändern dieser Datei finden Sie unter [Anpassen der Konfigurationsdatei](#customize-the-configuration-file).
  >

7. Nach erfolgreichem Speichern der Datei gilt die Konfiguration für alle VMs und Computer, die mit allen Arbeitsbereichen im gewählten Abonnement verbunden sind. Dieser Prozess kann einige Zeit in Anspruch nehmen (in der Regel einige Minuten), kann aber auch länger dauern, da er von der Größe der Infrastruktur abhängt. Klicken Sie auf **Speichern**, um die Änderung zu übernehmen. Andernfalls wird die Richtlinie nicht gespeichert.

    ![](media/security-center-customize-os-security-config/save-successfully.png)

Sie können die aktuelle Richtlinienkonfiguration jederzeit auf den Standardrichtlinienstatus zurücksetzen, indem Sie in **Regeln für Betriebssystem-Sicherheitskonfiguration bearbeiten** die Option **Zurücksetzen** auswählen. Wenn Sie diese Option auswählen, erhalten Sie die folgende Popupwarnung. Klicken Sie auf **Ja**, um zu bestätigen.

![](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Anpassen der Konfigurationsdatei

In der Anpassungsdatei hat jede unterstützte Betriebssystemversion einen Regelsatz. Jeder Regelsatz hat einen eigenen Namen und eine eindeutige ID, wie das folgende Beispiel zeigt:

![](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Diese Datei wurde mit Visual Studio bearbeitet, aber Sie können auch Editor verwenden, solange das JSON-Viewer-Plug-In installiert ist.
>
>

Beim Bearbeiten dieser Datei können Sie eine Regel oder alle Regeln ändern. Jeder Regelsatz enthält den Abschnitt *Rules* mit den Regeln, die in drei Kategorien von Regeln unterteilt sind: Registry, Audit Policy und Security Policy, wie unten gezeigt:

![](media/security-center-customize-os-security-config/rules-section.png)

Jede Kategorie hat einen eigenen Satz von Attributen. Für bestehende Regeln können Sie Änderungen an den folgenden Attributen vornehmen:

- expectedValue: Der Felddatentyp dieses Attributs muss mit den unterstützten Werten jedes Regeltyps übereinstimmen wie z.B.:

  - baselineRegistryRules: Der Wert muss mit dem [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884(v=vs.85) übereinstimmen, der in dieser Regel definiert ist.

  - baselineAuditPolicyRules: Der Wert muss ein Zeichenfolgenwert sein, und zwar einer der folgenden:

    - Erfolg und Fehler

    - Erfolg

  - baselineSecurityPolicyRules: Der Wert muss ein Zeichenfolgenwert sein, und zwar einer der folgenden:

    - „No one“

    - Liste der zulässigen Benutzergruppen, z.B.: „Administrators, Backup Operators“

-   state: Zeichenfolge, die die Optionen „Disabled“ oder „Enabled“ enthalten kann. Bei dieser Private Preview-Version wird für die Zeichenfolge Groß-/Kleinschreibung beachtet.

Dies sind die einzigen Felder, die konfiguriert werden können. Wenn Sie gegen das Dateiformat oder die Dateigröße verstoßen, können Sie die Änderung nicht speichern. Die folgende Fehlermeldung tritt auf, wenn die Datei nicht verarbeitet werden kann:

![](media/security-center-customize-os-security-config/invalid-json.png)

Unter [Fehlercodes](#error-codes) finden Sie eine Liste möglicher Fehler.

Nachfolgend finden Sie Beispiele für diese Regeln und die Attribute (fettgedruckt), die geändert werden können:

**Abschnitt „Rules“:** baselineRegistryRules
```
{

    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "**expectedValue**": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "**state**": "Disabled"

}
```

**Abschnitt „Rules“:** baselineAuditPolicyRules
```
{
"auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
"ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
"originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
"cceId": "CCE-11001-5",
"ruleName": "Audit Policy: Account Management: Other Account Management Events",
"ruleType": "AuditPolicy",
"**expectedValue**": "Success and Failure",
"severity": "Critical",
"analyzeOperation": "Equals",
"source": "Microsoft",
"**state**": "Enabled"
},
```

**Abschnitt „Rules“:** baselineSecurityPolicyRules
```
{
"sectionName": "Privilege Rights",
"settingName": "SeIncreaseWorkingSetPrivilege",
"ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
"originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
"cceId": "CCE-10548-6",
"ruleName": "Increase a process working set",
"ruleType": "SecurityPolicy",
"**expectedValue**": "Administrators, Local Service",
"severity": "Warning",
"analyzeOperation": "Equals",
"source": "Microsoft", "**state**": "Enabled"
},
```

Es gibt einige Regeln, die für die verschiedenen Betriebssystemtypen dupliziert werden. Duplizierte Regeln haben die gleiche „originalId“.

## <a name="adding-a-new-custom-rule"></a>Hinzufügen einer neuen benutzerdefinierten Regel

Sie können auch eine neue Regel erstellen. Bevor Sie eine neue Regel anlegen, beachten Sie die folgenden Einschränkungen:

-   Schemaversion, *baselineId* und *baselineName* können nicht geändert werden.

-   Der Regelsatz kann nicht entfernt werden.

-   Der Regelsatz kann nicht hinzugefügt werden.

-   Die maximale zulässige Anzahl von Regeln (einschließlich Standardregeln) ist 1000.

Neue benutzerdefinierte Regeln werden mit einer neuen benutzerdefinierten Quelle markiert (! = "Microsoft"). Das Feld *RuleId* kann NULL oder leer sein. Wenn es leer ist, wird es von Microsoft generiert. Wenn es nicht leer ist, muss es eine gültige eindeutige GUID für alle (Standard- und benutzerdefinierten) Regeln enthalten. Überprüfen Sie die folgenden Einschränkungen für die Hauptfelder:

-   *originalId*: Kann NULL oder leer sein. Wenn *originalId* nicht leer ist, muss dies eine gültige GUID sein.

-   *cceId*: Kann NULL oder leer sein. Wenn *cceId* ist nicht leer ist, muss der Wert eindeutig sein.

-   *ruleType*: Entweder „Registry“, „AuditPolicy“ oder „SecurityPolicy“.

-   *Severity* Entweder : „Unknown“, „Critical“, „Warning“ oder „Informational“.

-   *analyzeOperation*: Muss „Equals“ sein.

-   *auditPolicyId*: Muss eine gültige GUID sein.

-   *regValueType*: Entweder „Int“, „Long“, „String“ oder „MultipleString“.

> [!NOTE]
> „Hive“ muss *LocalMachine* sein.
>
>

Beispiel einer neuen benutzerdefinierten Regel:

**Registrierung**:
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule”, "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
   }
```
**Sicherheitsrichtlinie**:
```
{

   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Überwachungsrichtlinie**:
```
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Fehler beim Hochladen von Dateien

Wenn die übermittelte Konfigurationsdatei aufgrund von Fehlern bei Werten oder Formatierungen ungültig ist, wird ein Fehler angezeigt. Sie können einen detaillierten Fehlerbericht im CSV-Format herunterladen, um Fehler zu korrigieren, und dann die korrigierte Konfigurationsdatei erneut übermitteln.

![](media/security-center-customize-os-security-config/invalid-configuration.png)

Beispiel für Fehlerdatei:

![](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Fehlercodes

Die nachfolgenden Liste enthält alle möglichen Fehler:

| **Fehler**                                | **Beschreibung**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | Die gefundene Eigenschaft „schemaVersion“ war ungültig oder leer. Der Wert muss auf „{0}“ festgelegt werden.                                                         |
| BaselineInvalidStringError               | Die Eigenschaft „{0}“ darf nicht „\\n“ enthalten.                                                                                                         |
| BaselineNullRuleError                    | Die Liste der Regeln für die Baselinekonfiguration enthält eine Regel mit dem Wert NULL.                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE-ID „{0}“ ist nicht eindeutig.                                                                                                                  |
| BaselineRuleEmptyProperty                | Eigenschaft „{0}“ fehlt oder ist ungültig.                                                                                                       |
| BaselineRuleIdNotInDefault               | Die Regel hat die Quelleigenschaft „Microsoft“, wurde aber nicht im Standardregelsatz von Microsoft gefunden.                                                   |
| BaselineRuleIdNotUniqueError             | Die Regel-ID ist nicht eindeutig.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Die gefundene Eigenschaft „{0}“ ist ungültig. Der Wert ist keine gültige GUID.                                                                             |
| BaselineRuleInvalidHive                  | „Hive“ muss „LocalMachine“ sein.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Der Regelname ist nicht eindeutig.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Die Regel wurde in der neuen Konfiguration nicht gefunden. Regel kann nicht gelöscht werden.                                                                     |
| BaselineRuleNotFoundError                | Die Regel wurde im standardmäßigen Baselineregelsatz nicht gefunden.                                                                                        |
| BaselineRuleNotInPlace                   | Die Regel entspricht einer Standardregel des Typs {0} und ist in der Liste {1} aufgeführt.                                                                       |
| BaselineRulePropertyTooLong              | Die Eigenschaft „{0}“ ist zu lang. Maximal zulässige Länge: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Der erwartete Wert „{0}“ stimmt nicht mit dem definierten Registrierungswerttyp überein.                                                              |
| BaselineRulesetAdded                     | Regelsatz mit der ID „{0}“ wurde in der Standardkonfiguration nicht gefunden. Der Regelsatz kann nicht hinzugefügt werden.                                               |
| BaselineRulesetIdMustBeUnique            | Der angegebene Baselineregelsatz „{0}“ muss eindeutig sein.                                                                                           |
| BaselineRulesetNotFound                  | Regelsatz mit der ID „{0}“ und dem Namen „{1}“ wurde in der angegebenen Konfiguration nicht gefunden. Der Regelsatz kann nicht gelöscht werden.                                |
| BaselineRuleSourceNotMatch               | Regel mit der ID „{0}“ ist bereits definiert.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Der Standardregeltyp ist „{0}“.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Der tatsächliche Typ der Regel ist „{0}“, aber die „RuleType“-Eigenschaft ist „{1}“.                                                                          |
| BaselineRuleUnpermittedChangesError      | Nur die Eigenschaften „expectedValue“ und „state“ dürfen geändert werden.                                                                       |
| BaselineTooManyRules                     | Die maximal zulässige Anzahl angepasster Regeln ist {0}. Die angegebene Konfiguration enthält {1} Regeln. ({2} Standardregeln + {3} angepasste Regeln. |
| ErrorNoConfigurationStatus               | Es wurde kein Konfigurationsstatus gefunden. Geben Sie den gewünschten Konfigurationsstatus an: „Default“ oder „Custom“.                                    |
| ErrorNonEmptyRulesetOnDefault            | Der Konfigurationsstatus ist auf den Standardwert festgelegt. Die Liste „BaselineRulesets“ muss NULL oder leer sein.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Der angegebene Konfigurationstatus ist „Custom“, aber die Eigenschaft „baselineRulesets“ ist NULL oder leer.                                             |
| ErrorParsingBaselineConfig               | Die angegebene Konfiguration ist ungültig. Einer oder mehrere der definierten Werte haben einen NULL-Wert oder ungültigen Typ.                                  |
| ErrorParsingIsDefaultProperty            | Der angegebene „configurationStatus“-Wert „{0}“ ist ungültig. Der Wert kann nur „Default“ oder „Custom“ lauten.                                         |
| InCompatibleViewVersion                  | Ansichtsversion: {0} wird für diesen Arbeitsbereichstyp NICHT unterstützt.                                                                                   |
| InvalidBaselineConfigurationGeneralError | Die angegebene Baselinekonfiguration wies mindestens einen Typvalidierungsfehler auf.                                                          |
| ViewConversionError                      | Die Ansicht ist älter als die Version, die der Arbeitsbereich unterstützt. Fehler bei der Ansichtskonvertierung: {0}                                                                 |

Wenn Ihre Berechtigungen nicht ausreichen, erhalten Sie möglicherweise einen allgemeiner Fehler:

![](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie Bewertungen für Sicherheitskonfigurationen von Betriebssystemen in Azure Security Center anpassen. Weitere Informationen zu den Konfigurationsregeln und ihrer Korrektur finden Sie unter:

- [Security Center: Allgemeine Konfigurationsbezeichner und Baselineregeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Security Center verwendet die Common Configuration Enumeration (CCE), um Konfigurationsregeln eindeutige Bezeichner zuzuweisen. Auf der Website zu [CCE](https://nvd.nist.gov/config/cce/index) finden Sie weitere Informationen.
- Unter [Korrigieren von Sicherheitskonfigurationen](security-center-remediate-os-vulnerabilities.md) erfahren Sie, wie Sie Sicherheitsrisiken beheben können, wenn Ihre Betriebssystemkonfiguration nicht den empfohlenen Sicherheitskonfigurationsregeln entspricht.
