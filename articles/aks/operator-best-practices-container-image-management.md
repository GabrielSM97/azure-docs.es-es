---
title: 'Procedimientos recomendados para operadores: Administración de imágenes de contenedor en Azure Kubernetes Services (AKS)'
description: Obtenga información sobre los procedimientos recomendados del operador de clústeres para saber cómo administrar y proteger las imágenes de contenedor en Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: iainfou
ms.openlocfilehash: 1cc91f55d3895f06176875cb9ae620685dc09a26
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53605558"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Procedimientos recomendados para la administración y la protección de las imágenes de contenedor en Azure Kubernetes Service (AKS)

A medida que desarrolla y ejecuta aplicaciones en Azure Kubernetes Service (AKS), es importante tener en cuenta la seguridad de los contenedores y de sus imágenes. Los contenedores que incluyen imágenes base obsoletas o entornos de ejecución de la aplicación sin revisiones presentan un riesgo de seguridad y un posible vector de ataque. Para minimizar estos riesgos, debe integrar herramientas que busquen y corrijan errores en los contenedores tanto en el momento de la compilación como en el momento de la ejecución. Cuanto más temprano en el proceso se detecte la vulnerabilidad o la imagen de base desactualizada, más seguro será el clúster. En este artículo, *contenedores* se refiere tanto a las imágenes de contenedor almacenadas en un registro de contenedor como a los contenedores en ejecución.

En este artículo se indica cómo proteger los contenedores de AKS. Aprenderá a:

> [!div class="checklist"]
> * Buscar y corregir vulnerabilidades de la imagen
> * Usar un registro de confianza con imágenes de contenedor firmadas digitalmente
> * Desencadenar automáticamente y volver a implementar imágenes de contenedor cuando se actualiza una imagen base

También puede leer las prácticas recomendadas para la [seguridad del clúster][best-practices-cluster-security] y para la [seguridad de pod][best-practices-pod-security].

## <a name="secure-the-images-and-run-time"></a>Proteger las imágenes y el tiempo de ejecución

**Orientación con procedimientos recomendados**: analizar las imágenes del contenedor en busca de vulnerabilidades y solo implementar las imágenes que han pasado dicha validación. Actualizar regularmente las imágenes base y el entorno de ejecución de la aplicación y luego volver a implementar las cargas de trabajo en el clúster de AKS.

Un problema con la adopción de cargas de trabajo basadas en contenedores es comprobar la seguridad de las imágenes y el entorno de ejecución usados para compilar sus propias aplicaciones. ¿Cómo asegurarse de que no introduce vulnerabilidades de seguridad en sus implementaciones? El flujo de trabajo de implementación debe incluir un proceso para examinar imágenes de contenedor con herramientas como [Twistlock][twistlock] o [Aqua][aqua] y, después, solo permitir que se implementen imágenes comprobadas.

![Analizar y corregir imágenes de contenedor, validar e implementar](media/operator-best-practices-container-security/scan-container-images-simplified.png)

En un ejemplo del mundo real, puede usar una integración continua y una canalización de implementación continua (CI/CD) para automatizar el análisis, la comprobación y la implementación de imágenes. Azure Container Registry incluye estas funcionalidades de análisis de vulnerabilidades.

## <a name="use-a-trusted-registry"></a>Usar un registro de confianza

**Orientación con procedimientos recomendados**: limitar los registros de imágenes que pueden usar los pods y las implementaciones. Permitir solo registros de confianza donde puede validar y controlar las imágenes que están disponibles.

Para mayor seguridad, puede firmar digitalmente las imágenes del contenedor tal como se puede firmar digitalmente el código de la aplicación. A continuación, solo permitirá que AKS implemente imágenes firmadas. Este proceso proporciona una capa adicional de seguridad en la que limitar AKS para extraer solo las imágenes firmadas digitalmente y de confianza, no solo las imágenes que pasan una comprobación de vulnerabilidad. Asegúrese también de que la imagen de contenedor no se ha alterado ni reemplazado por una imagen con el mismo nombre.

Los registros de confianza que proporcionan las imágenes de contenedor firmadas digitalmente agregan complejidad al entorno, pero pueden ser necesarios para determinadas directivas o cumplimiento de normativas. Azure Container Registry admite el uso de los registros de confianza e imágenes firmadas.

Para obtener más información acerca de las imágenes firmadas digitalmente, vea [Confianza en el contenido en Azure Container Registry][acr-content-trust].

## <a name="automatically-build-new-images-on-base-image-update"></a>Compilar automáticamente nuevas imágenes en la actualización de imagen base

**Orientación con procedimientos recomendados**: cuando usa imágenes base para las imágenes de la aplicación, use la automatización para crear nuevas imágenes cuando se actualiza la imagen base. Como estas imágenes base suelen incluir correcciones de seguridad, actualice cualquier imagen de contenedor de aplicación de bajada.

Cada vez que se actualiza una imagen base, también se deben actualizar las imágenes de contenedor de bajada. Este proceso de compilación debe integrarse en las canalizaciones de validación e implementación, como [Azure Pipelines][azure-pipelines] o Jenkins. Estas canalizaciones aseguran que tus aplicaciones siguen ejecutándose en las imágenes base actualizadas. Una vez que se validan las imágenes de contenedor de aplicación, se pueden actualizar las implementaciones de AKS para ejecutar las imágenes más recientes y seguras.

Azure Container Registry Tasks también puede actualizar automáticamente las imágenes de contenedor cuando se actualiza la imagen base. Esta característica permite compilar un número pequeño de imágenes base, y mantenerlas actualizadas regularmente con las correcciones de errores y de seguridad.

Para obtener más información acerca de las actualizaciones de imagen base, vea [Automatización de compilaciones de imágenes en la actualización de imagen base con Azure Container Registry Tasks][acr-base-image-update].

## <a name="next-steps"></a>Pasos siguientes

En este artículo se indica cómo proteger los contenedores. Para implementar algunas de estas áreas, consulte los artículos siguientes:

* [Automatización de compilaciones de imágenes en la actualización de imagen base con Azure Container Registry Tasks][acr-base-image-update]
* [Confianza en el contenido en Azure Container Registry][acr-content-trust]

<!-- EXTERNAL LINKS -->
[azure-pipelines]: /azure/devops/pipelines/?view=vsts
[twistlock]: https://www.twistlock.com/
[aqua]: https://www.aquasec.com/

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[acr-content-trust]: ../container-registry/container-registry-content-trust.md
[acr-base-image-update]: ../container-registry/container-registry-tutorial-base-image-update.md
