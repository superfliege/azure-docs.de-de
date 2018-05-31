---
title: Erweiterung zum Konfigurieren des gewünschten Zustands mit Azure Resource Manager-Vorlagen
description: Erfahren Sie mehr über die Resource Manager-Vorlagendefinition für die Erweiterung zum Konfigurieren des gewünschten Zustands (Desired State Configuration, DSC) in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: DSC
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: d19d33d7d8d19923e814b8685c3e56553f85850d
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012477"
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Erweiterung zum Konfigurieren des gewünschten Zustands mit Azure Resource Manager-Vorlagen

Dieser Artikel beschreibt die Azure Resource Manager-Vorlage für den [Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands](dsc-overview.md) (Desired State Configuration, DSC).

> [!NOTE]
> Möglicherweise werden Ihnen andere Schemabeispiele angezeigt. Das Schema wurde im Release von Oktober 2016 geändert. Weitere Informationen finden Sie im Abschnitt zum [Update vom vorherigen Format](#update-from-the-previous-format).

## <a name="template-example-for-a-windows-vm"></a>Vorlagenbeispiel für einen virtuellen Windows-Computer

Der folgende Codeausschnitt wird in den **Ressourcenabschnitt** der Vorlage eingefügt.
Die DSC-Erweiterung erbt die Standardwerte für die Erweiterungseigenschaften.
Weitere Informationen finden Sie unter [VirtualMachineExtension Class](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.) (VirtualMachineExtension-Klasse).

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
    "apiVersion": "2017-12-01",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.76",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "protectedSettings": {
                "Items": {
                    "registrationKeyPrivate": "registrationKey"
                }
            },
            "publicSettings": {
                "configurationArguments": [{
                        "Name": "RegistrationKey",
                        "Value": {
                            "UserName": "PLACEHOLDER_DONOTUSE",
                            "Password": "PrivateSettingsRef:registrationKeyPrivate"
                        }
                    },
                    {
                        "RegistrationUrl": "registrationUrl",
                    },
                    {
                        "NodeConfigurationName": "nodeConfigurationName"
                    }
                ]
            }
        }
    }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Vorlagenbeispiel für Skalierungsgruppen virtueller Windows-Computer

Ein Knoten einer VM-Skalierungsgruppe weist den Abschnitt **properties** mit dem Attribut **VirtualMachineProfile, extensionProfile** auf.
Fügen Sie unter **Erweiterungen** die Details für die DSC-Erweiterung hinzu.

Die DSC-Erweiterung erbt die Standardwerte für die Erweiterungseigenschaften.
Weitere Informationen finden Sie unter [VirtualMachineScaleSetExtension Class](/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet) (VirtualMachineScaleSetExtension-Klasse).

```json
"extensionProfile": {
    "extensions": [{
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
        "apiVersion": "2017-12-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.Powershell",
            "type": "DSC",
            "typeHandlerVersion": "2.76",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "protectedSettings": {
                    "Items": {
                        "registrationKeyPrivate": "registrationKey"
                    }
                },
                "publicSettings": {
                    "configurationArguments": [{
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        {
                            "RegistrationUrl": "registrationUrl",
                        },
                        {
                            "NodeConfigurationName": "nodeConfigurationName"
                        }
                    ]
                }
            },
        }
    }]
}
```

## <a name="detailed-settings-information"></a>Ausführliche Einstellungsinformationen

Verwenden Sie das folgende Schema im Bereich **Einstellungen** der Azure DSC-Erweiterung in einer Resource Manager-Vorlage.

