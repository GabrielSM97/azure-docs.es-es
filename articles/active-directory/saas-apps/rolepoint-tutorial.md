---
title: 'Tutorial: Integración de Azure Active Directory con RolePoint | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y RolePoint.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 68d37f40-15da-45f5-a9e1-d53f78e786d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4997dc906344054ccb45b8eb152a49329bb26e55
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56197698"
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a>Tutorial: Integración de Azure Active Directory con RolePoint

En este tutorial, aprenderá a integrar RolePoint con Azure Active Directory (Azure AD).

La integración de RolePoint con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a RolePoint.
- Puede permitir que los usuarios inicien sesión automáticamente en RolePoint (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con RolePoint, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en RolePoint.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de RolePoint desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-rolepoint-from-the-gallery"></a>Adición de RolePoint desde la galería
Para configurar la integración de RolePoint en Azure AD, deberá agregar RolePoint desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar RolePoint desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **RolePoint**.

    ![Creación de un usuario de prueba de Azure AD](./media/rolepoint-tutorial/tutorial_rolepoint_search.png)

1. En el panel de resultados, seleccione **RolePoint** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/rolepoint-tutorial/tutorial_rolepoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con RolePoint con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de RolePoint para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de RolePoint.

Esta relación de vínculo se establece asignando el valor de **nombre de usuario** de Azure AD como el valor de **nombre de usuario** de RolePoint.

Para configurar y probar el inicio de sesión único de Azure AD con RolePoint, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de RolePoint](#creating-a-rolepoint-test-user)**: el objetivo es tener un homólogo de Britta Simon en RolePoint que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación RolePoint.

**Para configurar el inicio de sesión único de Azure AD con RolePoint, realice los pasos siguientes:**

1. En la página de integración de la aplicación **RolePoint** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/rolepoint-tutorial/tutorial_rolepoint_samlbase.png)

1. En la sección **Dominio y direcciones URL de RolePoint**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/rolepoint-tutorial/tutorial_rolepoint_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.rolepoint.com/login`.
    
    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://app.rolepoint.com/<instancename>`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Aquí se recomienda usar el valor de cadena único en el identificador. Póngase en contacto con el [equipo de soporte técnico de RolePoint](mailto:info@rolepoint.com) para obtener el valor. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/rolepoint-tutorial/tutorial_rolepoint_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/rolepoint-tutorial/tutorial_general_400.png)


1. Para configurar el inicio de sesión único en **RolePoint**, debe enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de RolePoint](mailto:info@rolepoint.com).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/rolepoint-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/rolepoint-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/rolepoint-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/rolepoint-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-rolepoint-test-user"></a>Creación de un usuario de prueba de RolePoint

En esta sección, se crea un usuario denominado Britta Simon en RolePoint. Trabaje con el [equipo de soporte técnico de RolePoint](mailto:info@rolepoint.com) para agregar los usuarios a la plataforma RolePoint.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a RolePoint.

![Asignar usuario][200] 

**Para asignar Britta Simon a RolePoint, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **RolePoint**.

    ![Configurar inicio de sesión único](./media/rolepoint-tutorial/tutorial_rolepoint_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de RolePoint en el panel de acceso, debería iniciar sesión automáticamente en su aplicación de RolePoint. 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rolepoint-tutorial/tutorial_general_01.png
[2]: ./media/rolepoint-tutorial/tutorial_general_02.png
[3]: ./media/rolepoint-tutorial/tutorial_general_03.png
[4]: ./media/rolepoint-tutorial/tutorial_general_04.png

[100]: ./media/rolepoint-tutorial/tutorial_general_100.png

[200]: ./media/rolepoint-tutorial/tutorial_general_200.png
[201]: ./media/rolepoint-tutorial/tutorial_general_201.png
[202]: ./media/rolepoint-tutorial/tutorial_general_202.png
[203]: ./media/rolepoint-tutorial/tutorial_general_203.png

