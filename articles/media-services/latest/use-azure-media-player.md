---
title: 'Reproducción con Azure Media Player: Azure | Microsoft Docs'
description: En este tema se proporciona información general sobre Azure Media Player.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/03/2018
ms.author: juliako
ms.openlocfilehash: 4dd0f697d16d7a2fbeec8f712d98e6a7c439cb4e
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064685"
---
# <a name="azure-media-player-overview"></a>Información general sobre Azure Media Player

Azure Media Player es un reproductor de vídeo web integrado que reproduce contenido multimedia de Microsoft Azure Media Services en una gran variedad de exploradores y dispositivos. Explorador multimedia de Azure utiliza los estándares del sector, como HTML5, Media Source Extensions (MSE, extensiones de origen multimedia) y Encrypted Media Extensions (EME, extensiones multimedia cifradas) para proporcionar una experiencia de streaming adaptativa enriquecida. Cuando estos estándares no están disponibles en un dispositivo o en un explorador, Azure Media Player usa Flash y Silverlight como tecnología de reserva. Independientemente de la tecnología de reproducción que se usa, los desarrolladores tendrán una interfaz unificada de JavaScript para tener acceso a las API. Esto permite que el contenido proporcionado por Azure Media Services se reproduzca a través de una amplia gama de dispositivos y exploradores sin ningún esfuerzo adicional.

Microsoft Azure Media Services permite suministrar contenido con formatos de streaming HLS, DASH y Smooth Streaming para su reproducción. Azure Media Player tiene en cuenta estos diversos formatos y reproduce automáticamente el vínculo mejor según las capacidades del explorador y plataforma. Media Services también permite el cifrado dinámico de recursos con el cifrado PlayReady o el cifrado de sellado de AES de 128 bits. Azure Media Player permite el descifrado del contenido cifrado de AES de 128 bits y PlayReady cuando se configura correctamente. 

[Iniciar la versión de evaluación gratuita](http://azure.microsoft.com/en-us/pricing/free-trial/)

## <a name="use-azure-media-player-demo-page"></a>Uso de la página de demostración de Azure Media Player

### <a name="start-using"></a>Primer uso

Puede usar la [página de demostración de Azure Media Player](http://aka.ms/amp) para reproducir los ejemplos de Azure Media Services o su propia secuencia.  

Para reproducir un vídeo nuevo, pegue una dirección URL diferente y presione **Update** (Actualizar).

Para configurar diversas opciones de reproducción (por ejemplo, tecnología, idioma o cifrado), presione **Advanced Options** (Opciones avanzadas).

![Azure Media Player](./media/azure-media-player/home-page.png)

### <a name="monitor-diagnostics-of-a-video-stream"></a>Supervisión de diagnósticos de una secuencia de vídeo

Puede usar la [página de demostración de Azure Media Player](http://aka.ms/amp) para supervisar los diagnósticos de una secuencia de vídeo. 

![Diagnósticos de Azure Media Player](./media/azure-media-player/diagnostics.png)

## <a name="set-up-azure-media-player-in-your-html"></a>Configuración de Azure Media Player en HTML

Azure Media Player es fácil de configurar. Solo se tarda unos minutos en obtener la reproducción básica de contenido multimedia desde la cuenta de Media Services. Para más información sobre cómo instalar y configurar Azure Media Player, consulte la [documentación de Azure Media Player](https://aka.ms/ampdocs). 

## <a name="next-steps"></a>Pasos siguientes

- [Documentación de Azure Media Player](https://aka.ms/ampdocs)
- [Ejemplos de Azure Media Player](https://aka.ms/ampsamples)
