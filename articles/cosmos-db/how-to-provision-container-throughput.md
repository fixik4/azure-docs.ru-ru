---
title: Подготовка пропускной способности контейнера в Azure Cosmos DB
description: Узнайте о том, как подготовить пропускную способность на уровне контейнера в Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 28060637db47b42db66f706815066d498032ec11
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258723"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Подготовка пропускной способности для контейнера Azure Cosmos

В этой статье описывается, как подготовить пропускную способность для контейнера (коллекции, графы или таблицы) в Azure Cosmos DB. Вы можете подготовить пропускную способность для одного контейнера или [для базы данных](how-to-provision-database-throughput.md) и предоставить к ней общий доступ между контейнерами. Пропускную способность контейнера можно подготовить с помощью портала Azure, интерфейса командной строки Azure или пакетов SDK Azure Cosmos DB.

## <a name="provision-throughput-by-using-azure-portal"></a>Подготовка пропускной способности с помощью портала Azure

1. Войдите на [портале Azure](https://portal.azure.com/).

1. [Создайте новую учетную запись Azure Cosmos DB](create-sql-api-dotnet.md#create-account) или выберите имеющуюся.

1. Откройте панель **Обозреватель данных** и выберите **Новая коллекция**. После этого предоставьте следующие сведения.

   * Укажите, создаете ли вы новую базу данных или используете существующую.
   * Введите идентификатор коллекции (таблицы или графа).
   * Введите значение ключа секции (например, `/userid`).
   * Укажите пропускную способность (например, 1000 ЕЗ/с).
   * Нажмите кнопку **ОК**.

![Снимок экрана обозревателя данных с выделенным элементом "Новая коллекция"](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-azure-cli"></a>Подготовка пропускной способности с помощью Azure CLI

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

При подготовке пропускной способности для учетной записи Azure Cosmos DB, настроенной с API Azure Cosmos DB для MongoDB, используйте `/myShardKey` для пути к ключу секции. При подготовке пропускной способности для учетной записи Azure Cosmos DB, настроенной с API Cassandra, используйте `/myPrimaryKey` для пути к ключу секции.

## <a name="provision-throughput-by-using-net-sdk"></a>Подготовка пропускной способности с помощью пакета SDK для .NET

> [!Note]
> Используйте SQL API для подготовки пропускной способности для всех API, за исключением API Cassandra.

### <a id="dotnet-most"></a>Интерфейсы API SQL, MongoDB, Gremlin и API таблиц

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>API Cassandra

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Дополнительная информация

Чтобы узнать о подготовке пропускной способности в Cosmos DB, обратитесь к следующим статьям:

* [Provision throughput for a database in Azure Cosmos DB](how-to-provision-database-throughput.md) (Подготовка пропускной способности для базы данных в Azure Cosmos DB)
* [Пропускная способность и единицы запросов в Azure Cosmos DB](request-units.md)
