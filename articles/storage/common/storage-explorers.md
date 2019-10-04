---
title: Tools für das Arbeiten mit Azure Storage
description: Enthält eine Liste der Tools, die das Anzeigen von und das Interagieren mit Azure Storage-Daten ermöglichen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/26/2019
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: common
ms.openlocfilehash: 11838a50d70d1b9a0216505e9ef0958d3b18bdac
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70035266"
---
# <a name="azure-storage-client-tools"></a>Azure Storage-Clienttools
Häufig möchten Benutzer von Azure Storage ihre Daten mithilfe eines Azure Storage-Clienttools anzeigen oder mit den Daten interagieren. In den folgenden Tabellen sind eine Reihe von Tools aufgeführt, die Ihnen dies ermöglichen. Ein Block ist jeweils mit „X“ gekennzeichnet, wenn er das Aufzählen und/oder Zugreifen auf die Datenabstraktion ermöglicht. In der Tabelle ist auch angegeben, ob die Tools kostenlos sind. Mit „Testversion“ wird angegeben, dass es eine kostenlose Testversion gibt, das vollständige Produkt aber nicht kostenfrei ist. Mit „J/N“ wird angegeben, dass eine Version kostenlos erhältlich ist und eine andere Version zum Kauf zur Verfügung steht.

Dies ist nur eine Momentaufnahme der verfügbaren Azure Storage-Clienttools. Diese Tools werden ggf. weiterentwickelt und mit weiteren Funktionen ausgestattet. Wenn Korrekturen oder Updates vorhanden sind, können Sie einen Kommentar hinterlassen, um uns dies mitzuteilen. Dies gilt auch, wenn Sie Tools kennen, die hier aufgeführt sein sollten. Wir fügen sie dann gerne hinzu.

**Microsoft Azure Storage-Clienttools**

<table>
  <tr>
    <th rowspan="2">Azure Storage-Clienttool</th>
    <th rowspan="2">Blockblob</th>
    <th rowspan="2">Seitenblob</th>
    <th rowspan="2">Blob anfügen</th>
    <th rowspan="2">Tabellen</th>
    <th rowspan="2">Warteschlangen</th>
    <th rowspan="2">Dateien</th>
    <th rowspan="2">Kostenlos</th>
    <th colspan="4">Plattform</th>
  </tr>
  <tr>
    <td>Web</td>
    <td>Windows</td>
    <td>OSX</td>
    <td>Linux</td>
  </tr>
  <tr>
    <td><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure-Portal</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>J</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://storageexplorer.com/">Microsoft Azure Storage-Explorer</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>J</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
  </tr>
  <tr>
    <td><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Server-Explorer von Microsoft Visual Studio</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>J</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
</table>

**Azure Storage-Clienttools von Drittanbietern**

Wir haben die Funktionalität oder Qualität nicht überprüft, die für die folgenden Tools von Drittanbietern angegeben wird, und das Aufführen in dieser Liste stellt keinerlei Empfehlung durch Microsoft dar.

<table>
  <tr>
    <th rowspan="2">Azure Storage-Clienttool</th>
    <th rowspan="2">Blockblob</th>
    <th rowspan="2">Seitenblob</th>
    <th rowspan="2">Blob anfügen</th>
    <th rowspan="2">Tabellen</th>
    <th rowspan="2">Warteschlangen</th>
    <th rowspan="2">Dateien</th>
    <th rowspan="2">Kostenlos</th>
    <th colspan="4">Plattform</th>
  </tr>
  <tr>
    <td>Web</td>
    <td>Windows</td>
    <td>OSX</td>
    <td>Linux</td>
  </tr>
  <tr>
    <td><a href="https://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>Testversion</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Web Storage-Explorer</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>J</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://www.cloudberrylab.com/explorer/microsoft-azure.aspx">CloudBerry Explorer</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
    <td>X</td>
    <td>J/N</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://www.gapotchenko.com/cloudcombine">Cloud Combine</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>Testversion</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://clumsyleaf.com">ClumsyLeaf: AzureXplorer, CloudXplorer, TableXplorer</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>J</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://www.gladinet.com/Azure-Storage/index.htm">Gladinet Cloud</a></td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>Testversion</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
</table>
