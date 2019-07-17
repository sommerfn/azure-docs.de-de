---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/26/2019
ms.author: alkohli
ms.openlocfilehash: 09d9b5bbf3f9ca7a4eef37891d03c9c865e7f74b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448623"
---
Ein ordnungsgemäßes SSL-Zertifikat stellt sicher, dass Sie verschlüsselte Informationen an den richtigen Server senden. Neben der Verschlüsselung ermöglicht das Zertifikat auch die Authentifizierung. Sie können über die PowerShell-Schnittstelle des Geräts Ihr eigenes vertrauenswürdiges SSL-Zertifikat hochladen.

1. [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](#connect-to-the-powershell-interface).
2. Verwenden Sie das Cmdlet `Set-HcsCertificate`, um das Zertifikat hochzuladen. Geben Sie bei entsprechender Aufforderung die folgenden Parameter an:

   - `CertificateFilePath`: Pfad zur Freigabe, der die Zertifikatdatei im *PFX*-Format enthält.
   - `CertificatePassword`: Kennwort zum Schutz des Zertifikats.
   - `Credentials`: Benutzername für den Zugriff auf die Freigabe, die das Zertifikat enthält. Geben Sie bei Aufforderung das Kennwort für die Netzwerkfreigabe an.

     Nachfolgend sehen Sie ein Beispiel für die Verwendung dieses Cmdlets:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username"
     ```

