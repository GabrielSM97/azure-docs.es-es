---
title: archivo de inclusión
description: archivo de inclusión
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 05/24/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: e431e7c45dc2cb41db5b9fa762abc908cc76b375
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665297"
---
## <a name="create-a-new-cdn-profile"></a>Crear un nuevo perfil de CDN

Un perfil de CDN es un contenedor para los puntos de conexión de CDN y especifica un plan de tarifa.

1. En Azure Portal, en la parte superior izquierda, seleccione **Crear un recurso**. 
    
    Aparece el panel **Nuevos**.
   
2. Seleccione **Web y móvil**, y después **CDN**.
   
    ![Seleccionar recurso de CDN](./media/cdn-create-profile/cdn-new-resource.png)

    Aparece el panel **Perfil de CDN**.

3. Para la configuración del perfil de CDN, utilice los valores especificados en la tabla siguiente:
   
    | Configuración  | Valor |
    | -------- | ----- |
    | **Name** | Escriba *my-cdn-profile-123* como nombre de perfil. Este nombre debe ser único globalmente; si ya está en uso, puede especificar otro diferente. |
    | **Suscripción** | Seleccione una suscripción de Azure en la lista desplegable. |
    | **Grupos de recursos** | Seleccione **Crear nuevo** y escriba *my-resource-group-123* y un nombre para el nuevo grupo de recursos. Este nombre debe ser único globalmente. Si ya está en uso, puede especificar un nombre diferente o puede seleccionar **Usar existente** y seleccionar **my-resource-group-123** en la lista desplegable. | 
    | **Ubicación del grupo de recursos** | Seleccione **Centro de EE. UU.** en la lista desplegable. |
    | **Plan de tarifa** | Seleccione **Verizon estándar** en la lista desplegable. |
    | **Crear un punto de conexión de CDN ahora** | Déjelo sin seleccionar. |  
   
    ![Nuevo perfil de CDN](./media/cdn-create-profile/cdn-new-profile.png)

4. Seleccione **Anclar al panel** para guardar el perfil en el panel después de crearlo.
    
5. Seleccione **Crear** para crear el perfil. 

    Solo en los perfiles de **Azure CDN Estándar de Microsoft**, la operación se completa normalmente en dos horas. 

