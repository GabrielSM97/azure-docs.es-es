---
title: 'Información sobre cómo el entorno de ejecución administra los dispositivos: Azure IoT Edge | Microsoft Docs'
description: Obtenga información sobre cómo el entorno de ejecución de Azure IoT Edge administra los módulos, la seguridad, la comunicación y los informes sobre los dispositivos.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a2412a286015cb403fe9a2af7754c7e5346fe98c
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54230431"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Información del entorno de ejecución de Azure IoT Edge y su arquitectura

El entorno de ejecución de IoT Edge es una colección de programas que deben instalarse en un dispositivo para que se considere un dispositivo IoT Edge. Colectivamente, los componentes de la instancia de IoT Edge en tiempo de ejecución permiten que los dispositivos IoT Edge reciban el código para ejecutarse en el perímetro y comunican los resultados. 

La instancia de IoT Edge en tiempo de ejecución realiza las siguientes funciones en los dispositivos IoT Edge:

* Instala y actualiza cargas de trabajo en el dispositivo.
* Mantiene los estándares de seguridad de Azure IoT Edge en el dispositivo.
* Garantiza que los [módulos de IoT Edge](iot-edge-modules.md) estén siempre en ejecución.
* Informa del estado de los módulos a la nube para realizar la supervisión remota.
* Facilita la comunicación entre los dispositivos de hoja de nivel inferior y los dispositivos IoT Edge.
* Facilita la comunicación entre los módulos y el dispositivo IoT Edge.
* Facilita la comunicación entre el dispositivo IoT Edge y la nube.

![El entorno de ejecución comunica la información y el estado del módulo a IoT Hub](./media/iot-edge-runtime/Pipeline.png)

Las responsabilidades del entorno de ejecución de IoT Edge se dividen en dos categorías: comunicación y administración de los módulos. Estas dos funciones las realizan dos componentes que constituyen la instancia en tiempo de ejecución de IoT Edge. El centro de IoT Edge es responsable de la comunicación, mientras que el agente de IoT Edge administra la implementación y supervisión de los módulos. 

El agente y el centro de IoT Edge son módulos, como cualquier otro que se ejecuta en un dispositivo de IoT Edge. 

## <a name="iot-edge-hub"></a>Centro de IoT Edge

El centro de IoT Edge es uno de los dos módulos que componen el entorno de ejecución de Azure IoT Edge. Actúa como un proxy local de IoT Hub exponiendo los mismos puntos de conexión de protocolo que IoT Hub. Esta coherencia significa que los clientes (dispositivos o módulos) pueden conectarse a la instancia de IoT Edge en tiempo de ejecución de la misma forma que lo harían a IoT Hub. 

>[!NOTE]
> El centro de IoT Edge admite clientes que se conectan mediante MQTT o AMQP. No admite los clientes que usan HTTP. 

El centro de IoT Edge no se trata de una versión completa del centro de IoT Hub que se ejecuta localmente. Hay algunos procesos que el centro de IoT Edge delega silenciosamente en IoT Hub. Por ejemplo, el centro de IoT Edge reenvía las solicitudes de autenticación a IoT Hub la primera vez que un dispositivo trata de conectarse. Una vez establecida la primera conexión, el centro de IoT Edge almacena la información de seguridad en caché localmente. Las conexiones posteriores desde dicho dispositivo se permiten sin tener que autenticarse en la nube. 

>[!NOTE]
>El entorno de tiempo de ejecución debe estar conectado cada vez que trata de autenticar un dispositivo.

Para reducir el ancho de banda que usa la solución IoT Edge, el centro de IoT Edge optimiza el número real de conexiones a la nube. El centro de IoT Edge toma las conexiones lógicas de clientes, como módulos o dispositivos de hoja, y las combina para crear una sola conexión física a la nube. Los detalles de este proceso son transparentes para el resto de la solución. Los clientes creen que tienen su propia conexión a la nube, aunque todos los datos van a enviarse a través de la misma. 

![El centro de IoT Edge es una puerta de enlace entre los dispositivos físicos e IoT Hub](./media/iot-edge-runtime/Gateway.png)

 El centro de IoT Edge puede determinar si está conectado a IoT Hub. Si se pierde la conexión, el centro de IoT Edge guarda los mensajes o las actualizaciones gemelas localmente. Una vez que se vuelva a establecer una conexión, se sincronizan todos los datos. La ubicación que usa esta caché temporal viene determinada por una propiedad del módulo gemelo del centro de IoT Edge. El tamaño de la caché no está limitado y aumentará siempre y cuando el dispositivo tenga capacidad de almacenamiento. 

### <a name="module-communication"></a>Comunicación entre módulos

 El centro de IoT Edge facilita la comunicación entre módulos. Al usar el centro de IoT Edge como un agente de mensajes, los módulos seguirán siendo independientes entre sí. Los módulos solo necesitan especificar las entradas en las que aceptan mensajes y las salidas en las que los escriben. Después, un desarrollador de soluciones une estas entradas y salidas para que los módulos procesen los datos en el orden específico de esa solución. 

![El centro de IoT Edge facilita la comunicación entre módulos.](./media/iot-edge-runtime/module-endpoints.png)

