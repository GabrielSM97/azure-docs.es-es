---
title: 'Solución de problemas del SDK de Voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: En este artículo se proporciona información para ayudarle a resolver los problemas que pueden surgir al usar el SDK del servicio Voz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: dbcdfd117a39939491914ebddb717f404e07f09c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859324"
---
# <a name="troubleshoot-the-speech-service-sdk"></a>Solución de problemas del SDK del servicio Voz

En este artículo se proporciona información para ayudarle a resolver los problemas que pueden surgir al usar el SDK del servicio Voz.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Error: Error de actualización de WebSocket con un error de autenticación (403)

Puede que el punto de conexión sea incorrecto para su región o servicio. Compruebe el URI para asegurarse de que sea correcto.

Además, puede que haya un problema con la clave de suscripción o el token de autorización. Para más información, consulte la siguiente sección.

## <a name="error-http-403-forbidden-or-http-401-unauthorized"></a>Error: HTTP 403 Prohibido o HTTP 401 No autorizado

Este error suele deberse a problemas de autenticación. Las solicitudes de conexión sin un encabezado `Ocp-Apim-Subscription-Key` o `Authorization` válido se rechazan con el estado 403 o 401.

* Si usa una clave de suscripción para la autenticación, el error puede aparecer por las razones siguientes:

    - Falta la clave de suscripción o no es válida.
    - Ha superado la cuota de uso de la suscripción.

* Si usa un token de autorización para la autenticación, el error puede aparecer por las razones siguientes:

    - El token de autorización no es válido.
    - El token de autorización ha expirado.

### <a name="validate-your-subscription-key"></a>Validar la clave de suscripción

Para comprobar si tiene una clave de suscripción válida, ejecute uno de los comandos siguientes.

> [!NOTE]
> Reemplace `YOUR_SUBSCRIPTION_KEY` y `YOUR_REGION` por su propia clave de la suscripción y región asociada.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

Si escribió una clave de suscripción válida, el comando devuelve un token de autorización; de lo contrario, se devuelve un error.

### <a name="validate-an-authorization-token"></a>Validar un token de autorización

Si usa un token de autorización para la autenticación, ejecute uno de los siguientes comandos para comprobar que el token de autorización sigue siendo válido. Los tokens son válidos durante 10 minutos.

> [!NOTE]
> Reemplace `YOUR_AUDIO_FILE` por la ruta de acceso al archivo de audio grabado previamente. Reemplace `YOUR_ACCESS_TOKEN` por el token de autorización devuelto en el paso anterior. Reemplace `YOUR_REGION` por la región correcta.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'

    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }

    # Read audio into byte array.
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

    # Show the result.
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

Si escribió un token de autorización válido, el comando devuelve la transcripción del archivo de audio; de lo contrario, se devuelve un error.

---

## <a name="error-http-400-bad-request"></a>Error: HTTP 400 Solicitud incorrecta

El motivo suele darse cuando el cuerpo de la solicitud contiene datos de audio no válidos. Solo se admite el formato WAV. Compruebe también los encabezados de la solicitud para asegurarse de que está especificando valores correctos para `Content-Type` y `Content-Length`.

## <a name="error-http-408-request-timeout"></a>Error: HTTP 408 Tiempo de espera de solicitud

El error suele producirse porque no se envía ningún dato de audio al servicio. Este error también podría deberse a problemas de red.

## <a name="recognitionstatus-in-the-response-is-initialsilencetimeout"></a>"RecognitionStatus" en la respuesta es "InitialSilenceTimeout"

Este problema suele deberse a datos de audio. Puede que aparezca este error por las razones siguientes:

* Se produce un período de silencio prolongado al principio del audio. En ese caso, el servicio detiene el reconocimiento después de unos segundos y devuelve `InitialSilenceTimeout`.

* El audio usa un formato de códec no admitido, que hace que los datos de audio se traten como silencio.

## <a name="next-steps"></a>Pasos siguientes

* [Revise las notas de la versión.](releasenotes.md)
