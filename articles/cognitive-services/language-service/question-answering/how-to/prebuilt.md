---
title: Prebuilt API - question answering
titleSuffix: Azure Cognitive Services
description: Use the question answering Prebuilt API to ask and receive answers to questions without having to create a project/knowledge base. 
ms.service: cognitive-services
ms.subservice: language-service
author: mrbullwinkle
ms.author: mbullwin
ms.topic: conceptual
ms.date: 11/03/2021
---

# Prebuilt API

The question answering **prebuilt API** provides you the capability to answer questions based on a passage of text without having to create projects/knowledge bases, maintain question and answer pairs, or incurring costs for underutilized infrastructure. This functionality is provided as an API and can be used to meet question and answering needs without having to learn the details about question answering.

Given a user query and a block of text/passage the API will return an answer and precise answer (if available).

## Example API usage

Imagine that you have one or more blocks of text from which you would like to get answers for a given question. Normally you would have had to create as many sources as the number of blocks of text. However, now with the prebuilt API you can query the blocks of text without having to define content sources in a project/knowledge base.

Some other scenarios where this API can be used are:

* You are developing an ebook reader app for end users, which allows them to highlight text, enter a question and find answers over a highlighted passage of text.
* A browser extension that allows users to ask a question over the content being currently displayed on the browser page.
* A health bot that takes queries from users and provides answers based on the medical content that the bot identifies as most relevant to the user query.

Below is an example of a sample request:

## Sample request

```
POST https://{Unique-to-your-endpoint}.api.cognitive.microsoft.com/language/:query-text
```

### Sample query over a single block of text

Request Body

```json
{
  "parameters": {
    "Endpoint": "{Endpoint}",
    "Ocp-Apim-Subscription-Key": "{API key}",
    "Content-Type": "application/json",
    "api-version": "2021-10-01",
    "stringIndexType": "TextElements_v8",
    "textQueryOptions": {
      "question": "how long it takes to charge surface?",
      "records": [
        {
          "id": "1",
          "text": "Power and charging. It takes two to four hours to charge the Surface Pro 4 battery fully from an empty state. It can take longer if you’re using your Surface for power-intensive activities like gaming or video streaming while you’re charging it."
        },
        {
          "id": "2",
          "text": "You can use the USB port on your Surface Pro 4 power supply to charge other devices, like a phone, while your Surface charges. The USB port on the power supply is only for charging, not for data transfer. If you want to use a USB device, plug it into the USB port on your Surface."
        }
      ],
      "language": "en"
    }
  }
}
```

## Sample response

In the above request body, we query over a single block of text. A sample response received for the above query is shown below,

```json
{
"responses": {
    "200": {
      "headers": {},
      "body": {
        "answers": [
          {
            "answer": "Power and charging. It takes two to four hours to charge the Surface Pro 4 battery fully from an empty state. It can take longer if you’re using your Surface for power-intensive activities like gaming or video streaming while you’re charging it.",
            "confidenceScore": 0.93,
            "id": "1",
            "answerSpan": {
              "text": "two to four hours",
              "confidenceScore": 0,
              "offset": 28,
              "length": 45
            },
            "offset": 0,
            "length": 224
          },
          {
            "answer": "It takes two to four hours to charge the Surface Pro 4 battery fully from an empty state. It can take longer if you’re using your Surface for power-intensive activities like gaming or video streaming while you’re charging it.",
            "confidenceScore": 0.92,
            "id": "1",
            "answerSpan": {
              "text": "two to four hours",
              "confidenceScore": 0,
              "offset": 8,
              "length": 25
            },
            "offset": 20,
            "length": 224
          },
          {
            "answer": "It can take longer if you’re using your Surface for power-intensive activities like gaming or video streaming while you’re charging it.",
            "confidenceScore": 0.05,
            "id": "1",
            "answerSpan": null,
            "offset": 110,
            "length": 244
          }
        ]
      }
    }
  }
```

We see that multiple answers are received as part of the API response. Each answer has a specific confidence score that helps understand the overall relevance of the answer. Answer span represents whether a potential short answer was also detected. Users can make use of this confidence score to determine which answers to provide in response to the query.

## Prebuilt API limits

If you need to use larger documents than the limit allows, you can break the text into smaller chunks of text before sending them to the API. In this context, a document is a defined single string of text characters.

These numbers represent the **per individual API call limits**:

* Number of documents: 5.
* Maximum size of a single document: 5,120 characters.
* Maximum three responses per document.

## Prebuilt API reference

Visit the [full prebuilt API samples](https://github.com/Azure/azure-rest-api-specs/blob/main/specification/cognitiveservices/data-plane/Language/stable/2021-10-01/examples/questionanswering/SuccessfulQueryText.json) documentation to understand the input and output parameters required for calling the API.