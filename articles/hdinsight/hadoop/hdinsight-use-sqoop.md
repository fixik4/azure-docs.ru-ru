---
title: Выполнение заданий Apache Sqoop с помощью Azure HDInsight в Apache Hadoop
description: Вы узнаете, как использовать Azure PowerShell с рабочей станции для запуска Sqoop, импорта и экспорта между кластером HDInsight и базой данных Azure SQL.
ms.reviewer: jasonh
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.openlocfilehash: 9351ffabd1a263de1ff262fe5f6fef48be518836
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446204"
---
# <a name="use-apache-sqoop-with-hadoop-in-hdinsight"></a>Использование Apache Sqoop с Hadoop в HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Узнайте, как использовать Apache Sqoop в HDInsight для импорта и экспорта между кластером HDInsight и базой данных SQL Azure или базой данных SQL Server.

Хотя Apache Hadoop оптимально подходит для обработки частично структурированных и неструктурированных данных, таких как журналы и файлы, возможно, возникнет необходимость обрабатывать структурированные данные, хранящиеся в реляционных базах данных.

[Apache Sqoop][sqoop-user-guide-1.4.4] — это средство, предназначенное для передачи данных между кластерами Hadoop и реляционными базами данных. С его помощью можно импортировать данные из системы управления реляционной базой данных (реляционной СУБД), например SQL Server, MySQL или Oracle, в распределенную файловую систему Hadoop (HDFS), преобразовать данные в системе Hadoop с использованием MapReduce или Apache Hive, а затем экспортировать данные обратно в реляционную СУБД. В этом учебнике в качестве реляционной базы данных используется база данных SQL.

Сведения о версиях Sqoop, поддерживаемых в кластерах HDInsight, см. в статье о [новых возможностях в версиях кластеров HDInsight][hdinsight-versions].

## <a name="understand-the-scenario"></a>Ознакомление со сценарием

Кластер HDInsight имеет несколько примеров данных. Вы будете использовать два следующих образца:

* Файл журнала Apache Log4j, расположенный в */example/data/sample.log*. Из файла извлекаются следующие журналы:
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* Таблица Hive с именем *hivesampletable*, которая ссылается на файл данных, расположенный в */hive/warehouse/hivesampletable*. Эта таблица содержит некоторые данные о мобильных устройствах. 
  
  | Поле | Тип данных |
  | --- | --- |
  | clientid |строка |
  | querytime |строка |
  | market |строка |
  | deviceplatform |строка |
  | devicemake |строка |
  | devicemodel |строка |
  | state |строка |
  | country |строка |
  | querydwelltime |Double |
  | sessionid |bigint |
  | sessionpagevieworder |bigint |

В данном руководстве используются эти два набора данных для тестирования импорта и экспорта Sqoop.

## <a name="create-cluster-and-sql-database"></a>Создание кластера и базы данных SQL
В этом разделе показано, как создать кластер, Базу данных SQL и ее схемы для выполнения заданий руководства с помощью портала Azure и шаблона Azure Resource Manager. Шаблон можно найти в [шаблонов быстрого запуска Azure](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). Шаблон Resource Manager вызывает пакет BACPAC для развертывания схем таблиц в базе данных SQL.  Пакет BACPAC хранится в общедоступном контейнере больших двоичных объектов (https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac). Чтобы использовать закрытый контейнер для BACPAC-файлов, используйте следующие значения в шаблоне:
   
