---
title: Tutorial de distribución global de Azure Cosmos DB para Table API
description: Aprenda a configurar la distribución global de Azure Cosmos DB con Table API.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 12/13/2017
ms.reviewer: sngun
ms.openlocfilehash: d7d68c2dbdf5ca32fb2936e92daafac838c97a06
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042537"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-table-api"></a>Configuración de la distribución global de Azure Cosmos DB con Table API

En este artículo se tratan las tareas siguientes: 

> [!div class="checklist"]
> * Configuración de la distribución global con Azure Portal
> * Configuración de la distribución global con [Table API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Conexión a una región de preferencia con Table API

Para aprovechar las ventajas de la [distribución global](distribute-data-globally.md), las aplicaciones cliente pueden especificar la lista del orden de preferencia de regiones que se usará para llevar a cabo operaciones de documentos. Puede hacerlo al establecer la propiedad [TableConnectionPolicy.PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.table.tableconnectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_CosmosDB_Table_TableConnectionPolicy_PreferredLocations). El SDK de Table API de Azure Cosmos DB elige el mejor punto de conexión para comunicarse en función de la configuración de la cuenta, la disponibilidad regional actual y la lista de preferencias proporcionada.

PreferredLocations debe contener una lista separada por comas de las ubicaciones preferidas (hospedaje múltiple) para las lecturas. Cada instancia de cliente puede especificar un subconjunto de estas regiones en el orden de preferencia para las lecturas de latencia baja. Estas regiones se deben denominar según sus [nombres para mostrar](https://msdn.microsoft.com/library/azure/gg441293.aspx), por ejemplo, `West US`.

Todas las lecturas se envían a la primera región disponible en la lista de PreferredLocations. Si se produce un error en la solicitud, el cliente pasará a la siguiente región de la lista y así sucesivamente.

El SDK intenta leer desde las regiones especificadas en PreferredLocations. Así que, por ejemplo, si la cuenta de base de datos está disponible en tres regiones, pero el cliente solo especifica dos de las regiones de no escritura para PreferredLocations, no se atenderá ninguna lectura desde la región de escritura, incluso en caso de conmutación por error.

El SDK envía automáticamente todas las escrituras a la región de escritura actual.

Si la propiedad PreferredLocations no está establecida, atenderá todas las solicitudes procedentes de la región de escritura actual.

De este modo finaliza este tutorial. Para información sobre cómo administrar la coherencia de la cuenta replicada globalmente, lea [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md). Para más información sobre cómo funciona la replicación global de bases de datos en Azure Cosmos DB, consulte [Distribución global de datos con Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha hecho lo siguiente:

> [!div class="checklist"]
> * Configuración de la distribución global con Azure Portal
> * Configurar la distribución global mediante Table API de Azure Cosmos DB

