---
title: 'Idiomas admitidos: Bing Web Search API'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas naturales, países y regiones admitidos por Bing News Search API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 09/25/2018
ms.author: aahi
ms.openlocfilehash: 8f10812fc77a36bc29c8926bb7f76bceed9d7718
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857692"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Compatibilidad de idiomas y regiones para Bing Web Search API

Bing Web Search API admite más de 36 países o regiones, muchos de ellos con varios idiomas. Cuando se realiza una consulta, puede resultar útil especificar un país o una región para refinar los resultados de búsqueda en función de los intereses de ese país o región. Los resultados pueden contener vínculos a Bing, y estos vínculos pueden localizar la experiencia del usuario de Bing con arreglo a la región o el país especificados o al idioma.

Puede especificar un país o región utilizando el parámetro de consulta `cc`. Cuando se especifica un país o región, debe especificarse también uno o varios códigos de idioma con el [encabezado `Accept-Language`](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers). Utilice la [tabla de Mercados](#Markets) para ver una lista de los idiomas que se admiten en cada mercado.

Si lo desea, también puede especificar el mercado con el parámetro de consulta `mkt` y un código de la tabla de **Mercados**. Cuando se especifica un mercado, también se indica un país o región y un idioma preferido. Puede establecer explícitamente el idioma con el parámetro de consulta `setLang`.

## <a name="countriesregions"></a>Países o regiones

|País/región|Código|
|-------|----|
|Argentina|AR|
|Australia|AU|
|Austria|AT|
|Bélgica|BE|
|Brasil|BR|
|Canadá|CA|
|Chile|CL|
|Dinamarca|DK|
|Finlandia|FI|
|Francia|VF|
|Alemania|DE|
|RAE de Hong Kong|HK|
|India|IN|
|Indonesia|ID|
|Italia|IT|
|Japón|JP|
|Corea|KR|
|Malasia|MY|
|México|MX|
|Países Bajos|NL|
|Nueva Zelanda|NZ|
|Noruega|NO|
|China|CN|
|Polonia|PL|
|Portugal|PT|
|Filipinas|PH|
|Rusia|RU|
|Arabia Saudí|SA|
|Sudáfrica|ZA|
|España|ES|
|Suecia|SE|
|Suiza|CH|
|Taiwán|TW|
|Turquía|TR|
|Reino Unido|GB|
|Estados Unidos|US|

## <a name="markets"></a>Mercados

|País/región|Idioma|Código de mercado|
|-------|--------|-----------|
|Argentina|Español|es-AR|
|Australia|English|en-AU|
|Austria|Alemán|de-AT|
|Bélgica|Neerlandés|nl-BE|
|Bélgica|Francés|fr-BE|
|Brasil|Portugués|pt-BR|
|Canadá|English|en-CA|
|Canadá|Francés|fr-CA|
|Chile|Español|es-CL|
|Dinamarca|Danés|da-DK|
|Finlandia|Finés|fi-FI|
|Francia|Francés|fr-FR|
|Alemania|Alemán|de-DE|
|RAE de Hong Kong|Chino tradicional|zh-HK|
|India|English|en-IN|
|Indonesia|English|en-ID|
|Italia|Italiano|it-IT|
|Japón|Japonés|ja-JP|
|Corea|Coreano|ko-KR|
|Malasia|English|en-MY|
|México|Español|es-MX|
|Países Bajos|Neerlandés|nl-NL|
|Nueva Zelanda|English|en-NZ|
|Noruega|Noruego|no-NO|
|China|Chino|zh-CN|
|Polonia|Polaco|pl-PL|
|Portugal|Portugués|pt-PT|
|Filipinas|English|en-PH|
|Rusia|Ruso|ru-RU|
|Arabia Saudí|Árabe|ar-SA|
|Sudáfrica|English|en-ZA|
|España|Español|es-ES|
|Suecia|Sueco|sv-SE|
|Suiza|Francés|fr-CH|
|Suiza|Alemán|de-CH|
|Taiwán|Chino tradicional|zh-TW|
|Turquía|Turco|tr-TR|
|Reino Unido|English|en-GB|
|Estados Unidos|English|en-US|
|Estados Unidos|Español|es-US|