```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

Сведения о создании кластера и Базы данных SQL с помощью Azure PowerShell см. в [приложении A](#appendix-a---a-powershell-sample).

> [!NOTE]  
> С помощью шаблона или портала Azure можно импортировать только BACPAC-файл из хранилища BLOB-объектов Azure.

**Настройка среды с помощью шаблона Resource Manager**
1. Щелкните следующее изображение, чтобы открыть шаблон Resource Manager на портале Azure.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
2. Введите следующие свойства.

    - **Подписка**: Введите подписку Azure.
    - **Группа ресурсов**. Создайте новую группу ресурсов Azure или выберите существующую.  Группа ресурсов нужна для управления.  Это контейнер для объектов.
    - **Расположение**. Выберите регион.
    - **Имя кластера**. Введите имя кластера Hadoop.
    - **Имя для входа и пароль для кластера**. Имя для входа по умолчанию — admin.
    - **Имя пользователя SSH и пароль**.
    - **Имя для входа на сервер базы данных SQL Azure и пароль**.
    - **Расположение артефактов**. Используйте значение по умолчанию, если вы не хотите использовать собственный bacpac-файл в другом месте.
    - **Маркер SAS расположения артефактов**. Оставьте пустым.
    - **Имя BACPAC-файла**. Используйте значение по умолчанию, если вы не хотите использовать собственный bacpac-файл.
     
        В разделе переменных жестко заданы указанные далее значения.
        
        |ИМЯ|Значение|
        |----|-----|
        | Имя учетной записи хранения по умолчанию | &lt;ClusterName>store |
        | Имя сервера базы данных SQL Azure | &lt;ClusterName>dbserver |
        | Имя базы данных SQL Azure | &lt;ClusterName>db |
     
3. Выберите **Я принимаю указанные выше условия**.
4. Щелкните **Приобрести**. Появится новый элемент под названием "Выполняется отправка развертывания для развертывания шаблона". Процесс создания кластера и базы данных SQL занимает около 20 минут.

Если вы решили использовать существующую базу данных SQL Azure или Microsoft SQL Server

* **База данных SQL Azure**. Для сервера базы данных SQL Azure необходимо настроить правило брандмауэра, чтобы разрешить доступ с рабочей станции. Инструкции по созданию базы данных Azure SQL и настройке брандмауэра см. в статье [Начало работы с серверами баз данных SQL Azure, базами данных и правилами брандмауэра с использованием портала Azure и SQL Server Management Studio][sqldatabase-get-started]. 
  
  > [!NOTE]  
  > По умолчанию в базе данных SQL Azure разрешены подключения из служб Azure, таких как Azure HDInsight. Если этот параметр брандмауэра отключен, его нужно включить на портале Azure. Инструкции по созданию базы данных SQL Azure и настройке правил брандмауэра см. в статье [Начало работы с серверами баз данных SQL Azure, базами данных и правилами брандмауэра с использованием портала Azure и SQL Server Management Studio][sqldatabase-create-configure].

* **SQL Server**. Если ваш кластер HDInsight находится в той же виртуальной сети Azure, что и SQL Server, эта статья поможет вам разобраться с импортом данных в базу данных SQL Server и их экспортом из нее.
  
  > [!NOTE]  
  > HDInsight поддерживает только географически привязанные виртуальные сети и на данный момент не работает с виртуальными сетями на основе территориальных групп.

  
  * Информацию по созданию и настройке виртуальной сети см. в статье [Создание виртуальной сети с помощью портала Azure](../../virtual-network/quick-create-portal.md).
    
    * Если SQL Server используется в собственном центре обработки данных, тип соединения в виртуальной сети необходимо настроить как *сеть — сеть* или *точка — сеть*.
      
      > [!NOTE]  
      > Для виртуальных сетей с типом соединения **точка — сеть** на сервере SQL Server должно быть запущено приложение настройки VPN-клиента, которое доступно на **панели мониторинга** конфигурации виртуальной сети Azure.


    * При использовании SQL Server на виртуальной машине Azure можно использовать любую конфигурацию виртуальной сети, если виртуальная машина с SQL Server находится в той же виртуальной сети, что и HDInsight.
  * Сведения о создании кластера HDInsight в виртуальной сети см. в статье [Создание кластеров Apache Hadoop под управлением Windows в HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
    
    > [!NOTE]  
    > Кроме того, SQL Server должен разрешать проверку подлинности. Для выполнения шагов, описанных в этой статье, вам понадобится использовать данные для входа на SQL Server.


**Проверка конфигурации**

1. На портале Azure откройте группу ресурсов. В группе отобразится четыре ресурса:

    - кластер;
    - сервер базы данных;
    - база данных;
    - учетная запись хранения по умолчанию.

2. Откройте базу данных в Microsoft SQL Server Management Studio.  Должны отобразиться две развернутые базы данных:

    ![Azure HDInsight Sqoop SQL Management Studio](./media/hdinsight-use-sqoop/hdinsight-sqoop-sql-management-studio.png)


## <a name="run-sqoop-jobs"></a>Выполнение заданий Sqoop
HDInsight может выполнять задания Sqoop с помощью различных методов. Используйте следующую таблицу, чтобы решить, какой метод подходит вам, а затем перейдите по ссылке к пошаговому руководству.

| **Используйте** , если требуется... | ... **интерактивная** оболочка | ...**пакетная** обработка | ...with этим **кластером операционной системы** | ...из этого **кластера операционной системы** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-sqoop-mac-linux.md) |? |? |Linux |Linux, Unix, Mac OS X или Windows |
| [.NET SDK для Hadoop](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |? |Windows или Linux |Windows (сейчас) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |? |Windows или Linux | Windows |

## <a name="limitations"></a>Ограничения
* Массовый экспорт: при использовании HDInsight на основе Linux соединитель Sqoop, применяемый для экспорта данных в Microsoft SQL Server или базу данных SQL Azure, пока не поддерживает операции массовой вставки.
* Пакетная обработка: при использовании HDInsight на основе Linux, когда для вставок применяется параметр `-batch`, Sqoop выполняет несколько вставок вместо пакетной обработки операций вставки.

## <a name="next-steps"></a>Дальнейшие действия
Теперь вы узнали, как использовать Sqoop. Дополнительные сведения см. на следующих ресурсах:

* [Использование Hive и HiveQL с Hadoop в HDInsight для анализа примера файла Apache log4j](../hdinsight-use-hive.md)
* [Использование Pig с Hadoop в HDInsight](../hdinsight-use-pig.md)
* [Отправка данных в HDInsight][hdinsight-upload-data]. Узнайте о других способах отправки данных в HDInsight или хранилище BLOB-объектов Azure.

## <a name="appendix-a---a-powershell-sample"></a>Приложение А. Пример PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

В этом примере PowerShell выполняются приведенные ниже действия.

1. Подключение к Azure.
2. Создание группы ресурсов Azure. Дополнительные сведения см. в статье [Использование Azure PowerShell с Azure Resource Manager](../../azure-resource-manager/manage-resource-groups-powershell.md).
3. Создание сервера базы данных SQL Azure, базы данных SQL Azure и двух таблиц. 
   
    Если же вы используете SQL Server, воспользуйтесь следующими инструкциями, чтобы создать таблицы:
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    Проверить базу данных и таблицы проще всего с помощью Visual Studio. Сервер базы данных и базу данных можно проверить с помощью портала Azure.
4. Создание кластера HDInsight.
   
    Для проверки кластера можно использовать портал Azure или Azure PowerShell.
5. Выполнение предварительной обработки исходного файла данных.
   
    В этом руководстве вы экспортируете файл журнала log4j (файл с разделителями) и таблицу Hive в базу данных SQL Azure. Файл с разделителями — */example/data/sample.log*. Ранее в учебнике вы видели несколько примеров журналов log4j. В файле журнала есть несколько пустых строк и несколько строк, похожих на следующие:
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    Это нормально для других примеров, которые используют эти данные, но для импорта в базу данных SQL Azure или на SQL Server нам необходимо удалить такие исключения. Экспорт Sqoop не будет выполнен, если присутствует пустая строка или строка, число элементов в которой меньше числа полей, определенных в таблице базы данных SQL Azure. Таблица log4jlogs содержит семь полей типа "строка".
   
    Данная процедура создает новый файл в кластере: tutorials/usesqoop/data/sample.log. Чтобы проанализировать измененный файл данных, вы можете использовать портал Azure, средство обозревателя службы хранилища Azure или Azure PowerShell. В статье [Руководство по Hadoop. Начало работы с Hadoop в HDInsight на платформе Linux][hdinsight-get-started] есть пример кода, где Azure PowerShell используется для скачивания файла и отображения его содержимого.
6. Экспортирование файла данных в базу данных SQL Azure.
   
    Исходный файл: tutorials/usesqoop/data/sample.log. Таблица, в которую экспортируются данные, называется log4jlogs.
   
   > [!NOTE]  
   > Кроме информации в строке соединения, описанные в этом разделе шаги должны подходить как для базы данных SQL Azure, так и для SQL Server. Эти шаги были протестированы для следующей конфигурации:
   > 
   > * **Конфигурация виртуальной сети Azure типа "точка — сеть"**. Кластер HDInsight, подключенный через виртуальную сеть к SQL Server в частном центре обработки данных. Дополнительные сведения см. в статье [Настройка подключения типа "точка — сеть" к виртуальной сети с помощью классического портала](../../vpn-gateway/vpn-gateway-point-to-site-create.md).
   > * **Azure HDInsight**. См. дополнительные сведения о [создании кластеров Hadoop в HDInsight с помощью пользовательских параметров](../hdinsight-hadoop-provision-linux-clusters.md).
   > * **SQL Server 2014**. Настроенный для разрешения проверки подлинности и использующий пакет настройки VPN-клиента для надежного подключения к виртуальной сети.
   > 
   > 
7. Экспортирование таблицы Hive в базу данных SQL Azure.
8. Импортирование таблицы mobiledata в кластер HDInsight.
   
    Чтобы проанализировать измененный файл данных, вы можете использовать портал Azure, средство обозревателя службы хранилища Azure или Azure PowerShell.  В статье [Руководство по Hadoop. Начало работы с Hadoop в HDInsight на платформе Linux][hdinsight-get-started] есть пример кода, где Azure PowerShell используется для скачивания файла и отображения его содержимого.

### <a name="the-powershell-sample"></a>Пример PowerShell

```powershell
# Prepare an Azure SQL database to be used by the Sqoop tutorial

