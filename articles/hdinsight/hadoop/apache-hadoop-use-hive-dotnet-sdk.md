---
title: 'Ejecución de consultas de Apache Hive mediante el SDK de HDInsight para .NET: Azure'
description: Obtenga información sobre cómo enviar trabajos de Apache Hadoop a HDInsight Apache Hadoop de Azure mediante el SDK de .NET de HDInsight.
ms.reviewer: jasonh
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: d3c9c8eab0ce13aee5dbe7bdb732d45555e547fb
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407174"
---
# <a name="run-apache-hive-queries-using-hdinsight-net-sdk"></a>Ejecución de consultas de Apache Hive mediante el SDK de .NET para HDInsight
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Obtenga información sobre cómo enviar consultas de Apache Hive mediante el SDK de .NET para HDInsight. Escriba un programa C# para enviar una consulta de Hive para enumerar tablas de Hive y mostrar los resultados.

> [!NOTE]  
> Los pasos de este artículo deben realizarse desde un cliente de Windows. Para obtener información sobre cómo usar un cliente Linux, OS X o Unix para trabajar con Hive, utilice el selector de pestañas que se muestra en la parte superior del artículo.

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar este artículo, debe tener los siguientes elementos:

* **Un clúster de Apache Hadoop en HDInsight**. Consulte [Tutorial de Hadoop: Introducción al uso de Hadoop en HDInsight basado en Linux](apache-hadoop-linux-tutorial-get-started.md).

    > [!WARNING]  
    > A partir del 15 de septiembre de 2017, el SDK de .NET para HDInsight solo admite devolver resultados de consulta de Hive desde cuentas de Azure Storage. Si usa este ejemplo con un clúster de HDInsight que utilice Azure Data Lake Storage como almacenamiento principal, no se pueden recuperar resultados de búsqueda mediante .NET SDK.

* **Visual Studio 2013/2015/2017**.

## <a name="run-a-hive-query"></a>Ejecución de una consulta de Hive
El SDK .NET de HDInsight ofrece bibliotecas de cliente .NET que facilitan el trabajo con los clústeres de HDInsight de .NET. 

**Para enviar trabajos**

1. Cree una aplicación de consola en C# mediante Visual Studio.
2. En la Consola del Administrador de paquetes NuGet, ejecute el siguiente comando:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Use el código siguiente:

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
                
                // Only Azure Storage accounts are supported by the SDK
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. Presione **F5** para ejecutar la aplicación.

El resultado de la aplicación será similar a la imagen siguiente:

![Salida de trabajo de Hive para Hadoop en HDInsight](./media/apache-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido varias maneras de crear un clúster de HDInsight. Para obtener más información, consulte los artículos siguientes:

* [Introducción a Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Creación de clústeres de Apache Hadoop en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Administración de clústeres de Apache Hadoop en HDInsight mediante Azure Portal](../hdinsight-administer-use-management-portal.md)
* [Referencia del SDK de .NET de HDInsight](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)
* [Uso de Apache Pig con HDInsight](hdinsight-use-pig.md)
* [Uso de Apache Sqoop con HDInsight](apache-hadoop-use-sqoop-mac-linux.md)
* [Crear aplicaciones .NET para HDInsight de autenticación no interactiva](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
 


