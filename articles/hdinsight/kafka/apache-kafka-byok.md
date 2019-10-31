---
title: Bring Your Own Key für Apache Kafka in Azure HDInsight
description: In diesem Artikel wird beschrieben, wie Sie mit Ihrem eigenen Schlüssel aus Azure Key Vault Daten verschlüsseln, die in Apache Kafka in Azure HDInsight gespeichert sind.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: ba49944011546db45d25cc87c2c4b93c8b99502a
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "71122678"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight"></a>Bring Your Own Key für Apache Kafka in Azure HDInsight

Azure HDInsight beinhaltet BYOK-Unterstützung (Bring Your Own Key) für Apache Kafka. Dank dieser Funktion können Sie die Verantwortung und die Verwaltung für die Schlüssel übernehmen, die zum Verschlüsseln ruhender Daten verwendet werden.

Alle verwalteten Datenträger in HDInsight werden mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) von Azure geschützt. Die Daten auf diesen Datenträgern werden standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Bei der Aktivierung von BYOK geben Sie den Verschlüsselungsschlüssel für HDInsight für die Verwendung und Verwaltung mit Azure Key Vault an.

Die BYOK-Verschlüsselung besteht aus einem Schritt, der bei der Clustererstellung ohne zusätzliche Kosten ausgeführt wird. Sie müssen bei der Erstellung Ihres Clusters lediglich HDInsight als verwaltete Identität bei Azure Key Vault registrieren und den Verschlüsselungsschlüssel hinzufügen.

Alle Nachrichten an den Kafka-Cluster (einschließlich von Kafka verwaltete Replikate) werden mit einem symmetrischen Datenverschlüsselungsschlüssel (Data Encryption Key, DEK) verschlüsselt. Der DEK wird mit dem Schlüssel für die Schlüsselverschlüsselung (Key Encryption Key, KEK) aus Ihrem Schlüsseltresor geschützt. Die Ver- und Entschlüsselung wird vollständig von Azure HDInsight übernommen.

Sie können für das sichere Rotieren der Schlüssel im Schlüsseltresor das Azure-Portal oder die Azure CLI verwenden. Beim Rotieren eines Schlüssels beginnt der HDInsight Kafka-Cluster innerhalb weniger Minuten mit der Verwendung des neuen Schlüssels. Aktivieren Sie die Schlüsselschutzfunktion „Soft Delete“ (Vorläufig löschen) zum Schutz vor Ransomware und versehentlichem Löschen. Schlüsseltresore ohne diese Schutzfunktion werden nicht unterstützt.

## <a name="get-started-with-byok"></a>Erste Schritte mit BYOK
Zum Erstellen eines Kafka-Clusters mit BYOK führen Sie die folgenden Schritte aus:
1. Erstellen verwalteter Identitäten für Azure-Ressourcen
2. Einrichten von Azure Key Vault mit den Schlüsseln
3. Erstellen von HDInsight-Kafka-Clustern mit aktiviertem BYOK
4. Rotieren des Verschlüsselungsschlüssels

## <a name="create-managed-identities-for-azure-resources"></a>Erstellen verwalteter Identitäten für Azure-Ressourcen

   Erstellen Sie für die Authentifizierung bei Key Vault mit dem [Azure-Portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md) oder der [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) eine benutzerseitig zugewiesene verwaltete Identität. Weitere Informationen zur Funktionsweise verwalteter Identitäten in Azure HDInsight finden Sie unter [Verwaltete Identitäten in Azure HDInsight](../hdinsight-managed-identities.md). Azure Active Directory ist für verwaltete Identitäten und BYOK in Kafka erforderlich, das Enterprise-Sicherheitspaket (Enterprise Security Package, ESP) jedoch nicht. Achten Sie darauf, die Ressourcen-ID der verwalteten Identität zu speichern, da Sie sie später zur Key Vault-Zugriffsrichtlinie hinzufügen.

   ![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität im Azure-Portal](./media/apache-kafka-byok/user-managed-identity-portal.png)

