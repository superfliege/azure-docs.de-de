---
title: 'Glossar: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Glossar
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: b22ec27b2999d322945e37c5a38d2b1d1532e7e3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166043"
---
# <a name="glossary"></a>Glossar

## <a name="qna-maker-service"></a>QnA Maker Service
Ein QnA Maker-Dienst ist eine Voraussetzung für die Verwendung von QnA Maker. Durch Erwerb eines QnA Maker-Tarifs werden Ressourcen in Ihrem Azure-Abonnement für das Erstellen und Verwalten Ihrer Wissensdatenbank eingerichtet. Jedes QnA Maker-Benutzerkonto kann mehrere QnA Maker-Dienste in seinem Azure-Abonnement erstellen.

## <a name="knowledge-base"></a>Wissensdatenbank
Eine Wissensdatenbank ist das Repository mit Fragen und Antworten, die über QnA Maker erstellt, verwaltet und verwendet werden. Jeder QnA Maker-Tarif kann für mehrere Wissensdatenbanken verwendet werden.

## <a name="endpoint"></a>Endpunkt
Ein REST-basierter HTTP-Endpunkt zur Bereitstellung der Inhalte Ihrer Wissensdatenbank, die in Ihre Anwendung integriert werden können, häufig in Form eines Chatbots. 

## <a name="test-knowledge-base"></a>Testversion der Wissensdatenbank
Eine Wissensdatenbank verfügt über zwei Statusarten: „Test“ und „Veröffentlicht“. Die Testversion der Wissensdatenbank ist die Version, die gerade bearbeitet, gespeichert und auf Genauigkeit und Vollständigkeit der Antworten getestet wird. Änderungen an der Testversion der Wissensdatenbank wirken sich nicht auf den Endbenutzer Ihrer Anwendung bzw. Ihres Chatbots aus.

## <a name="published-knowledge-base"></a>Veröffentlichte Wissensdatenbank
Eine Wissensdatenbank verfügt über zwei Statusarten: „Test“ und „Veröffentlicht“.  Die veröffentlichte Wissensdatenbank ist die Version, die in Ihrem Chatbot bzw. Ihrer Anwendung verwendet wird. Bei der Aktion zur Veröffentlichung einer Wissensdatenbank wird der Inhalt der Testversion in die veröffentlichte Version der Wissensdatenbank übertragen. Da es sich bei der veröffentlichten Wissensdatenbank um die Version handelt, die von der Anwendung über den Endpunkt verwendet wird, sollte sichergestellt sein, dass der Inhalt korrekt und ausreichend getestet ist.

## <a name="query"></a>Abfragen
Eine Benutzerabfrage ist die Frage, die der Endbenutzer oder Tester an die Wissensdatenbank richtet. Die Abfrage wird häufig in einem natürlichen Sprachformat oder mit einigen Schlüsselwörtern formuliert, die die Frage darstellen.

## <a name="response"></a>response
Hierbei handelt es sich um die Antwort, die aus der Wissensdatenbank basierend auf der bestmöglichen Übereinstimmung für eine bestimmte Benutzerabfrage abgerufen wird.

## <a name="confidence-score"></a>Zuverlässigkeitsbewertung
Die Zuverlässigkeitsbewertung der Antwort ist ein numerischer Wert zwischen 0 und 100. Dabei gibt 100 an, dass eine genaue Übereinstimmung zwischen Benutzerabfrage und einer Frage in der Wissensdatenbank vorliegt, die bereitgestellte Antwort richtig ist und es sich um eine geeignete Antwort für eine bestimmte Benutzerabfrage handelt. Antworten weisen in der Regel eine Rangfolge basierend auf der Zuverlässigkeitsbewertung auf, wobei die Antwort mit der höchsten Zuverlässigkeitsbewertung als Standardantwort verwendet wird.
