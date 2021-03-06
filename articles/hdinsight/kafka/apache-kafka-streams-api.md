---
title: 'Руководство. Использование API Потоков Apache Kafka в Azure HDInsight '
description: Узнайте, как использовать API Потоков Apache Kafka в HDInsight. Этот API позволяет выполнять потоковую обработку между разделами в Kafka.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 11/06/2018
ms.openlocfilehash: 3c40e00d55af49b1b040d3fe706f08af719b2238
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58112795"
---
# <a name="tutorial-apache-kafka-streams-api"></a>Руководство. API Потоков Apache Kafka

Узнайте, как создать приложение, использующее API для Apache Kafka Streams, и запустить его с помощью Kafka в HDInsight. 

В этом руководстве используется приложение для подсчета слов во время потоковой передачи. Оно считывает текстовые данные из раздела Kafka, извлекает отдельные слова, а затем сохраняет слово и количество слов в другом разделе Kafka.

> [!NOTE]  
> Потоковая обработка Kafka часто выполняется с помощью Apache Spark или Apache Storm. В Kafka версии 0.10.0 (в HDInsight 3.5 и 3.6) появился API Потоков Kafka. Этот API позволяет преобразовать потоки данных между входными и выходными разделами. В некоторых случаях это может быть альтернативой созданию решения потоковой передачи Spark или Storm. 
>
> Дополнительные сведения о Потоках Kafka см. в [вводной документации ](https://kafka.apache.org/10/documentation/streams/) на сайте Apache.org.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Настройка среды разработки
> * Изучение кода
> * Создание и развертывание приложения.
> * Настройка разделов Kafka.
> * Выполнение кода

## <a name="prerequisites"></a>Предварительные требования

* Kafka в кластере HDInsight 3.6; Чтобы узнать, как создать кластер Kafka в HDInsight, ознакомьтесь с документом [начале работы с Apache Kafka в HDInsight](apache-kafka-get-started.md).

* Выполните шаги, приведенные в статье [Руководство. Использование API производителя и потребителя Apache Kafka](apache-kafka-producer-consumer-api.md). В шагах, описанных в этом документе, используется пример приложения и разделы, созданные в этом руководстве.

## <a name="set-up-your-development-environment"></a>Настройка среды разработки

В среде разработки необходимо установить следующие компоненты:

* [Java JDK 8](https://aka.ms/azure-jdks) или эквивалент, например OpenJDK.

* [Apache Maven](https://maven.apache.org/)

* Клиент SSH и команда `scp`. Дополнительные сведения см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>Изучение кода

Пример приложения расположен в подкаталоге `Streaming` по адресу [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). Приложение состоит из двух файлов:

* `pom.xml`: этот файл определяет зависимости проекта, версию Java и методы упаковки.
* `Stream.java`: этот файл реализует логику потоковой передачи.

### <a name="pomxml"></a>Pom.xml

В файле `pom.xml` важны следующие элементы:

* Зависимости. Этот проект использует API Потоков Kafka, предоставленный в пакете `kafka-clients`. Приведенный ниже код XML определяет эту зависимость:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]  
    > Запись `${kafka.version}` объявлена в разделе `<properties>..</properties>` файла `pom.xml`. Она настроена для версии Kafka кластера HDInsight.

* Подключаемые модули. Подключаемые модули Maven предоставляют различные возможности. В этом проекте используются следующие подключаемые модули:

    * `maven-compiler-plugin`: с помощью этого модуля можно задать для проекта Java версии 8. Для HDInsight 3.6 требуется Java версии 8.
    * `maven-shade-plugin`: используется для создания файла типа uber jar, содержащего это приложение, а также любые зависимости. Он также используется для установки точки входа приложения, с помощью которой вы сможете напрямую запускать JAR-файл, не указывая основной класс.

### <a name="streamjava"></a>Stream.java

Файл [Stream.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Streaming/src/main/java/com/microsoft/example/Stream.java) использует API потоков для реализации приложения для подсчета слов. Оно считывает данные из раздела Kafka с именем `test` и записывает количество слов в раздел с именем `wordcounts`.

Следующий код определяет приложение для подсчета слов:

```java
package com.microsoft.example;

import org.apache.kafka.common.serialization.Serde;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.KeyValue;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.KStreamBuilder;

import java.util.Arrays;
import java.util.Properties;

public class Stream
{
    public static void main( String[] args ) {
        Properties streamsConfig = new Properties();
        // The name must be unique on the Kafka cluster
        streamsConfig.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-example");
        // Brokers
        streamsConfig.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, args[0]);
        // SerDes for key and values
        streamsConfig.put(StreamsConfig.KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        streamsConfig.put(StreamsConfig.VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());

        // Serdes for the word and count
        Serde<String> stringSerde = Serdes.String();
        Serde<Long> longSerde = Serdes.Long();

        KStreamBuilder builder = new KStreamBuilder();
        KStream<String, String> sentences = builder.stream(stringSerde, stringSerde, "test");
        KStream<String, Long> wordCounts = sentences
                .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
                .map((key, word) -> new KeyValue<>(word, word))
                .countByKey("Counts")
                .toStream();
        wordCounts.to(stringSerde, longSerde, "wordcounts");

        KafkaStreams streams = new KafkaStreams(builder, streamsConfig);
        streams.start();

        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }
}
```


## <a name="build-and-deploy-the-example"></a>Создание и развертывание примера

Чтобы создать и развернуть проект для Kafka в кластере HDInsight, выполните следующие действия:

1. Скачайте примеры по адресу [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Измените каталоги на каталог `Streaming`, а затем используйте следующую команду, чтобы создать пакет JAR:

    ```bash
    mvn clean package
    ```

    Эта команда создает пакет в файле `target/kafka-streaming-1.0-SNAPSHOT.jar`.

3. Используя следующую команду, скопируйте файл `kafka-streaming-1.0-SNAPSHOT.jar` в свой кластер HDInsight.
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Замените `sshuser` именем пользователя SSH для кластера, а `clustername` — именем кластера. При появлении запроса введите пароль для учетной записи пользователя SSH. Дополнительные сведения об использовании `scp` с HDInsight см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-apache-kafka-topics"></a>Создание разделов Apache Kafka

1. Чтобы открыть SSH-подключение к кластеру, выполните следующую команду:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Замените `sshuser` именем пользователя SSH для кластера, а `clustername` — именем кластера. При появлении запроса введите пароль для учетной записи пользователя SSH. Дополнительные сведения об использовании `scp` с HDInsight см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Чтобы сохранить имя кластера в переменной и установить служебную программу синтаксического анализа JSON (`jq`), используйте следующие команды. При появлении запроса введите имя кластера Kafka:

    ```bash
    sudo apt -y install jq
    read -p 'Enter your Kafka cluster name:' CLUSTERNAME
    ```

3. Чтобы получить узлы Apache Zookeeper и брокера Kafka, используйте приведенные ниже команды. При появлении запроса введите пароль для учетной записи администратора, чтобы войти на кластер. Запрос на ввод пароля появится дважды.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
    ```

4. Чтобы создать разделы для операции потоковой передачи, используйте следующие команды:

    > [!NOTE]  
    > Вы можете получить сообщение-ошибку о том, что раздел `test` уже существует. Это нормально, так как он, возможно, был создан в руководстве по API производителя и потребителя.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

    Разделы используются для следующих целей:

   * `test`: в этот раздел поступают записи. Здесь приложение потоковой передачи считывает их.
   * `wordcounts`: в этом разделе приложение потоковой передачи хранит свои выходные данные.
   * `RekeyedIntermediateTopic`: этот раздел используется для секционирования данных, так как счетчик обновляется оператором `countByKey`.
   * `wordcount-example-Counts-changelog`: этот раздел является хранилищем состояний, используемым операцией `countByKey`.

     > [!IMPORTANT]  
     > Кроме того, Kafka в HDInsight можно настроить на автоматическое создание разделов. Дополнительные сведения см. в статье [How to configure Apache Kafka on HDInsight to automatically create topics](apache-kafka-auto-create-topics.md) (Настройка автоматического создания разделов в Apache Kafka в HDInsight).

## <a name="run-the-code"></a>Выполнение кода

1. Для запуска приложения потоковой передачи в качестве фонового процесса используйте следующую команду:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]  
    > Может появиться предупреждение об Apache log4j. На это можно не обращать внимания.

2. Чтобы отправить записи в раздел `test`, используйте следующую команду для запуска приложения-отправителя:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

3. После завершения работы отправителя просмотрите сведения, хранящиеся в разделе `wordcounts`, с помощью следующей команды:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --from-beginning
    ```

    > [!NOTE]  
    > В соответствии с параметрами `--property` объект-получатель консоли печатает ключ (машинное слово) и число (значение). Кроме того, этот параметр настраивает десериализатор, используемый при считывании этих значений из Kafka.

    Результат будет аналогичен приведенному ниже:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
   
    > [!NOTE]  
    > Параметр `--from-beginning` настраивает запуск объекта-получателя в начале записей, хранящихся в разделе. Число увеличивается каждый раз, когда встречается слово, поэтому раздел содержит несколько записей для каждого слова с увеличивающимся числом.

7. Нажмите клавиши __Ctrl+C__, чтобы закрыть отправитель. Снова нажмите клавиши __Ctrl+C__, чтобы выйти из приложения и объекта-получателя.

## <a name="next-steps"></a>Дополнительная информация

Из этого документа вы узнали, как использовать API для Apache Kafka Streams с Kafka в HDInsight. Дополнительные сведения о работе с Kafka см. в следующих материалах.

* [Анализ журналов для Apache Kafka](apache-kafka-log-analytics-operations-management.md)
* [Репликация данных между кластерами Apache Kafka](apache-kafka-mirroring.md)