Para enviar datos al centro de IoT Edge, un módulo llama al método SendEventAsync. El primer argumento especifica en qué salida enviará el mensaje. El siguiente seudocódigo envía un mensaje a la salida output1:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Para recibir un mensaje, registre una devolución de llamada que procese los mensajes procedentes de una entrada específica. El siguiente seudocódigo registra la función messageProcessor, que se usará para procesar todos los mensajes recibidos en la entrada input1:

   ```csharp
   await client.SetInputMessageHandlerAsync(“input1”, messageProcessor, userContext);
   ```

Para más información sobre la clase ModuleClient y sus métodos de comunicación, consulte la referencia de API de su lenguaje preferido para el SDK: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C y Python](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable) o [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

El desarrollador de soluciones es responsable de especificar las reglas que determinan cómo el centro de IoT Edge pasa los mensajes entre los módulos. Las reglas de enrutamiento se definen en la nube y se envían al centro de IoT Edge de su dispositivo gemelo. Se utiliza la misma sintaxis de las rutas de IoT Hub para definir las rutas entre módulos de Azure IoT Edge. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Las rutas entre los módulos pasan por el centro de IoT Edge](./media/iot-edge-runtime/module-endpoints-with-routes.png)

## <a name="iot-edge-agent"></a>Agente de IoT Edge

El agente de IoT Edge es el otro módulo que constituye el entorno de ejecución de Azure IoT Edge. Es responsable de crear instancias de los módulos, lo que garantiza que continúen ejecutándose y notificando el estado de los módulos a IoT Hub. Al igual que cualquier otro módulo, el agente de IoT Edge usa su módulo gemelo para almacenar estos datos de configuración. 

El [demonio de seguridad de IoT Edge](iot-edge-security-manager.md) inicia el agente de IoT Edge durante el inicio del dispositivo. El agente recupera su módulo gemelo de IoT Hub e inspecciona el manifiesto de implementación. El manifiesto de implementación es un archivo JSON que declara al módulo que debe iniciarse. 

Cada elemento del manifiesto de implementación contiene información específica de un módulo y el agente de IoT Edge lo usa para controlar el ciclo de vida del módulo. Estas son algunas de las propiedades más interesantes: 

* **Settings.Image**: la imagen de contenedor que usa el agente de IoT Edge al iniciar el módulo. El agente de IoT Edge debe configurarse con las credenciales del registro de contenedor si la imagen está protegida por una contraseña. Las credenciales para el registro de contenedor se pueden configurar mediante el manifiesto de implementación de forma remota o en el propio dispositivo IoT Edge actualizando el archivo `config.yaml` en la carpeta del programa IoT Edge.
* **settings.createOptions**: una cadena que se pasa directamente al demonio de Docker cuando se inicia un contenedor del módulo. Al agregar las opciones de Docker en esta propiedad, se pueden obtener opciones avanzadas como reenvío de puertos o montaje de volúmenes en el contenedor de un módulo.  
* **status**: el estado en el que el agente de IoT Edge pone el módulo. Este valor se establece normalmente en *running*, ya que la mayoría de los usuarios quieren que el agente de IoT Edge inicie inmediatamente todos los módulos del dispositivo. Sin embargo, puede especificar que el estado inicial de un módulo sea stopped y esperar a indicar más adelante al agente de IoT Edge que inicie un módulo. El agente de IoT Edge notifica el estado de cada módulo a la nube en las propiedades notificadas. Una diferencia entre la propiedad deseada y la notificada es un indicador de un dispositivo con un comportamiento incorrecto. Los estados admitidos son los siguientes:
   * Downloading (Descargando)
   * En ejecución
   * Unhealthy (Incorrecto)
   * Con error
   * Stopped
* **restartPolicy**: cómo el agente de IoT Edge reinicia un módulo. Los valores posibles son:
   * Never: el agente de IoT Edge nunca reinicia el módulo.
   * onFailure: si se bloquea el módulo, el agente de IoT Edge lo reinicia. Si el módulo se cierra sin problemas, el agente de IoT Edge no lo reinicia.
   * Unhealthy: si el módulo se bloquea o se consideran incorrecto, el agente de IoT Edge lo reinicia.
   * Always: si el módulo se bloquea, se considera incorrecto o se apaga, el agente de IoT Edge lo reinicia. 

El agente de IoT Edge envía la respuesta de entorno de ejecución a IoT Hub. A continuación, se ofrece una lista de las posibles respuestas:
  * 200 - CORRECTO
  * 400 - La configuración de implementación tiene un formato incorrecto o no es válida.
  * 417 - El dispositivo no tiene un conjunto de configuración de implementación.
  * 412 - La versión del esquema de la configuración de implementación no es válida.
  * 406 - El dispositivo de IoT Edge está sin conexión o no envía informes de estado.
  * 500 - Error en el entorno en tiempo de ejecución de Azure IoT Edge.

### <a name="security"></a>Seguridad

El agente de IoT Edge desempeña un papel fundamental en la seguridad de un dispositivo IoT Edge. Por ejemplo, realiza acciones como comprobar la imagen de un módulo antes de iniciarlo. 

Para obtener más información sobre el marco de seguridad de Azure IoT Edge, consulte [Administrador de seguridad de IoT Edge](iot-edge-security-manager.md)

## <a name="next-steps"></a>Pasos siguientes

[Información sobre los certificados de Azure IoT Edge](iot-edge-certs.md)
