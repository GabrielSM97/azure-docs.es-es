---
title: Preguntas frecuentes relacionadas con Windows Server de Azure Stack | Microsoft Docs
description: Lista de preguntas frecuentes del Marketplace de Azure Stack para Windows Server
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2018
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 11/12/2018
ms.openlocfilehash: 03a6f649f15f6a4905433d6e2ec292a901340929
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249687"
---
# <a name="windows-server-in-azure-stack-marketplace-faq"></a>Preguntas frecuentes sobre Windows Server en el Marketplace de Azure Stack

En este artículo se responde a algunas preguntas frecuentes sobre las imágenes de Windows Server en el [Marketplace de Azure Stack](azure-stack-marketplace.md).

## <a name="marketplace-items"></a>Elementos de Marketplace

### <a name="how-do-i-update-to-a-newer-windows-image"></a>¿Cómo actualizo a una imagen de Windows más reciente?

En primer lugar, determine si las plantillas de Azure Resource Manager hacen referencia a alguna versión específica. Si es así, actualice las plantillas o mantenga versiones de imágenes antiguas. Es mejor usar **version: latest**.

A continuación, si las instancias de Virtual Machine Scale Sets hacen referencia a una versión específica, debería pensar si será necesario escalarlas más tarde y decidir si conservar las versiones anteriores. Si ninguna de estas condiciones es aplicable, elimine las imágenes antiguas de Marketplace antes de descargar las más recientes. Puede usar Administración de Marketplace para hacer esto si es así cómo se descargó el original. A continuación, descargue la versión más reciente.

### <a name="what-are-the-licensing-options-for-windows-server-marketplace-images-on-azure-stack"></a>¿Qué opciones hay para obtener licencias de imágenes de Windows Server en el Marketplace de Azure Stack?

Microsoft ofrece dos versiones de imágenes de Windows Server a través del Marketplace de Azure Stack:

- **Pago por uso**: estas imágenes ejecutan los medidores de precio completo de Windows. 
   ¿Quién debe usarlo?: Clientes con contratos Enterprise (EA) que usan el *modelo de facturación de consumo*; o para CSP que no desean usar licencias de SPLA.
- **Traiga su propia licencia (BYOL)**: Estas imágenes ejecutan medidores básicos.
   ¿Quién debe usarlo?: Clientes con EA y con una licencia de Windows Server; o para CSP que usan licencias de SPLA.

Ventaja de uso híbrido de Azure (AHUB) no se admite en Azure Stack. Los clientes que obtienen licencias a través del modelo "Capacidad" deben usar la imagen BYOL. Si va a realizar pruebas con el Kit de desarrollo de Azure Stack (ASDK), puede usar cualquiera de estas opciones.

### <a name="what-if-i-downloaded-the-wrong-version-to-offer-my-tenantsusers"></a>¿Qué ocurre si descargué la versión incorrecta para mis inquilinos o usuarios?

Primero, debe eliminar la versión incorrecta a través de Administración de Marketplace. Espere a que la eliminación finalice por completo (consulte las notificaciones que indican la finalización, no la hoja de Administración de Marketplace). A continuación, descargue la versión correcta.

### <a name="what-if-my-user-incorrectly-checked-the-i-have-a-license-box-in-previous-windows-builds-and-they-dont-have-a-license"></a>¿Qué ocurre si mi usuario marco incorrectamente el cuadro "Tengo una licencia" en compilaciones anteriores de Windows y no tiene ninguna licencia?

