---
title: Administración de perfiles de la versión de la API de Azure Stack | Microsoft Docs
description: Aprenda los perfiles de la versión de API en Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 4977e96e215aa58873146fe57203bbd895df551e
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55769834"
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Administre los perfiles de la versión de API en Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Los perfiles de la API especifican el proveedor de recursos de Azure y la versión de la API para los puntos de conexión REST de Azure. Puede crear clientes personalizados en distintos idiomas mediante perfiles de API. Cada cliente utiliza un perfil de API para ponerse en contacto con el proveedor de recursos correcto y la versión de API para Azure Stack.

Puede crear una aplicación para trabajar con los proveedores de recursos de Azure sin tener que escoger exactamente qué versión de cada API de proveedor de recursos es compatible con Azure Stack. Solo tiene que alinear la aplicación con un perfil; el SDK vuelve a las versiones de la API correctas.

Este tema le ayudará a:

 - Comprender los perfiles de API para Azure Stack.
 - Aprender a utilizar los perfiles de la API para desarrollar soluciones.
 - Ver dónde obtener guías específicas del código.

## <a name="summary-of-api-profiles"></a>Resumen de perfiles de API

- Los perfiles de API se utilizan para representar un conjunto de proveedores de recursos de Azure y sus versiones de API.
- Los perfiles de API se crearon para que pueda crear plantillas por varias nubes de Azure. Los perfiles proporcionan una interfaz compatible y estable.
- Los perfiles se publican cuatro veces al año.
- Se utilizan tres perfiles de convenciones de nomenclatura:
    - **más reciente**  
        Contiene las versiones más recientes de la API publicadas en la versión global de Azure.
    - **yyyy-mm-dd-hybrid**  
    Esta versión de publicación semestral se centra en la estabilidad y coherencia entre varias nubes. Este perfil tiene como objetivo la compatibilidad óptima de Azure Stack.
    - **yyyy-mm-dd-profile** Equilibra la estabilidad óptima con las últimas características.

## <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Compatibilidad con perfiles de la API de Azure y Azure Stack

Los perfiles de la API de Azure más recientes no son compatibles con Azure Stack. Puede usar las siguientes convenciones de nomenclatura para identificar qué perfiles usará para las soluciones de Azure Stack.

**Más reciente**  
Este perfil tiene la versión de API más actualizada que se encuentra en todo Azure, pero no funciona en Azure Stack. **Más reciente** tiene el mayor número de cambios importantes. Asimismo, deja de lado la estabilidad y la compatibilidad con otras nubes. Si intenta usar las versiones de API más recientes, **Más reciente** es el perfil que debe usar.

**Yyyy-mm-dd-hybrid**  
Este perfil se publica en marzo y septiembre de cada año. Asimismo, tiene una estabilidad y compatibilidad óptimas con varias nubes. **Yyyy-mm-dd-hybrid** está diseñado para funcionar tanto en la versión global de Azure como en Azure Stack. Las versiones de API de Azure enumeradas en este perfil serán las mismas que las que se muestran en Azure Stack. Puede usar este perfil para desarrollar código para soluciones de nube híbrida.

**yyyy-mm-dd-profile**  
Este perfil se publica para la versión global de Azure en junio y diciembre. No funciona con Azure Stack, ya que, normalmente, habrá muchos cambios importantes. Si bien equilibra estabilidad óptima con las últimas características, la diferencia entre **Más reciente** y este perfil es que **Más reciente** siempre tendrá las últimas versiones de API, independientemente de cuándo se lanzaran. Por ejemplo, si se crea una nueva versión de API para Compute API mañana, esa versión de API se mostrará en **Más reciente**, pero no en **yyyy-mm-dd-profile**, dado que este perfil ya existe. **yyyy-mm-dd-profile** cubre las versiones más actualizadas lanzadas antes de junio o diciembre.

## <a name="azure-resource-manager-api-profiles"></a>Perfiles de la API de Azure Resource Manager

Azure Stack no utiliza la última versión de las versiones de API que se encuentran en la versión global de Azure. Al crear una solución, debe encontrar la versión de API para cada proveedor de recursos en Azure que sea compatible con Azure Stack.

