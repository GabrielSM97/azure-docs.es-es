---
title: 'Traducción detrás de los firewalls: Translator Text API'
titlesuffix: Azure Cognitive Services
description: Realice sus traducciones detrás de los firewalls de direcciones IP con Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: 1e598594f53bc94d3a740d2bd5cdd100456131fa
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884739"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>Traducción detrás de los firewalls de direcciones IP con Translator Text API

Translator Text API puede traducir detrás de los firewalls utilizando el filtrado de nombres de dominio o de direcciones IP. El filtrado de nombres de dominio es el método preferido. **No se recomienda** ejecutar Microsoft Translator desde detrás de un firewall con filtrado de direcciones IP. Es probable que la configuración se interrumpa en el futuro sin previo aviso. 

## <a name="translator-ip-addresses"></a>Direcciones IP de Translator
Direcciones IP de api.cognitive.microsofttranslator.com en Microsoft Translator Text API desde el 20 de noviembre de 2018:

* **Asia Pacífico:** 40.90.139.163, 104.44.89.44
* **Europa:** 40.90.138.4, 40.90.141.99
* **Norteamérica:** 40.90.139.36, 40.90.139.2


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Translate behind IP firewalls in your Translator API call](reference/v3-0-translate.md) (Traducción detrás de los firewalls de direcciones IP en la llamada API de Translator)