## <a name="setup-the-key-vault-and-keys"></a>Einrichten des Schlüsseltresors mit den Schlüsseln

   HDInsight unterstützt nur Azure Key Vault. Falls Sie einen eigenen Schlüsseltresor besitzen, können Sie Ihre Schlüssel in Azure Key Vault importieren. Denken Sie daran, dass für die Schlüssel „Vorläufig löschen“ aktiviert sein muss. Die Funktion für vorläufiges Löschen ist über REST, .NET/C#, PowerShell und die Azure-Befehlszeilenschnittstelle verfügbar.

   1. Befolgen Sie zum Erstellen eines neuen Schlüsseltresors die Schnellstartanleitung für [Azure Key Vault](../../key-vault/key-vault-overview.md). Weitere Informationen zum Importieren vorhandener Schlüsseln finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../../key-vault/about-keys-secrets-and-certificates.md).

   2. Aktivieren Sie „Vorläufig löschen“ für den Schlüsseltresor mithilfe des CLI-Befehls [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update).

        ```Azure CLI
        az keyvault update --name <Key Vault Name> --enable-soft-delete
        ```

   3. Erstellen von Schlüsseln

        a. Wählen Sie zum Erstellen eines neuen Schlüssels **Generieren/Importieren** im Menü **Schlüssel** unter **Einstellungen** aus.

        ![Generieren eines neuen Schlüssels in Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png "Generieren eines neuen Schlüssels in Azure Key Vault")

        b. Legen Sie **Optionen** auf **Generieren** fest, und benennen Sie den Schlüssel.

        ![Apache Kafka: Generieren des Schlüsselnamens](./media/apache-kafka-byok/apache-kafka-create-key.png "Generieren des Schlüsselnamens")

        c. Wählen Sie den Schlüssel, den Sie erstellt haben, aus der Liste der Schlüssel aus.

        ![Apache Kafka Key Vault-Schlüsselliste](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

        d. Wenn Sie Ihren eigenen Schlüssel für die Verschlüsselung des Kafka-Clusters verwenden, müssen Sie die Schlüssel-URI angeben. Kopieren Sie den **Schlüsselbezeichner**, und speichern sie ihn, bis Sie zum Erstellen des Clusters bereit sind.

        ![Apache Kafka – Abrufen des Schlüsselbezeichners](./media/apache-kafka-byok/kafka-get-key-identifier.png)

    4. Fügen Sie eine verwaltete Identität zur Key Vault-Zugriffsrichtlinie hinzu.

        a. Erstellen Sie eine neue Azure Key Vault-Zugriffsrichtlinie.

        ![Erstellen einer neuen Azure Key Vault-Zugriffsrichtlinie](./media/apache-kafka-byok/add-key-vault-access-policy.png)

        b. Wählen Sie unter **Prinzipal auswählen** die von Ihnen erstellte benutzerseitig zugewiesene verwaltete Identität aus.

        ![Festlegen der Option „Prinzipal auswählen“ für die Azure Key Vault-Zugriffsrichtlinie](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

        c. Aktivieren Sie unter **Schlüsselberechtigungen** die Optionen **Abrufen**, **Schlüssel entpacken** und **Schlüssel packen**.

        ![Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 1](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png "Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 1")

        d. Aktivieren Sie unter **Berechtigungen für Geheimnis** die Optionen **Abrufen**, **Festlegen** und **Löschen**.

        ![Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 2](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png "Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 2")

        e. Klicken Sie auf **Speichern**. 

        ![Speichern einer Azure Key Vault-Zugriffsrichtlinie](./media/apache-kafka-byok/add-key-vault-access-policy-save.png)

## <a name="create-hdinsight-cluster"></a>Erstellen des HDInsight-Clusters

   Sie können nun einen neuen HDInsight-Cluster erstellen. BYOK kann während der Clustererstellung nur auf neue Cluster angewendet werden. Die Verschlüsselung kann für BYOK-Cluster nicht entfernt werden, und BYOK kann nicht zu vorhandenen Clustern hinzugefügt werden.

   ![Kafka-Datenträgerverschlüsselung im Azure-Portal](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   Geben Sie während der Clustererstellung die vollständige Schlüssel-URL (einschließlich Schlüsselversion) an. Beispiel: `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. Darüber hinaus müssen Sie die verwaltete Identität dem Cluster zuweisen und den Schlüssel-URI angeben.

## <a name="rotating-the-encryption-key"></a>Rotieren des Verschlüsselungsschlüssels

   Es gibt möglicherweise Szenarien, in denen Sie die Verschlüsselungsschlüssel ändern möchten, die vom Kafka-Cluster verwendet werden, nachdem er erstellt wurde. Hierzu können Sie das Portal verwenden. Für diesen Vorgang muss der Cluster sowohl auf den aktuellen Schlüssel als auch auf den vorgesehenen neuen Schlüssel zugreifen können, andernfalls schlägt der Vorgang zum Rotieren des Schlüssels fehl.

   Um den Schlüssel zu rotieren, müssen Sie über die vollständige URL des neuen Schlüssels verfügen (siehe Schritt 3 unter [Einrichten des Schlüsseltresors und der Schlüssel](#setup-the-key-vault-and-keys)). Wechseln Sie anschließend zum Abschnitt „Kafka-Clustereigenschaften“ im Portal, und klicken Sie unter **Schlüssel-URL für Datenträgerverschlüsselung** auf **Schlüssel ändern**. Geben Sie die neue Schlüssel-URL ein, und senden Sie sie, um den Schlüssel zu rotieren.

   ![Schlüssel für Kafka-Datenträgerverschlüsselung rotieren](./media/apache-kafka-byok/apache-kafka-change-key.png)

## <a name="faq-for-byok-to-apache-kafka"></a>Häufig gestellte Fragen zu BYOK in Apache Kafka

**Wie greift der Kafka-Cluster auf meinen Schlüsseltresor zu?**

   Ordnen Sie eine verwaltete Identität während der Clustererstellung dem HDInsight Kafka-Cluster zu. Diese verwaltete Identität kann vor oder während der Clustererstellung erstellt werden. Außerdem müssen Sie der verwalteten Identität Zugriff auf den Schlüsseltresor mit dem gespeicherten Schlüssel gewähren.

**Ist dieses Feature für alle Kafka-Cluster in HDInsight verfügbar?**

   Die BYOK-Verschlüsselung ist nur für Kafka-Cluster mit Version 1.1 und höheren Versionen möglich.

**Kann ich verschiedene Schlüssel für verschiedene Themen/Partitionen verwenden?**

   Nein, alle verwalteten Datenträger im Cluster werden mit dem gleichen Schlüssel verschlüsselt.

**Was geschieht, wenn der Cluster den Zugriff auf den Schlüsseltresor oder den Schlüssel verliert?**
Wenn der Cluster den Zugriff auf den Schlüssel verliert, werden im Apache Ambari-Portal Warnungen angezeigt. In diesem Zustand schlägt der Vorgang **Schlüssel ändern** fehl. Sobald der Schlüsselzugriff wieder hergestellt wurde, verschwinden die Ambari-Warnungen, und Vorgänge wie z. B. die Schlüsselrotation können erfolgreich ausgeführt werden.

   ![Ambari-Warnung zum Apache Kafka-Schlüsselzugriff](./media/apache-kafka-byok/kafka-byok-ambari-alert.png)

**Wie kann ich den Cluster wiederherstellen, wenn die Schlüssel gelöscht werden?**

   Da nur Schlüssel unterstützt werden, für die „Soft Delete“ (Vorläufig löschen) aktiviert ist, sollte der Cluster wieder Zugriff auf die Schlüssel erlangen, wenn die Schlüssel im Schlüsseltresor wiederhergestellt werden. Informationen zum Wiederherstellen eines Azure Key Vault-Schlüssels finden Sie unter [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) oder [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**Können Producer-/Consumeranwendungen gleichzeitig einen BYOK-Cluster und einen Cluster ohne BYOK verwenden?**

   Ja. Die Verwendung von BYOK ist für Producer-/Consumeranwendungen transparent. Die Verschlüsselung erfolgt auf Betriebssystemebene. An vorhandenen Producer-/Consumeranwendungen mit Kafka müssen keine Änderungen vorgenommen werden.

**Werden Betriebssystemdatenträger/Ressourcendatenträger ebenfalls verschlüsselt?**

   Nein. Betriebssystemdatenträger und Ressourcendatenträger werden nicht verschlüsselt.

**Unterstützen die neuen Broker BYOK einwandfrei, wenn ein Cluster zentral hochskaliert wird?**

   Ja. Der Cluster benötigt während der zentralen Hochskalierung Zugriff auf den Schlüssel im Schlüsseltresor. Alle verwalteten Datenträger im Cluster werden mit dem gleichen Schlüssel verschlüsselt.

**Ist BYOK an meinem Standort verfügbar?**

   Kafka BYOK ist in allen öffentlichen Clouds verfügbar.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure Key Vault finden Sie unter [Was ist der Azure-Schlüsseltresor?](../../key-vault/key-vault-overview.md).
* Weitere Informationen zu den ersten Schritten mit Azure Key Vault finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../../key-vault/key-vault-overview.md).