#region - provide the following values

$subscriptionID = "<Enter your Azure Subscription ID>"

$sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
$sqlDatabasePassword = "<Enter a Password>"

$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = "<Enter a Password>"

$sshUserName = "sshuser" #HDInsight ssh username
$sshPassword = $httpPassword 

# used for creating Azure service names
$nameToken = "<Enter an alias>" 
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
#endregion

#region - variables

# Resource group variables
$resourceGroupName = $namePrefix + "rg"
$location = "East US 2" # used by all Azure services defined in this tutorial

# SQL database varialbes
$sqlDatabaseServerName = $namePrefix + "sqldbserver"
$sqlDatabaseName = $namePrefix + "sqldb"
$sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
$sqlDatabaseMaxSizeGB = 10

# Used for retrieving external IP address and creating firewall rules
$ipAddressRestService = "https://bot.whatismyipaddress.com"
$fireWallRuleName = "UseSqoop"

# Used for creating tables and clustered indexes
$cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
    [t1] [nvarchar](50),
    [t2] [nvarchar](50),
    [t3] [nvarchar](50),
    [t4] [nvarchar](50),
    [t5] [nvarchar](50),
    [t6] [nvarchar](50),
    [t7] [nvarchar](50))"

$cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

$cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
[clientid] [nvarchar](50),
[querytime] [nvarchar](50),
[market] [nvarchar](50),
[deviceplatform] [nvarchar](50),
[devicemake] [nvarchar](50),
[devicemodel] [nvarchar](50),
[state] [nvarchar](50),
[country] [nvarchar](50),
[querydwelltime] [float],
[sessionid] [bigint],
[sessionpagevieworder][bigint])"

$cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

# HDInsight variables
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName
#endregion

# Treat all errors as terminating
$ErrorActionPreference = "Stop"

#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzContext}
catch{Connect-AzAccount}
#endregion

#region - Create Azure resource group
Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
try{
    Get-AzResourceGroup -Name $resourceGroupName
}
catch{
    New-AzResourceGroup -Name $resourceGroupName -Location $location
}
#endregion

#region - Create Azure SQL database server
Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
try{
    Get-AzSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
catch{
    Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

    $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

    $sqlDatabaseServerName = (New-AzSqlServer `
                                -ResourceGroupName $resourceGroupName `
                                -ServerName $sqlDatabaseServerName `
                                -SqlAdministratorCredentials $credential `
                                -Location $location).ServerName
    Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

    Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
    $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
    New-AzSqlServerFirewallRule `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -FirewallRuleName "$fireWallRuleName-workstation" `
        -StartIpAddress $workstationIPAddress `
        -EndIpAddress $workstationIPAddress

    #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
    #Note that this allows Azure traffic from any Azure subscription to access the server.
    New-AzSqlServerFirewallRule `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -FirewallRuleName "$fireWallRuleName-Azureservices" `
        -StartIpAddress "0.0.0.0" `
        -EndIpAddress "0.0.0.0"
}

#endregion

#region - Create and validate Azure SQL database
Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

try {
    Get-AzSqlDatabase `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -DatabaseName $sqlDatabaseName
}
catch {
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    New-AzSqlDatabase `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -DatabaseName $sqlDatabaseName `
        -Edition "Standard" `
        -RequestedServiceObjectiveName "S1"
}

#endregion

#region - Create tables
Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green

$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = $sqlDatabaseConnectionString
$conn.Open()

# Create the log4jlogs table and index
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.Connection = $conn
$cmd.CommandText = $cmdCreateLog4jTable
$ret = $cmd.ExecuteNonQuery()
$cmd.CommandText = $cmdCreateLog4jClusteredIndex
$cmd.ExecuteNonQuery()

# Create the mobiledata table and index
$cmd.CommandText = $cmdCreateMobileTable
$cmd.ExecuteNonQuery()
$cmd.CommandText = $cmdCreateMobileDataClusteredIndex
$cmd.ExecuteNonQuery()

$conn.close()

#endregion


#region - Create HDInsight cluster

Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green

# Create the default storage account
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -Type Standard_LRS

# Create the default Blob container
$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
$defaultStorageAccountContext = New-AzStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey 
New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageAccountContext 

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $HDInsightClusterName `
    -Location $location `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version 3.6 `
    -ClusterSizeInNodes 2 `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $defaultBlobContainerName  

# Validate the cluster
Get-AzHDInsightCluster -ClusterName $hdinsightClusterName
#endregion

#region - pre-process the source file

Write-Host "Preprocessing the source file ..." -ForegroundColor Green

# This procedure creates a new file with $destBlobName
$sourceBlobName = "example/data/sample.log"
$destBlobName = "tutorials/usesqoop/data/sample.log"

# Define the connection string
$storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

# Create block blob objects referencing the source and destination blob.
$storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
$storageClient = $storageAccount.CreateCloudBlobClient();
$storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
$sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
$destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

# Define a MemoryStream and a StreamReader for reading from the source file
$stream = New-Object System.IO.MemoryStream
$stream = $sourceBlob.OpenRead()
$sReader = New-Object System.IO.StreamReader($stream)

# Define a MemoryStream and a StreamWriter for writing into the destination file
$memStream = New-Object System.IO.MemoryStream
$writeStream = New-Object System.IO.StreamWriter $memStream

# Pre-process the source blob
$exString = "java.lang.Exception:"
while(-Not $sReader.EndOfStream){
    $line = $sReader.ReadLine()
    $split = $line.Split(" ")

    # remove the "java.lang.Exception" from the first element of the array
    # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
    if ($split[0] -eq $exString){
        #create a new ArrayList to remove $split[0]
        $newArray = [System.Collections.ArrayList] $split
        $newArray.Remove($exString)

        # update $split and $line
        $split = $newArray
        $line = $newArray -join(" ")
    }

    # remove the lines that has less than 7 elements
    if ($split.count -ge 7){
        write-host $line
        $writeStream.WriteLine($line)
    }
}

# Write to the destination blob
$writeStream.Flush()
$memStream.Seek(0, "Begin")
$destBlob.UploadFromStream($memStream)

#endregion

#region - export a log file from the cluster to the SQL database

Write-Host "Preprocessing the source file ..." -ForegroundColor Green

$tableName_log4j = "log4jlogs"

# Connection string for Azure SQL Database.
# Comment if using SQL Server
$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
# Connection string for SQL Server.
# Uncomment if using SQL Server.
#$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

$exportDir_log4j = "/tutorials/usesqoop/data"

# Submit a Sqoop job
$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose
Wait-AzHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
Write-Host "Standard Output" -BackgroundColor Green
Get-AzHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

#endregion

#region - export a Hive table

$tableName_mobile = "mobiledata"
$exportDir_mobile = "/hive/warehouse/hivesampletable"

$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput

#endregion

#region - import a database

$targetDir_mobile = "/tutorials/usesqoop/importeddata/"

$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput

#endregion
```


[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: ../hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../../sql-database/sql-database-get-started.md
[sqldatabase-create-configure]: ../../sql-database/sql-database-get-started.md

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
