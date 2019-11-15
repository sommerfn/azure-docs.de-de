---
title: Entfernen der Verwendung von TLS 1.0 und 1.1 mit Azure Cache for Redis | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie TLS 1.0 und 1.1 bei der Kommunikation mit Azure Cache for Redis aus Ihrer Anwendung entfernen.
services: cache
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 10/22/2019
ms.author: yegu
ms.openlocfilehash: 638ce4b07201453d0bb00d3610cb695b72b6d698
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73806756"
---
# <a name="remove-tls-10-and-11-from-use-with-azure-cache-for-redis"></a>Entfernen der Verwendung von TLS 1.0 und 1.1 mit Azure Cache for Redis

Es zeichnet sich ein branchenweiter Trend zur ausschließlichen Verwendung von Transport Layer Security (TLS) Version 1.2 oder höher ab. Die TLS-Versionen 1.0 und 1.1 sind bekanntermaßen anfällig für Angriffe wie BEAST und POODLE und weisen andere allgemeine Sicherheitslücken und Schwachstellen (CVE, Common Vulnerabilities and Exposures) auf. Außerdem unterstützen diese Versionen nicht die durch die Konformitätsstandards der Payment Card Industry (PCI) empfohlenen modernen Verschlüsselungsmethoden und Verschlüsselungssammlungen. Einige dieser Sicherheitslücken werden in diesem [Blog zur TLS-Sicherheit](https://www.acunetix.com/blog/articles/tls-vulnerabilities-attacks-final-part/) ausführlicher erläutert.

Obwohl keiner dieser Aspekte ein unmittelbares Problem darstellt, wird empfohlen, die Verwendung von TLS 1.0 und 1.1 bald einzustellen. In Azure Cache for Redis werden diese TLS-Versionen ab 31. März 2020 nicht mehr unterstützt. Nach diesem Datum müssen Sie in Ihrer Anwendung für die Kommunikation mit dem Cache TLS 1.2 oder höher verwenden.

Dieser Artikel enthält eine allgemeine Anleitung zum Erkennen von Abhängigkeiten von diesen früheren TLS-Versionen und zum Entfernen der Versionen aus Ihrer Anwendung.

## <a name="check-whether-your-application-is-already-compliant"></a>Überprüfen, ob Ihre Anwendung bereits konform ist

Am einfachsten lässt sich herausfinden, ob die Anwendung mit TLS 1.2 kompatibel ist, indem Sie den Wert **TLS-Mindestversion** für einen in der Anwendung verwendeten Test- oder Stagingcache auf „TLS 1.2“ festlegen. Die Einstellung **TLS-Mindestversion** befindet sich unter [Erweiterte Einstellungen](cache-configure.md#advanced-settings) für Ihre Cache-Instanz im Azure-Portal. Wenn die Anwendung nach dieser Änderung weiterhin erwartungsgemäß ausgeführt wird, ist sie wahrscheinlich konform. Möglicherweise müssen Sie einige in der Anwendung verwendeten Clientbibliotheken spezifisch für die Aktivierung von TLS 1.2 konfigurieren, damit sie über dieses Sicherheitsprotokoll eine Verbindung mit Azure Cache for Redis herstellen können.

## <a name="configure-your-application-to-use-tls-12"></a>Konfigurieren Ihrer Anwendung für die Verwendung von TLS 1.2

In den meisten Anwendungen werden Redis-Clientbibliotheken für die Kommunikation mit den zugehörigen Caches verwendet. Hier finden Sie Anweisungen zum Konfigurieren einiger gängiger Clientbibliotheken in verschiedenen Programmiersprachen und Frameworks für die Verwendung von TLS 1.2.

### <a name="net-framework"></a>.NET Framework

Bei Redis .NET-Clients wird standardmäßig die früheste TLS-Version in .NET Framework 4.5.2 oder früher und die neueste TLS-Version in .NET Framework 4.6 oder höher verwendet. Wenn Sie eine ältere Version von .NET Framework verwenden, können Sie TLS 1.2 manuell aktivieren:

* **StackExchange.Redis:** Legen Sie `ssl=true` und `sslprotocls=tls12` in der Verbindungszeichenfolge fest.
* **ServiceStack.Redis:** Befolgen Sie die [Anweisungen für ServiceStack.Redis](https://github.com/ServiceStack/ServiceStack.Redis/pull/247).

### <a name="net-core"></a>.NET Core

Bei Redis .NET Core-Clients wird standardmäßig die neueste TLS-Version verwendet.

### <a name="java"></a>Java

Bei Redis Java-Clients wird TLS 1.0 für Java Version 6 oder früher verwendet. Bei Jedis, Lettuce, und Radisson kann keine Verbindung mit Azure Cache for Redis hergestellt werden, wenn TLS 1.0 für den Cache deaktiviert ist. Derzeit gibt es dafür keine bekannte Problemumgehung.

In Java 7 oder höher wird bei Redis-Clients standardmäßig TLS 1.2 nicht verwendet, die Clients können aber dafür konfiguriert werden. Derzeit wird diese Konfiguration bei Lettuce und Radisson nicht unterstützt. Es erfolgt ein Abbruch, wenn der Cache nur TLS 1.2-Verbindungen akzeptiert. In Jedis können Sie die zugrunde liegenden TLS-Einstellungen mit dem folgenden Codeausschnitt angeben:

``` Java
SSLSocketFactory sslSocketFactory = (SSLSocketFactory) SSLSocketFactory.getDefault();
SSLParameters sslParameters = new SSLParameters();
sslParameters.setEndpointIdentificationAlgorithm("HTTPS");
sslParameters.setProtocols(new String[]{"TLSv1", "TLSv1.1", "TLSv1.2"});
 
URI uri = URI.create("rediss://host:port");
JedisShardInfo shardInfo = new JedisShardInfo(uri, sslSocketFactory, sslParameters, null);
 
shardInfo.setPassword("cachePassword");
 
Jedis jedis = new Jedis(shardInfo);
```

### <a name="nodejs"></a>Node.js

Bei Node Redis und IORedis wird standardmäßig TLS 1.2 verwendet.

### <a name="php"></a>PHP

Predis in PHP 7 wird nicht ausgeführt, da PHP 7 nur TLS 1.0 unterstützt. In PHP 7.2.1 oder früher wird bei Predis standardmäßig TLS 1.0 oder 1.1 verwendet. Beim Erstellen der Clientinstanz können Sie TLS 1.2 angeben:

``` PHP
$redis=newPredis\Client([
    'scheme'=>'tls',
    'host'=>'host',
    'port'=>6380,
    'password'=>'password',
    'ssl'=>[
        'crypto_type'=>STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT,
    ],
]);
```

In PHP 7.3 oder höher wird bei Predis die neueste TLS-Version verwendet.

Bei PhpRedis wird TLS in keiner PHP-Version unterstützt.

### <a name="python"></a>Python

Bei Redis-py wird standardmäßig TLS 1.2 verwendet.

### <a name="go"></a>GO

Bei Redigo wird standardmäßig TLS 1.2 verwendet.

## <a name="additional-information"></a>Zusätzliche Informationen

- [Konfigurieren von Azure Cache for Redis](cache-configure.md)
