---
title: Tools für die Verwendung von Ansible mit Azure
description: Installieren und Verwenden individueller Tools für Ansible mit Azure
ms.service: ansible
keywords: Ansible, Azure, DevOps, Tools, VS Code, Visual Studio Code, Erweiterung
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/14/2018
ms.topic: article
ms.openlocfilehash: 73d31054bb0c40dd08bf4b4a93c31c59354b5bed
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="tools-for-using-ansible-with-azure"></a>Tools für die Verwendung von Ansible mit Azure

Die folgenden Tools ermöglichen Ihnen ein einfacheres und effizienteres Arbeiten mit Ansible und Azure.

## <a name="visual-studio-code-extension-for-ansible"></a>Visual Studio Code-Erweiterung für Ansible

Die [Visual Studio Code-Erweiterung für Ansible](https://marketplace.visualstudio.com/items?itemName=vscoss.vscode-ansible) bietet verschiedene Funktionen für die Verwendung von Ansible über Visual Studio-Code:

- Ansible-Anweisungen, Module und Plug-Ins aus der Ansible-Dokumentation werden während der Eingabe vervollständigt.
- Codeausschnitte werden angezeigt, wenn Sie während der Erstellung von Ansible-Playbooks &lt;STRG>&lt;LEERTSTE> betätigen.
- Die Syntaxhervorhebung zeigt Ihren Ansible-Playbookcode der YAML-Syntax entsprechend in unterschiedlichen Farben und Schriftarten an.
- Ansible-Playbooks können über das Visual Studio Code-Terminalfenster ausgeführt werden.
    - (Nur Windows) Ansible kann über einen Docker-Container ausgeführt werden.
    - (Linux und macOS) Ansible kann über einen Docker-Container oder über eine lokale Ansible-Installation ausgeführt werden. 
- Ansible Playbooks können über Azure Cloud Shell ausgeführt werden.