Consulte [Conversión de nuevo de las máquinas virtuales de Windows Server con la ventaja al pago por uso](../virtual-machines/windows/hybrid-use-benefit-licensing.md#powershell-1).

### <a name="what-if-i-have-an-older-image-and-my-user-forgot-to-check-the-i-have-a-license-box-or-we-use-our-own-images-and-we-do-have-enterprise-agreement-entitlement"></a>¿Qué ocurre si tengo una imagen anterior y el usuario olvidó marcar el cuadro "Tengo una licencia"? ¿Y si usamos nuestras propias imágenes y tenemos un Contrato Enterprise?

Consulte [Conversión de una máquina virtual existente con la Ventaja híbrida de Azure para Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md#convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server). Tenga en cuenta que la ventaja híbrida de Azure no se aplica a Azure Stack, pero sí el efecto de esta configuración.

### <a name="what-about-other-vms-that-use-windows-server-such-as-sql-or-machine-learning-server"></a>¿Qué ocurre con otras máquinas virtuales que usan Windows Server, como SQL o Machine Learning Server?

Estas imágenes se aplican al parámetro **licenseType**, por lo que se pagan en función de su uso. Puede establecer este parámetro (consulte la pregunta anterior). Esto solo se aplica al software de Windows Server, y no a productos en capas, como SQL, que tienen que aportar su propia licencia. Las licencias de pago por uso no se aplican a productos de software en capas.

### <a name="i-have-an-enterprise-agreement-ea-and-will-be-using-my-ea-windows-server-license-how-do-i-make-sure-images-are-billed-correctly"></a>Tengo un contrato Enterprise (EA) y usaré mi licencia EA Windows Server, ¿cómo puedo asegurarme de que las imágenes se facturarán correctamente?

Puede agregar **licenseType: Windows_Server** en una plantilla de Azure Resource Manager. Esta configuración debe agregarse a cada bloque de recursos de máquina virtual.

## <a name="activation"></a>Activación

Para activar una máquina virtual de Windows Server en Azure Stack, deben cumplirse las condiciones siguientes:

- El OEM estableció el marcador de BIOS adecuado en todos los sistemas host de Azure Stack.
- Windows Server 2012 R2 y Windows Server 2016 deben usar la [activación automática de máquina virtual](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)). En Azure Stack, no se admiten el Servicio de administración de claves (KMS) ni otros servicios de activación.

### <a name="how-can-i-verify-that-my-virtual-machine-is-activated"></a>¿Cómo puedo comprobar que mi máquina virtual está activada?

Ejecute el siguiente comando en un símbolo del sistema con privilegios elevados: 

```shell
slmgr /dlv
``` 

Si se activa correctamente, verá esto indicado claramente y el nombre de host se mostrará en la salida de `slmgr`. No se base en las marcas de agua en la pantalla, ya que es posible que no estén actualizadas o se muestren desde una máquina virtual anterior a la suya.

### <a name="my-vm-is-not-set-up-to-use-avma-how-can-i-fix-it"></a>Mi máquina virtual no está configurada para usar AVMA, ¿cómo puedo arreglarlo?

Ejecute el siguiente comando en un símbolo del sistema con privilegios elevados: 

```shell
slmgr /ipk <AVMA key> 
```

Consulte el artículo [Activación de la máquina virtual automática](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) para obtener las claves necesarias para usar su imagen.

### <a name="i-create-my-own-windows-server-images-how-can-i-make-sure-they-use-avma"></a>Me encargo de crear mis propias imágenes de Windows Server, ¿cómo puedo asegurarme de que usan AVMA?

Se recomienda que ejecute la línea de comandos `slmgr /ipk` con la clave adecuada antes de ejecutar el comando `sysprep`. O bien, puede incluir la clave de AVMA en cualquier archivo de instalación Unattend.exe.

### <a name="i-am-trying-to-use-my-windows-server-2016-image-created-on-azure-and-it-is-not-activating-or-using-kms-activation"></a>Estoy intentando usar mi imagen de Windows Server 2016, que cree en Azure, y no se activa o no usa la activación de KMS.

Ejecute el comando `slmgr /ipk`. Es posible que las imágenes de Azure no recurran correctamente a AVMA; pero si alcanzan el sistema KMS de Azure, se activarán. Es recomendable asegurarse de que estas máquinas virtuales están configuradas para usar AVMA.

### <a name="i-have-performed-all-of-these-steps-but-my-virtual-machines-are-still-not-activating"></a>He realizado todos estos pasos, pero sigo sin conseguir activar mis máquinas virtuales.

Póngase en contacto con su proveedor de hardware para comprobar que se han instalado los marcadores de BIOS correctos.

### <a name="what-about-earlier-versions-of-windows-server"></a>¿Qué sucede con las versiones anteriores de Windows Server?

[Activación automática de máquina virtual](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) no se admite en versiones anteriores de Windows Server. Tendrá que activar las máquinas virtuales manualmente.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos.

- [Información general de Azure Stack Marketplace](azure-stack-marketplace.md)
- [Descarga de elementos de Marketplace desde Azure a Azure Stack](azure-stack-download-azure-marketplace-item.md)
