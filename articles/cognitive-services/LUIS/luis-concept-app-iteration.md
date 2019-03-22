---
title: Iterative app design
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS learns best in an iterative cycle of model changes, utterance examples, publishing, and gathering data from endpoint queries.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: 67bcb33727bc808f5e5bea701daffc77dde736ff
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337480"
---
# <a name="authoring-cycle-for-your-luis-app"></a>Authoring cycle for your LUIS app
LUIS learns best in an iterative cycle of model changes, utterance examples, publishing, and gathering data from endpoint queries. 

![Authoring cycle](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Building a LUIS model
The model's purpose is to figure out what the user is asking for (the intention or intent) and what parts of the question provide details (entities) that help determine the answer. 

The model needs to be specific to the app domain in order to determine words and phrases that are relevant as well as typical word ordering. 

The model includes intent, entities. 

## <a name="add-training-examples"></a>Add training examples
LUIS needs example utterances in the intents. The examples need enough variation of word choice and word order to be able to determine which intent the utterance is meant for. Each example utterance needs to have any required data labeled as entities. 

You instruct LUIS to ignore utterances that are not relevant to your app's domain by assigning the utterance to the **None** intent. Any words or phrases you do not need pulled out of an utterance do not need to be labeled. There is no label for words or phrases to ignore. 

## <a name="train-and-publish-the-app"></a>Train and publish the app
Once you have 10 to 15 different utterances in each intent, with the required entities labeled, train and publish. From the publish success notification, use the link to get your endpoints. Make sure to create your app and publish your app so that it is available in the [endpoint regions](luis-reference-regions.md) you need. 

## <a name="https-endpoint-testing"></a>HTTPS endpoint testing
You can test your LUIS app from the HTTPS endpoint. Testing from the endpoint allows LUIS to choose any utterances with low-confidence for review.  

## <a name="recycle"></a>Recycle
When you are done with a cycle of authoring, you can begin again. Start with reviewing endpoint utterances LUIS marked with low-confidence. Check these utterances for both intent and entity. Once you review utterances, the review list should be empty.  

## <a name="batch-testing"></a>Batch testing
Batch testing is a way to see how many example utterances are scored by LUIS. The examples should be new to LUIS and should be correctly labeled with intent and entities you want LUIS to find. The test results indicate how well LUIS would perform on that set of utterances. 

## <a name="next-steps"></a>Next steps

Learn concepts about [collaboration](luis-concept-collaborator.md).