Eine Liste mit den verfügbaren Argumenten für das Standardkonfigurationsskript finden Sie unter [Standardkonfigurationsskript](#default-configuration-script).

```json
"settings": {
    "wmfVersion": "latest",
    "configuration": {
        "url": "http://validURLToConfigLocation",
        "script": "ConfigurationScript.ps1",
        "function": "ConfigurationFunction"
    },
    "configurationArguments": {
        "argument1": "Value1",
        "argument2": "Value2"
    },
    "configurationData": {
        "url": "https://foo.psd1"
    },
    "privacy": {
        "dataCollection": "enable"
    },
    "advancedOptions": {
        "downloadMappings": {
            "customWmfLocation": "http://myWMFlocation"
        }
    }
},
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        },
        "parameterOfTypePSCredential2": {
            "userName": "UsernameValue2",
            "password": "PasswordValue2"
        }
    },
    "configurationUrlSasToken": "?g!bber1sht0k3n",
    "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}
```

## <a name="details"></a>Details

| Eigenschaftenname | Typ | BESCHREIBUNG |
| --- | --- | --- |
| settings.wmfVersion |Zeichenfolge |Gibt die Version von Windows Management Framework (WMF) an, die auf Ihrem virtuellen Computer installiert sein muss. Wenn diese Eigenschaft auf **neueste** festgelegt ist, wird die aktuelle Version von WMF installiert. Die einzigen derzeit möglichen Werte für diese Eigenschaft sind **4.0**, **5.0**, **5.0PP** und **neueste**. Diese möglichen Werte werden gelegentlich aktualisiert. Der Standardwert ist **neueste**. |
| settings.configuration.url |Zeichenfolge |Gibt den URL-Speicherort an, von dem die ZIP-Datei Ihrer DSC-Konfiguration herunterzuladen ist. Wenn die bereitgestellte URL ein SAS-Token für den Zugriff erfordert, müssen Sie die Eigenschaft **protectedSettings.configurationUrlSasToken** auf den Wert Ihres SAS-Tokens festlegen. Diese Eigenschaft ist erforderlich, wenn **settings.configuration.script** oder **settings.configuration.function** definiert sind. Wenn für diese Eigenschaften kein Wert angegeben ist, ruft die Erweiterung das Standardkonfigurationsskript auf, um Metadaten von Location Configuration Manager (LCM) festzulegen, und es müssen Argumente angegeben werden. |
| settings.configuration.script |Zeichenfolge |Gibt den Dateinamen des Skripts an, das die Definition Ihrer DSC-Konfiguration enthält. Dieses Skript muss sich im Stammverzeichnis der ZIP-Datei befinden, die von der durch die Eigenschaft **configuration.url** angegebenen URL heruntergeladen wurde. Diese Eigenschaft ist erforderlich, wenn **settings.configuration.url** oder **settings.configuration.script** definiert sind. Wenn für diese Eigenschaften kein Wert angegeben ist, ruft die Erweiterung das Standardkonfigurationsskript auf, um LCM-Metadaten festzulegen, und es müssen Argumente angegeben werden. |
| settings.configuration.function |Zeichenfolge |Gibt den Namen Ihrer DSC-Konfiguration an. Die Konfiguration mit diesem Namen muss im Skript enthalten sein, das durch **configuration.script** definiert ist. Diese Eigenschaft ist erforderlich, wenn **settings.configuration.url** oder **settings.configuration.function** definiert sind. Wenn für diese Eigenschaften kein Wert angegeben ist, ruft die Erweiterung das Standardkonfigurationsskript auf, um LCM-Metadaten festzulegen, und es müssen Argumente angegeben werden. |
| settings.configurationArguments |Sammlung |Definiert beliebige Parameter, die Sie Ihrer DSC-Konfiguration übergeben möchten. Diese Eigenschaft ist nicht verschlüsselt. |
| settings.configurationData.url |Zeichenfolge |Gibt die URL an, unter der die Datei mit Ihren Konfigurationsdaten (PSD1) heruntergeladen werden kann, um sie als Eingabe für Ihre DSC-Konfiguration zu nutzen. Wenn die bereitgestellte URL ein SAS-Token für den Zugriff erfordert, müssen Sie die Eigenschaft **protectedSettings.configurationDataUrlSasToken** auf den Wert Ihres SAS-Tokens festlegen. |
| settings.privacy.dataEnabled |Zeichenfolge |Aktiviert bzw. deaktiviert die Erfassung von Telemetriedaten. Die einzig möglichen Werte für diese Eigenschaft sind **Enable**, **Disable**, **''** oder **$null**. Wird die Eigenschaft leer gelassen oder „null“ angegeben, ist die Telemetrie aktiviert. Der Standardwert ist **''**. Weitere Informationen finden Sie unter [Azure DSC extension data collection](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) (Datensammlung mit der Azure DSC-Erweiterung). |
| settings.advancedOptions.downloadMappings |Sammlung |Definiert alternative Speicherorte zum Herunterladen von WMF. Weitere Informationen finden Sie unter [Azure DSC Extension 2.8 & How to map downloads of the extension dependencies to your own location](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) (Azure DSC-Erweiterung 2.8 und Zuweisen von Downloads der Erweiterungsabhängigkeiten an Ihren eigenen Speicherort). |
| protectedSettings.configurationArguments |Sammlung |Definiert beliebige Parameter, die Sie Ihrer DSC-Konfiguration übergeben möchten. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationUrlSasToken |Zeichenfolge |Gibt das SAS-Token für den Zugriff auf die durch **configuration.url** definierte URL an. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationDataUrlSasToken |Zeichenfolge |Gibt das SAS-Token für den Zugriff auf die durch **configurationData.url** definierte URL an. Diese Eigenschaft ist verschlüsselt. |

## <a name="default-configuration-script"></a>Standardkonfigurationsskript

Weitere Informationen zu den folgenden Werten finden Sie im Abschnitt [Grundlegende Einstellungen](/powershell/dsc/metaconfig#basic-settings) des Artikels zum lokalen Konfigurations-Manager.
Sie können das Konfigurationsskript für die DSC-Erweiterung standardmäßig so konfigurieren, dass Sie nur die LCM-Eigenschaften in der folgenden Tabelle verwenden.

| Eigenschaftenname | Typ | BESCHREIBUNG |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |securestring |Erforderliche Eigenschaft. Gibt den Schlüssel, der für die Registrierung eines Knotens beim Azure Automation-Dienst verwendet wird, als Kennwort eines PowerShell-Anmeldeinformationsobjekts an. Dieser Wert kann durch Verwendung der **listkeys**-Methode für das Automation-Konto automatisch ermittelt werden. Der Wert sollte als geschützte Einstellung gesichert werden. |
| settings.configurationArguments.RegistrationUrl |Zeichenfolge |Erforderliche Eigenschaft. Gibt die URL des Automation-Endpunkts an, an dem der Knoten die Registrierung durchführt. Dieser Wert kann durch Verwendung der **reference**-Methode für das Automation-Konto automatisch ermittelt werden. |
| settings.configurationArguments.NodeConfigurationName |Zeichenfolge |Erforderliche Eigenschaft. Gibt die Knotenkonfiguration im Automation-Konto an, die dem Knoten zugewiesen werden soll. |
| settings.configurationArguments.ConfigurationMode |Zeichenfolge |Gibt den Modus für LCM an. Gültige Optionen: **ApplyOnly**, **ApplyandMonitor** und **ApplyandAutoCorrect**.  Standardwert: **ApplyAndMonitor**. |
| settings.configurationArguments.RefreshFrequencyMins | UInt32 | Gibt an, wie oft der lokale Konfigurations-Manager das Automation-Konto auf Updates überprüft.  Standardwert: **30**.  Mindestwert: **15**. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | UInt32 | Gibt an, wie oft der LCM die aktuelle Konfiguration überprüft. Standardwert: **15**. Mindestwert: **15**. |
| settings.configurationArguments.RebootNodeIfNeeded | boolean | Gibt an, ob ein Knoten automatisch neu gestartet werden kann, wenn dies durch einen DSC-Vorgang angefordert wird. Der Standardwert ist **false**. |
| settings.configurationArguments.ActionAfterReboot | Zeichenfolge | Gibt an, was nach einem Neustart geschieht, wenn eine Konfiguration angewendet wird. Gültige Optionen: **ContinueConfiguration** und **StopConfiguration**. Standardwert: **ContinueConfiguration**. |
| settings.configurationArguments.AllowModuleOverwrite | boolean | Gibt an, ob der LCM vorhandene Module auf dem Knoten überschreibt. Der Standardwert ist **false**. |

## <a name="settings-vs-protectedsettings"></a>„Settings“ im Vergleich zu „ProtectedSettings“

Alle Einstellungen werden auf dem virtuellen Computer in einer Einstellungstextdatei gespeichert.
Eigenschaften unter **Einstellungen** sind öffentliche Eigenschaften.
Öffentliche Eigenschaften werden nicht in der Einstellungstextdatei verschlüsselt.
Eigenschaften unter **protectedSettings** sind mit einem Zertifikat verschlüsselt und werden in der Einstellungsdatei auf dem virtuellen Computer nicht als Nur-Text angezeigt.

Wenn die Konfiguration Anmeldeinformationen erfordert, können diese in **protectedSettings** enthalten sein:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example-configuration-script"></a>Beispiel für Konfigurationsskript

Das folgende Beispiel zeigt das Standardverhalten für die DSC-Erweiterung. Dabei werden Metadateneinstellungen für den LCM angegeben, und eine Registrierung beim Azure Automation DSC-Dienst erfolgt.
Konfigurationsargumente sind erforderlich.
Konfigurationsargumente werden an das Standardkonfigurationsskript übergeben, um LCM-Metadaten festzulegen.

```json
"settings": {
    "configurationArguments": {
        {
            "Name": "RegistrationKey",
            "Value": {
                "UserName": "PLACEHOLDER_DONOTUSE",
                "Password": "PrivateSettingsRef:registrationKeyPrivate"
            },
        },
        "RegistrationUrl": "[parameters('registrationUrl1')]",
        "NodeConfigurationName": "nodeConfigurationNameValue1"
    }
},
"protectedSettings": {
    "Items": {
        "registrationKeyPrivate": "[parameters('registrationKey1']"
    }
}
```

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Beispiel für die Verwendung des Konfigurationsskripts in Azure Storage

Das folgende Beispiel stammt aus dem Abschnitt [DSC Erweiterung: Handler-Übersicht](dsc-overview.md).
In diesem Beispiel werden Resourcen Manager-Vorlagen anstelle von Cmdlets zum Bereitstellen der Erweiterung verwendet.
Speichern Sie die Konfiguration „IisInstall.ps1“, fügen Sie sie einer ZIP-Datei hinzu, und laden Sie die Datei unter einer zugänglichen URL hoch.
In diesem Beispiel wird Azure Blob Storage verwendet. Sie können ZIP-Dateien jedoch von beliebigen Speicherorten herunterladen.

In der Resource Manager-Vorlage weist der folgende Code den virtuellen Computer an, die richtige Datei herunterzuladen und die entsprechende PowerShell-Funktion auszuführen:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="update-from-a-previous-format"></a>Aktualisieren vom vorherigen Format

Alle Einstellungen im vorherigen Format der Eigenschaft (das die öffentlichen Eigenschaften **ModulesUrl**, **ConfigurationFunction**, **SasToken** oder **Properties** enthält) werden automatisch an das aktuelle Format angepasst.
Sie werden wie gehabt ausgeführt.

Das frühere Einstellungsschema sah folgendermaßen aus:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties": {
        "ParameterToConfigurationFunction1": "Value1",
        "ParameterToConfigurationFunction2": "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1"
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": {
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

So wird das frühere Format an das aktuelle Format angepasst:

| Eigenschaftenname | Entsprechung im früheren Schema |
| --- | --- |
| settings.wmfVersion |settings.wmfVersion |
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |Erster Teil von „settings.ConfigurationFunction“ (vor „\\\\“) |
| settings.configuration.function |Zweiter Teil von „settings.ConfigurationFunction“ (nach „\\\\“) |
| settings.configurationArguments |settings.Properties |
| settings.configurationData.url |protectedSettings.DataBlobUri (ohne SAS-Token) |
| settings.privacy.dataEnabled |settings.privacy.dataEnabled |
| settings.advancedOptions.downloadMappings |settings.advancedOptions.downloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS-Token aus „protectedSettings.DataBlobUri“ |

## <a name="troubleshooting---error-code-1100"></a>Problembehandlung – Fehlercode 1100

Fehlercode 1100 gibt an, dass ein Problem mit der Benutzereingabe in die DSC-Erweiterung vorliegt.
Der Text dieser Fehler variiert und kann sich ändern.
Hier finden Sie einige Fehler, die auftreten können, und die entsprechenden Behebungen.

### <a name="invalid-values"></a>Ungültige Werte

„Privacy.dataCollection ist '{0}'.
Die einzig möglichen Werte sind '', 'Aktivieren' und 'Deaktivieren'.“
„WmfVersion ist '{0}'.
Einzig mögliche Werte sind ... und 'neueste'.“

**Problem**: Ein bereitgestellter Wert ist nicht zulässig.

**Lösung**: Ändern Sie den ungültigen Wert in einen gültigen Wert.
Ausführlichere Informationen finden Sie in der Tabelle unter [Details](#details).

### <a name="invalid-url"></a>Ungültige URL

„ConfigurationData.url ist '{0}'. Dies ist keine gültige URL“ „DataBlobUri ist '{0}'. Dies ist keine gültige URL“ „Configuration.url ist '{0}'. Dies ist keine gültige URL“

**Problem**: Eine bereitgestellte URL ist ungültig.

**Lösung**: Überprüfen Sie alle angegebenen URLs.
Stellen Sie sicher, dass alle URLs in gültige Speicherorte aufgelöst werden, auf die die Erweiterung auf dem Remotecomputer zugreifen kann.

### <a name="invalid-configurationargument-type"></a>Ungültiger ConfigurationArgument-Typ

„Ungültiger ConfigurationArguments-Typ {0}“

**Problem**: Die Eigenschaft *ConfigurationArguments* kann nicht in ein **Hashtabellenobjekt** aufgelöst werden.

**Lösung**: Stellen Sie sicher, dass Ihre Eigenschaft *ConfigurationArguments* eine **Hashtabelle** ist.
Nutzen Sie das Format im vorherigen Beispiel. Achten Sie auf Anführungszeichen, Kommas und Klammern.

### <a name="duplicate-configurationarguments"></a>„ConfigurationArguments“ doppelt vorhanden

„Doppelte Argumente '{0}' sowohl in öffentlichen als auch geschützten configurationArguments-Elementen gefunden“

**Problem**: *ConfigurationArguments* in öffentlichen Einstellungen und *ConfigurationArguments* in geschützten Einstellungen enthalten Eigenschaften mit dem gleichen Namen.

**Lösung**: Entfernen Sie eine der doppelten Eigenschaften.

### <a name="missing-properties"></a>Fehlende Eigenschaften

„Configuration.function erfordert die Angabe von configuration.url oder configuration.module“

„Configuration.url erfordert die Angabe von configuration.script“

„Configuration.script erfordert die Angabe von configuration.url“

„Configuration.url erfordert die Angabe von configuration.function“

„ConfigurationUrlSasToken erfordert die Angabe von configuration.url“

„ConfigurationDataUrlSasToken erfordert die Angabe von configurationData.url“

**Problem**: Eine definierte Eigenschaft benötigt eine andere Eigenschaft, die nicht vorhanden ist.

**Lösungen**:

- Geben Sie die fehlende Eigenschaft an.
- Entfernen Sie die Eigenschaft, die die fehlende Eigenschaft benötigt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Verwenden von VM-Skalierungsgruppen mit der Azure DSC-Erweiterung](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).
- Weitere Informationen finden Sie unter [Sichere Verwaltung von Anmeldeinformationen durch DSC](dsc-credentials.md).
- Lesen Sie die [Einführung in den Azure DSC-Erweiterungshandler](dsc-overview.md).
- Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](/powershell/dsc/overview).
