---
title: 在 HDInsight 中的查询控制台上使用 Hadoop Hive — Azure | Microsoft Docs
description: 了解如何在浏览器中使用基于 Web 的查询控制台在 HDInsight Hadoop 群集上运行 Hive 查询。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 04a6ad67fec4145d8f9164743b08f9e105778091
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2018
---
# <a name="run-hive-queries-using-the-query-console"></a>使用查询控制台运行 Hive 查询
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

在本文中，将了解如何在浏览器中使用 HDInsight 查询控制台在 HDInsight Hadoop 群集上运行 Hive 查询。

> [!IMPORTANT]
> HDInsight 查询控制台只能在基于 Windows 的 HDInsight 群集上使用。 Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 在 Windows 上停用](../hdinsight-component-versioning.md#hdinsight-windows-retirement)。
>
> 有关 HDInsight 3.4 或更高版本，请参阅[在 Ambari Hive 视图中运行 Hive 查询](apache-hadoop-use-hive-ambari-view.md)，了解如何通过 Web 浏览器运行 Hive 查询。

## <a id="prereq"></a>先决条件
要完成本文中的步骤，需要：

* 基于 Windows 的 HDInsight Hadoop 群集
* 现代 Web 浏览器

## <a id="run"></a>使用查询控制台运行 Hive 查询
1. 打开 Web 浏览器并导航到 **https://CLUSTERNAME.azurehdinsight.net**，其中“CLUSTERNAME”是 HDInsight 群集的名称。 出现提示时，输入在创建群集时使用的用户名和密码。
2. 在页面顶部的链接中，选择“Hive 编辑器”。 此时会显示一个窗体，可以在其中输入要在 HDInsight 群集上运行的 HiveQL 语句。

    ![hive 编辑器](./media/apache-hadoop-use-hive-query-console/queryconsole.png)

    将文本 `Select * from hivesampletable` 替换为以下 HiveQL 语句：

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    这些语句将执行以下操作：

   * **DROP TABLE**：删除表和数据文件（如果该表已存在）。
   * **CREATE EXTERNAL TABLE**：在 Hive 中创建新的“外部”表。 外部表仅在 Hive 中存储表定义；数据会保留在原始位置。

     > [!NOTE]
     > 预期以外部源更新基础数据（例如自动化数据上传过程），或以其他 MapReduce 操作更新基础数据，但希望 Hive 查询始终使用最新数据时，必须使用外部表。
     >
     > 删除外部表**不会**删除数据，只会删除表定义。
     >
     >
   * **ROW FORMAT**：告知 Hive 如何设置数据的格式。 在此情况下，每个日志中的字段以空格分隔。
   * **STORED AS TEXTFILE LOCATION**：告知 Hive 数据的存储位置（example/data 目录），并且告知数据已存储为文本。
   * **SELECT**：选择其列 **t4** 包含值 **[ERROR]** 的所有行的计数。 这应会返回值 **3**，因为有三行包含此值。
   * **INPUT__FILE__NAME LIKE '%.log'** - 告诉 Hive，我们只应返回以 .log 结尾的文件中的数据。 此项将搜索限定于包含数据的 sample.log 文件，使搜索不会返回与所定义架构不符的其他示例数据文件中的数据。
3. 单击“提交”。 页面底部的“作业会话”应显示作业的详细信息。
4. 当“状态”字段更改为“已完成”时，请选择与作业对应的“查看详细信息”。 在详细信息页上，“作业输出”包含 `[ERROR]    3`。 可以使用此字段下面的“下载”按钮下载包含作业的输出的文件。

## <a id="summary"></a>摘要
如你所见，查询控制台提供了简单的方法让你在 HDInsight 群集上运行 Hive 查询，监视作业状态，以及检索输出。

要了解有关使用 Hive 查询控制台运行 Hive 作业的详细信息，请选择查询控制台顶部的“入门”，并使用提供的示例。 每个示例演练使用 Hive 分析数据的过程，包括有关本示例中使用的 HiveQL 语句的说明。

## <a id="nextsteps"></a>后续步骤
有关 HDInsight 中的 Hive 的一般信息：

* [将 Hive 与 Hadoop on HDInsight 配合使用](hdinsight-use-hive.md)

有关 HDInsight 上的 Hadoop 的其他使用方法的信息：

* [将 Pig 与 Hadoop on HDInsight 配合使用](hdinsight-use-pig.md)
* [将 MapReduce 与 HDInsight 上的 Hadoop 配合使用](hdinsight-use-mapreduce.md)

如果将 Tez 与 Hive 配合使用，请参阅以下文档以了解调试信息：

* [在基于 Windows 的 HDInsight 上使用 Tez UI](../hdinsight-debug-tez-ui.md)
* [在基于 Linux 的 HDInsight 上使用 Ambari Tez 视图](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
