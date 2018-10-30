---
title: Testen von Terraform-Modulen in Azure mit Terratest
description: Es wird beschrieben, wie Sie Terratest zum Testen Ihrer Terraform-Module verwenden.
services: terraform
ms.service: terraform
keywords: Terraform, DevOps, Speicherkonto, Azure, Terratest, Komponententest, Integrationstest
author: JunyiYi
manager: jeconnoc
ms.author: junyi
ms.topic: tutorial
ms.date: 10/19/2018
ms.openlocfilehash: 7feee063c7b311934f7d157a9dff62d803a041b0
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638706"
---
# <a name="test-terraform-modules-in-azure-using-terratest"></a>Testen von Terraform-Modulen in Azure mit Terratest

Terraform-Module werden verwendet, um wiederverwendbare, zusammensetzbare und testbare Komponenten zu erstellen. Hiermit wird die Kapselung in den Bereich „Infrastruktur als Code“ eingebunden.

Wie bei anderen Softwarekomponenten auch, spielt die Qualitätssicherung bei Terraform-Modulen eine wichtige Rolle. Leider gibt es nicht viel Dokumentation dazu, wie Komponententests und Integrationstests in Terraform-Modulen erstellt werden. Dieses Tutorial enthält eine Einführung in eine Testinfrastruktur und bewährte Methoden, die wir beim Erstellen unserer [Azure Terraform-Module](https://registry.terraform.io/browse?provider=azurerm) genutzt haben.

Nach Auswertung aller gängigen Testinfrastrukturen haben wir uns für [Terratest](https://github.com/gruntwork-io/terratest) entschieden. Terratest wird als Go-Bibliothek implementiert. Sie umfasst eine Sammlung mit Hilfsfunktionen und Mustern für häufige Aufgaben bei Infrastrukturtests, z.B. Senden von HTTP-Anforderungen und Erstellen einer SSH-Verbindung mit einem bestimmten virtuellen Computer. Hier sind einige wichtige Vorteile von Terratest angegeben:

- **Es werden komfortable Hilfsfunktionen zum Überprüfen der Infrastruktur bereitgestellt.** Dieses Feature ist nützlich, wenn Sie Ihre reale Infrastruktur in der realen Umgebung überprüfen möchten.
- **Die Ordnerstruktur ist eindeutig organisiert.** Ihre Testfälle sind klar strukturiert und basieren auf der [Standardordnerstruktur für Terraform-Module](https://www.terraform.io/docs/modules/create.html#standard-module-structure).
- **Alle Testfälle werden in Go geschrieben.** Da die meisten Terraform-Entwickler bereits Go-Entwickler sind, ist es bei Verwendung von Terratest normalerweise nicht erforderlich, noch eine weitere Programmiersprache zu erlernen. Außerdem sind die beiden einzigen Abhängigkeiten, die zum Durchführen von Testfällen in Terratest erforderlich sind, Go und Terraform.
- **Diese Infrastruktur ist stark erweiterbar.** Es ist nicht schwierig, zusätzlich zu Terratest weitere Funktionen bereitzustellen, z.B. Azure-spezifische Features.

## <a name="prerequisites"></a>Voraussetzungen

Diese praktische Anleitung ist plattformunabhängig. Die Ausführung ist unter Windows, Linux oder macOS möglich. Installieren Sie die folgende Software, bevor Sie fortfahren:

- **Go-Programmiersprache**: Die Terraform-Testfälle sind in [Go](https://golang.org/dl/) geschrieben.
- **dep**: [dep](https://github.com/golang/dep#installation) ist ein Tool zum Verwalten von Abhängigkeiten für Go.
- **Azure CLI**: Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ist ein Befehlszeilentool zum Verwalten von Azure-Ressourcen. (Terraform unterstützt die Authentifizierung für Azure über einen Dienstprinzipal oder [die Azure CLI](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html).)
- **mage**: Wir verwenden die [ausführbare mage-Datei](https://github.com/magefile/mage/releases), um zu beschreiben, wie Sie die Ausführung Ihrer Terratest-Testfälle vereinfachen. 

## <a name="create-a-static-webpage-module"></a>Erstellen eines statischen Webseitenmoduls

In diesem Tutorial erstellen Sie ein Terraform-Modul, mit dem eine statische Webseite bereitgestellt wird, indem eine einzelne HTML-Datei in ein Azure-Speicherblob hochgeladen wird. Mit diesem Modul können Benutzer weltweit über eine URL auf diese Webseite zugreifen, die vom Modul zurückgegeben wird.

> [!NOTE]
> Alle in diesem Abschnitt beschriebenen Dateien sollten unter [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH) erstellt werden.

Erstellen Sie zunächst unter dem Ordner `src` Ihres GoPath einen neuen Ordner mit dem Namen `staticwebpage`. Unten ist die gesamte Ordnerstruktur dieses Tutorials dargestellt. (In diesem Abschnitt liegt der Schwerpunkt auf den Dateien, die mit einem Sternchen `(*)` gekennzeichnet sind.)

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

Das statische Webseitenmodul akzeptiert drei Eingaben, die in `./variables.tf` deklariert werden:

```hcl
variable "location" {
  description = "The Azure region in which all resources will be created."
}

variable "website_name" {
  description = "The website name which will be used to create a bunch of related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static homepage HTML in your local filesystem."
  default     = "index.html"
}
```

Wie bereits erwähnt, gibt dieses Modul auch eine URL aus, die in `./outputs.tf` deklariert wird:

```hcl
output "homepage_url" {
  value = "${azurerm_storage_blob.homepage.url}"
}
```

Hiermit kommen wir zur Hauptlogik dieses Moduls. Insgesamt werden vier Ressourcen bereitgestellt:
- Eine Ressourcengruppe, deren Name die `website_name`-Eingabe mit dem angefügten Element `-staging-rg` ist.
- Ein Speicherkonto, dessen Name die `website_name`-Eingabe mit dem angefügten Element `data001` ist. Um die Namenskonvention für Speicherkonten zu erfüllen, entfernt das Modul aber alle Sonderzeichen und konvertiert den Namen in Kleinbuchstaben.
- Ein fest benannter Container `wwwroot`, der im obigen Speicherkonto erstellt wird.
- Eine einzelne HTML-Datei, die aus der `html_path`-Eingabe ausgelesen und in `wwwroot/index.html` hochgeladen wird.

Die Modullogik der statischen Webseite wird in `./main.tf` implementiert:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = "${var.location}"
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = "${azurerm_resource_group.main.name}"
  location                 = "${azurerm_resource_group.main.location}"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = "${azurerm_resource_group.main.name}"
  storage_account_name  = "${azurerm_storage_account.main.name}"
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = "${azurerm_resource_group.main.name}"
  storage_account_name   = "${azurerm_storage_account.main.name}"
  storage_container_name = "${azurerm_storage_container.main.name}"
  source                 = "${var.html_path}"
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Komponententest

Terratest ist ein Tool, das für Integrationstests ausgelegt ist. Dies bedeutet, dass reale Ressourcen in einer realen Umgebung bereitgestellt werden. In einigen Fällen können Aufträge dieser Art eine außergewöhnliche Größe erreichen. Dies gilt besonders, wenn eine sehr hohe Zahl von Ressourcen bereitgestellt wird. Die Speicherkontologik für die Konvertierung von Namen, die im vorherigen Abschnitt beschrieben wurde, ist ein gutes Beispiel: Wir müssen nicht tatsächlich Ressourcen bereitstellen, sondern möchten nur sicherstellen, dass die Logik für die Konvertierung von Namen korrekt ist.

Aufgrund der Flexibilität von Terratest kann dies durch den Einsatz von Komponententests leicht erreicht werden. Komponententests sind lokal ausgeführte Testfälle (Internetzugriff ist aber trotzdem erforderlich). Es werden einfach die Befehle `terraform init` und `terraform plan` ausgeführt, und die Komponententestfälle analysieren die Ausgabe von `terraform plan` und suchen nach den zu vergleichenden Attributwerten.

Im restlichen Teil dieses Abschnitts wird beschrieben, wie Sie Terratest verwenden, um einen Komponententest zu implementieren, mit dem die Korrektheit der Speicherkontologik für die Konvertierung von Namen sichergestellt wird. Für uns sind nur die Dateien von Interesse, die mit einem Sternchen `(*)` gekennzeichnet sind.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Zuerst ist die leere HTML-Datei `./test/fixtures/storage-account-name/empty.html` nur ein Platzhalter.

Die Datei `./test/fixtures/storage-account-name/main.tf` ist das Gerüst für den Testfall. Sie akzeptiert nur die Eingabe von `website_name`, und dies ist auch die Eingabe der Komponententests. Die Logik ist hier angegeben:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = "${var.website_name}"
  html_path    = "empty.html"
}
```

Die Hauptkomponente ist schließlich die Implementierung der Komponententests: `./test/storage_account_name_unit_test.go`.

Wenn Sie ein Go-Entwickler sind, werden Sie erkennen, dass dies mit der Signatur einer klassischen Go-Testfunktion übereinstimmt, indem ein Argument vom Typ `*testing.T` akzeptiert wird.

Im Text des Komponententests sind in der Variablen `testCases` fünf Fälle definiert (Schlüssel als Eingabe und Wert als erwartete Ausgabe). Für jeden Komponententestfall führen wir zuerst `terraform init` mit dem Testfixtureordner (`./test/fixtures/storage-account-name/`) als Ziel aus. 

Anschließend wird mit dem Befehl `terraform plan` mit einer spezifischen Testfalleingabe (siehe `website_name`-Definition in `tfOptions`) das Ergebnis in `./test/fixtures/storage-account-name/terraform.tfplan` gespeichert (nicht in der Gesamtordnerstruktur aufgeführt).

Als Nächstes wird diese Ergebnisdatei in eine für den Code lesbare Struktur analysiert, indem der offizielle Terraform-Planparser verwendet wird.

Wir suchen jetzt nach den Attributen, die für uns interessant sind (hier der `name` von `azurerm_storage_account`), und vergleichen sie mit der erwarteten Ausgabe.

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions.Vars, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Sie müssen in der Befehlszeile die folgenden Schritte ausführen, um die Komponententests durchzuführen.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestUT_StorageAccountName
```

Nach ungefähr einer Minute wird das herkömmliche Go-Testergebnis angezeigt.

### <a name="integration-test"></a>Integrationstest

Im Gegensatz zu Komponententests sind Integrationstests erforderlich, um für Ressourcen eine End-to-End-Bereitstellung in einer realen Umgebung durchzuführen. Terratest ist für diese Aufgaben gut geeignet. Da in den bewährten Methoden für das Terraform-Modul auch empfohlen wird, dass der Ordner `examples` einige End-to-End-Beispiele enthält, bietet es sich an, diese Beispiele für Integrationstests zu verwenden. In diesem Abschnitt liegt der Schwerpunkt auf drei Dateien, die jeweils mit einem Sternchen `(*)` gekennzeichnet sind.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Zunächst beginnen wir mit den Beispielen. Im Ordner `./examples/` wird ein neuer Beispielordner mit dem Namen `hello-world/` erstellt. Hier stellen wir die einfache HTML-Seite `./examples/hello-world/index.html` für den Upload bereit:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demostrate Terratest.</p>
</body>
</html>
```

Das Terraform-Beispiel `./examples/hello-world/main.tf` ähnelt dem Beispiel aus dem Komponententest, aber es gibt einen großen Unterschied: Die URL der hochgeladenen HTML-Seite mit dem Namen `homepage` wird ebenfalls ausgegeben.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = "${var.website_name}"
}

output "homepage" {
  value = "${module.staticwebpage.homepage_url}"
}
```

Terratest und die klassische Go-Testfunktion wird wieder in der Integrationstestdatei `./test/hello_world_example_test.go` angezeigt.

Im Gegensatz zu Komponententests werden bei Integrationstests echte Ressourcen in Azure erstellt, sodass Sie darauf achten müssen, dass Namenskonflikte vermieden werden. (Achten Sie besonders auf einige global eindeutige Namen, z.B. den Speicherkontonamen.) Der erste Schritt der Testlogik ist daher das Generieren eines zufälligen `websiteName`-Elements, indem die von Terratest bereitgestellte Funktion `UniqueId()` verwendet wird. Diese Funktion generiert einen zufälligen Namen mit Kleinbuchstaben, Großbuchstaben oder Zahlen. `tfOptions` bewirkt, dass alle Terraform-Befehle auf den Ordner `./examples/hello-world/` ausgerichtet sind, und es wird auch sichergestellt, dass `website_name` auf den zufälligen `websiteName` festgelegt wird.

Anschließend werden `terraform init`, `terraform apply` und `terraform output` einzeln nacheinander ausgeführt. Wir haben eine weitere Hilfsfunktion `HttpGetWithCustomValidation()` von Terratest genutzt, um sicherzustellen, dass die HTML-Daten für die ausgegebene `homepage`-URL hochgeladen werden, die von `terraform output` zurückgegeben wurde. Hierzu wird der HTTP GET-Statuscode mit `200` verglichen, und im HTML-Inhalt wird nach einigen Schlüsselwörtern gesucht. Abschließend wird die Ausführung von `terraform destroy` sichergestellt, indem das `defer`-Feature von Go genutzt wird.

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demostrate Terratest.")
    })
}
```

Sie müssen in der Befehlszeile die folgenden Schritte ausführen, um die Integrationstests durchzuführen.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestIT_HelloWorldExample
```

Nach ungefähr zwei Minuten wird das herkömmliche Go-Testergebnis angezeigt. Sie können natürlich auch sowohl Komponententests als auch Integrationstests durchführen, indem Sie Folgendes ausführen:

```shell
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ go test
```

Wie Sie gesehen haben, können Integrationstests deutlich länger als Komponententests dauern (zwei Minuten für einen Integrationstest, während für fünf Komponententests nur eine Minute benötigt wird). Es ist aber Ihre Entscheidung, wann Sie Komponententests nutzen und wann Sie Integrationstests einsetzen. Normalerweise bevorzugen wir die Verwendung von Komponententests für komplexe Logik mit Terraform-HCL-Funktionen, während Integrationstests eher aus End-to-End-Sicht eines Benutzers genutzt werden.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Verwenden von mage zum Vereinfachen der Ausführung von Terratest-Testfällen 

Wie Sie gesehen haben, ist die Ausführung von Testfällen in einer Shell keine einfache Aufgabe, weil Sie zu unterschiedlichen Verzeichnissen navigieren und verschiedene Befehle ausführen müssen. Aus diesem Grund führen wir das Buildsystem in unserem Projekt ein. In diesem Abschnitt verwenden wir das Go-Buildsystem mage für diese Aufgabe.

Für mage ist lediglich die Datei `magefile.go` im Stammverzeichnis Ihres Projekts erforderlich (in der folgenden Abbildung mit `(+)` gekennzeichnet).

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Hier ist ein Beispiel für `./magefile.go` angegeben. In diesem Buildskript, das in Go geschrieben ist, haben wir fünf Buildschritte implementiert:
- `Clean`: In diesem Schritt werden alle generierten/temporären Dateien während der Testausführungen entfernt.
- `Format`: In diesem Schritt werden `terraform fmt` und `go fmt` ausgeführt, um Ihre Codebasis zu formatieren.
- `Unit`: In diesem Schritt werden alle Komponententests (mit der Funktionsbenennungskonvention `TestUT_*`) unter dem Ordner `./test/` ausgeführt.
- `Integration`: Ähnelt `Unit`, aber anstelle von Komponententests werden Integrationstests (`TestIT_*`) durchgeführt.
- `Full`: In diesem Schritt werden `Clean`, `Format`, `Unit', and `Integration` als Sequenz ausgeführt.

```go
// +build mage

// Build script to format and run tests of a Terraform module project.
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// Default target when execute `mage` in shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build/test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Ähnlich wie bei den zuvor ausgeführten Schritten auch, können Sie die folgenden Befehle verwenden, um eine vollständige Testsuite auszuführen:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in magefile or test cases
GoPath/src/staticwebpage$ go fmt      # Only requied when you change the magefile
GoPath/src/staticwebpage$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage$ mage
```

Sie können die letzte Befehlszeile durch beliebige mage-Schritte ersetzen, z.B. `mage unit` oder `mage clean`. Nun denken Sie vielleicht, dass immer noch viele Befehlszeilen vorhanden sind und es eine gute Idee ist, `dep`-Befehle sowie `az login` in die mage-Datei einzubetten. Diesen Code geben wir hier aber nicht an. Ein weiterer Schritt bei der Verwendung von mage besteht darin, dass die Schritte mit dem Go-Paketsystem gemeinsam genutzt werden können. mage-Dateien über Ihre gesamten Module hinweg können vereinfacht werden, indem einfach auf eine allgemeine Implementierung verwiesen wird und Abhängigkeiten (`mg.Deps()`) deklariert werden.

> [!NOTE]
> **Option: Festlegen von Dienstprinzipal-Umgebungsvariablen zum Durchführen von Akzeptanztests**
> 
> Anstatt `az login` vor Tests auszuführen, können Sie die Azure-Authentifizierung auch erzielen, indem Sie die Dienstprinzipal-Umgebungsvariablen festlegen. Terraform veröffentlicht [eine Liste mit Namen von Umgebungsvariablen](https://www.terraform.io/docs/providers/azurerm/index.html#testing). (Nur die ersten vier dieser Umgebungsvariablen sind erforderlich.) Terraform veröffentlicht auch ausführliche Anleitungen, in denen beschrieben wird, wie Sie [die Werte dieser Umgebungsvariablen beschaffen können](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Terratest finden Sie auf der [entsprechenden GitHub-Seite](https://github.com/gruntwork-io/terratest). Einige nützliche Informationen zu mage finden Sie ggf. auf der [GitHub-Seite](https://github.com/magefile/mage) und der [Website](https://magefile.org/).