En lugar de investigar cada proveedor de recursos y la versión específica admitida por Azure Stack, puede usar un perfil de API. El perfil especifica un conjunto de proveedores de recursos y las versiones de API. El SDK o una herramienta integrada con el SDK, devolverá el valor de `api-version` de destino especificada en el perfil. Con los perfiles de API, puede especificar una versión de perfil que se aplique a una plantilla completa y, en el entorno de ejecución, Azure Resource Manager selecciona la versión correcta del recurso.

Los perfiles de API funcionan con herramientas que utilizan Azure Resource Manager, como PowerShell, CLI de Azure, código proporcionado en el SDK y Microsoft Visual Studio. Las herramientas y los SDK pueden utilizar perfiles para leer la versión de los módulos y bibliotecas que se van incluir al crear una aplicación.

Por ejemplo, si utiliza PowerShell para crear una cuenta de almacenamiento con el proveedor de recursos **Microsoft.Storage**, que admite **api-version** 2016-03-30 y una máquina virtual que utiliza el proveedor de recursos **Microsoft.Compute** con **api-version** 2015-12-01, debe buscar qué módulo de PowerShell admite 2016-03-30 para Storage y qué módulo admite 2015-02-01 para Compute e instalarlos. En su lugar, puede usar un perfil. Use el cmdlet `Install-Profile <profilename>` y PowerShell carga la versión correcta de los módulos.

De forma similar, cuando usa el SDK de Python para crear una aplicación basada en Python, puede especificar el perfil. El SDK carga los módulos correctos para los proveedores de recursos que ha especificado en el script.

Como desarrollador, puede centrarse en escribir la solución. En lugar de investigar qué versiones de API, qué proveedor de recursos y qué nube funcionan juntos, puede usar un perfil y saber qué código va a funcionar en todas las nubes que lo admiten.

## <a name="api-profile-code-samples"></a>Ejemplos de código de perfil de API

Puede encontrar ejemplos de código que le ayudarán a integrar la solución con el idioma que prefiera con Azure Stack mediante el uso de perfiles. Actualmente, puede encontrar guía y ejemplos para los siguientes idiomas:

- **.NET** Puede usar el perfil de API de .NET para obtener la versión más reciente y más estable de cada tipo de recurso de un paquete de proveedor de recursos. Para más información, consulte [Uso de los perfiles de la versión de la API con .NET en Azure Stack](azure-stack-version-profiles-net.md).
- **PowerShell**  
Se puede usar el módulo **AzureRM.Bootstrapper** disponible mediante la Galería de PowerShell para obtener cmdlets de PowerShell que son necesarios para trabajar con perfiles de la versión de la API. Para más información, consulte el artículo sobre el [uso de perfiles de versión de API para PowerShell](azure-stack-version-profiles-powershell.md).
- **CLI de Azure**  
Puede actualizar la configuración del entorno para usar el perfil de la versión de API específico de Azure Stack. Para más información, consulte el artículo [Uso de los perfiles de la versión de la API con la CLI de Azure](azure-stack-version-profiles-azurecli2.md).
- **Go**  
En el SDK de Go, un perfil es una combinación de diferentes tipos de recursos con distintas versiones de diferentes servicios. Los perfiles están disponibles en la ruta de acceso profiles/, con la versión en formato **AAAA-MM-DD**. Para más información, consulte el artículo sobre el [uso de perfiles de versión de API para Go](azure-stack-version-profiles-go.md).
- **Ruby**  
El SDK de Ruby para Resource Manager de Azure Stack proporciona herramientas que le ayudarán a crear y administrar su infraestructura. Proveedores de recursos en el SDK incluye Compute, Redes virtuales y opciones de almacenamiento con el lenguaje Ruby. Para obtener más información, consulte el [uso de perfiles de versión de API con Ruby](azure-stack-version-profiles-ruby.md).
- **Python**  
El SDK de Python es compatible con los perfiles de la versión de la API para su uso con diferentes plataformas en la nube, como Azure Stack y Azure global. Puede utilizar los perfiles de la API para crear soluciones para una nube híbrida. Para más información, consulte [Usar perfiles de la versión de la API con Python](azure-stack-version-profiles-python.md).

## <a name="next-steps"></a>Pasos siguientes

* [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) (Instalación de PowerShell para Azure Stack)
* [Configuración del entorno de PowerShell del usuario de Azure Stack](azure-stack-powershell-configure-user.md)
* [Revise los detalles sobre las versiones de API del proveedor de recursos admitidas por los perfiles](azure-stack-profiles-azure-resource-manager-versions.md).
