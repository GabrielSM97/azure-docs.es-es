---
title: Solución de problemas de conmutación por error en Azure | Microsoft Docs
description: En este artículo se describe cómo solucionar problemas y errores comunes de la conmutación por error en Azure
author: ponatara
manager: abhemraj
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 1/29/2019
ms.author: mayg
ms.openlocfilehash: 62b69364f0b3d3e14d0b2d877604cecfcc346dce
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207503"
---
# <a name="troubleshoot-errors-when-failing-over-vmware-vm-or-physical-machine-to-azure"></a>Solución de problemas cuando se conmuta por error una máquina física o una máquina virtual de VMware en Azure

Es posible que aparezca uno de los errores siguientes mientras se realiza la conmutación por error de una máquina virtual en Azure. Para solucionar los problemas, use los pasos que se describen para cada condición de error.

## <a name="failover-failed-with-error-id-28031"></a>No se pudo realizar la conmutación por error con el identificador de error 28031

Site Recovery no pudo crear una máquina virtual conmutada por error en Azure. Esto podría deberse a uno de los siguientes motivos:

* No hay cuota suficiente disponible para crear la máquina virtual: Para comprobar la cuota disponible, vaya a Suscripción ->Uso y cuotas. Puede abrir una [solicitud de soporte técnico nueva](http://aka.ms/getazuresupport) para aumentar la cuota.

* Se intenta conmutar por error máquinas virtuales de familias de distinto tamaño en el mismo conjunto de disponibilidad. Asegúrese de elegir una familia del mismo tamaño para todas las máquinas virtuales del mismo conjunto de disponibilidad. Cambie el tamaño en la configuración de Proceso y red de la máquina virtual y reintente la conmutación por error.

* Hay una directiva de la suscripción que impide crear una máquina virtual. Cambie la directiva para permitir la creación de una máquina virtual y reintente la conmutación por error.

## <a name="failover-failed-with-error-id-28092"></a>No se pudo realizar la conmutación por error con el identificador de error 28092

Site Recovery no pudo crear una interfaz de red para la máquina virtual conmutada por error. Asegúrese de tener una cuota suficiente disponible para crear interfaces de red en la suscripción. Para comprobar la cuota disponible, vaya a Suscripción ->Uso y cuotas. Puede abrir una [solicitud de soporte técnico nueva](http://aka.ms/getazuresupport) para aumentar la cuota. Si tiene una cuota suficiente, puede tratarse de un error intermitente. Vuelva a intentar la operación. Si el problema sigue después de varios reintentos, deje un comentario al final de este documento.  

## <a name="failover-failed-with-error-id-70038"></a>No se pudo realizar la conmutación por error con el identificador de error 70038

Site Recovery no pudo crear una máquina virtual clásica conmutada por error en Azure. Esto podría suceder porque:

* No existe uno de los recursos, como una red virtual, que se requieren para crear la máquina virtual. Cree la red virtual según lo indicado en la configuración de Proceso y red de la máquina virtual, o bien modifique la configuración a una red virtual que ya existe y reintente la conmutación por error.

## <a name="failover-failed-with-error-id-170010"></a>No se pudo realizar la conmutación por error con el identificador de error 170010

Site Recovery no pudo crear una máquina virtual conmutada por error en Azure. Esto podría suceder porque se produjo un error en una actividad interna de hidratación de la máquina virtual local.

Para mostrar todas las máquinas de Azure, el entorno de Azure requiere que algunos de los controladores tengan el estado de inicio de arranque y que servicios como DHCP tengan el estado de inicio automático. Por lo tanto, la actividad hidratación, en el momento de la conmutación por error, convierte el tipo de inicio de **atapi, intelide, storflt, vmbus, and storvsc drivers** en el inicio del arranque. También se convierte el tipo de inicio de una serie de servicios, como DHCP, para iniciarse automáticamente. Esta actividad puede producir un error debido a problemas específicos del entorno. 

Para cambiar manualmente el tipo de inicio de los controladores para el **sistema operativo invitado de Windows**, siga estos pasos:

1. [Descargue](http://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1) el script de no hidratación y ejecútelo de esta forma. Este script comprueba si la máquina virtual requiere hidratación.

    `.\Script-no-hydration.ps1`

    Proporciona el siguiente resultado si se requiere la hidratación:

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0

        This system doesn't meet no-hydration requirement.

    En caso de que la máquina virtual cumpla el requisito de no hidratación, el script proporcionará el resultado "This system meets no-hydration requirement" (Este sistema cumple los requisitos de no hidratación). En este caso, todos los conductores y servicios están en el estado requerido por Azure y no se requiere hidratación en la máquina virtual.

2. Ejecute el script no-hydration-set como se indica a continuación si la máquina virtual no cumple el requisito de no hidratación.

    `.\Script-no-hydration.ps1 -set`
    
    Esto convertirá el tipo de inicio de los controladores y ofrecerá un resultado similar al siguiente:
    
        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0 

        Updating registry:  REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc   start =  0 

        This system is now no-hydration compatible. 

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>No se puede conectar/RDP/SSH a la máquina virtual que conmutó por error debido a que el botón Conectar de la máquina virtual no está disponible.

Si el botón **Conectar** de la máquina virtual conmutada por error de Azure no está disponible y no está conectado a Azure a través de una conexión VPN Express Route o de sitio a sitio, entonces:

1. Vaya a la **Máquina virtual** > **Red**, y haga clic en el nombre de la interfaz de red necesaria.  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navegue hasta **Configuraciones IP** y, después, haga clic en el campo de nombre de la configuración IP necesaria. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Para habilitar la dirección IP pública, haga clic en **Habilitar**. ![Habilitar IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Haga clic en **Configurar los valores obligatorios** > **Crear uno nuevo**. ![Cree uno nuevo](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Escriba el nombre de la dirección pública, seleccione las opciones predeterminadas para **SKU**y **asignación** y, después, haga clic en **Aceptar**.
6. Ahora, haga clic en **Guardar** para guardar los cambios.
7. Cierre los paneles y navegue a la sección **Introducción** de la máquina virtual para conectar/RDP.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>No se puede conectar/RDP/botón de conexión SSH con la máquina virtual disponible

Si el botón **Conectar** de la máquina virtual conmutada por error de Azure está disponible, compruebe **Diagnósticos de arranque** en su máquina virtual, así como los errores enumerados en [este artículo](../virtual-machines/windows/boot-diagnostics.md).

1. Si la máquina virtual no se ha iniciado, realice la conmutación por error a un punto de recuperación anterior.
2. Si la aplicación dentro de la máquina virtual no aparece, realice la conmutación por error a un punto de recuperación coherente con la aplicación.
3. Si la máquina virtual está unida al dominio, asegúrese de que ese controlador de dominio funciona correctamente. Esto se puede hacer siguiendo estos pasos:

     a. Cree una nueva máquina virtual en la misma red.

    b.  Asegúrese de que es capaz de unirse al mismo dominio en el que se espera que aparezca la máquina virtual que ha conmutado por error.

    c. Si el controlador de dominio **no** funciona correctamente, inicie sesión en la máquina virtual con conmutación por error mediante una cuenta de administrador local.
4. Si está utilizando un servidor DNS personalizado, asegúrese de que es accesible. Esto se puede hacer siguiendo estos pasos:

     a. Cree una nueva máquina virtual en la misma red y

    b. Compruebe si la máquina virtual es capaz de realizar la resolución de nombres con el servidor DNS personalizado.

>[!Note]
>Habilitar cualquier ajuste que no sea Diagnósticos de arranque requiere que el agente de la máquina virtual de Azure esté instalado en la máquina virtual antes de la conmutación por error.

## <a name="unexpected-shutdown-message-event-id-6008"></a>Mensaje de cierre inesperado (Id. de evento 6008)

Al arrancar una máquina tras la conmutación por error, si recibe un mensaje de cierre inesperado sobre la máquina virtual recuperada, esto indica que el estado de cierre no se capturó en el punto de recuperación usado para la conmutación por error. Esto ocurre al recuperarse hasta un punto en el que la máquina virtual no se había cerrado completamente.

Esto no suele ser motivo de preocupación y normalmente puede omitirse en las conmutaciones por error no planeadas. En el caso de una conmutación por error planeada, asegúrese de que la máquina virtual se cierra correctamente antes de la conmutación por error y proporcione tiempo suficiente para que los datos de replicación pendientes locales se envíen a Azure. A continuación, use la opción **Más reciente** de la [pantalla Conmutación por error](site-recovery-failover.md#run-a-failover) para que los datos pendientes de Azure se procesen en un punto de recuperación, el cual se utiliza para la conmutación por error de la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes
- Solucionar problemas de [conexión RDP con una máquina virtual Windows](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Solucionar problemas de [conexión SSH a una máquina virtual Linux](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Si necesita más ayuda, publique la consulta en el [foro de Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) o deje un comentario al final de este documento. Tenemos una comunidad activa que debería poder ayudarle.
