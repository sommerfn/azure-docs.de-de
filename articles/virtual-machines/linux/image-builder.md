---
title: Erstellen eines virtuellen Linux-Computers mit Azure Image Builder (Vorschauversion)
description: Erstellen Sie einen virtuellen Linux-Computer mit Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: gwallace
ms.openlocfilehash: 2966a1803d0664312d71ba992a5ba65f73b27370
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667526"
---
# <a name="preview-create-a-linux-vm-with-azure-image-builder"></a>Vorschau: Erstellen eines virtuellen Linux-Computers mit Azure Image Builder

In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Linux-Image mit Azure Image Builder und der Azure-Befehlszeilenschnittstelle erstellen. Im Beispiel in diesem Artikel werden drei verschiedene [Anpassungen](image-builder-json.md#properties-customize) für das Image verwendet:

- Shell (ScriptUri): lädt ein [Shellskript](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh) herunter und führt es aus.
- Shell (inline): führt spezifische Befehle aus. In diesem Beispiel schließen die Inline-Befehle das Erstellen eines Verzeichnisses und das Aktualisieren des Betriebssystems ein.
- Datei: kopiert eine [Datei von GitHub](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) in ein Verzeichnis auf dem virtuellen Computer.


Wir verwenden zum Konfigurieren des Images eine JSON-Beispielvorlage. Wir verwenden die JSON-Datei [helloImageTemplateLinux.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json). 

> [!IMPORTANT]
> Azure Image Builder ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Registrieren des Features
Sie müssen das neue Feature registrieren, um Azure Image Builder während der Vorschauphase verwenden zu können.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Überprüfen Sie den Status der Featureregistrierung.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Überprüfen Sie die Registrierung.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState

az provider show -n Microsoft.Storage | grep registrationState
```

Wenn nicht „registered“ ausgegeben wird, führen Sie den folgenden Befehl aus:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```

## <a name="setup-example-variables"></a>Einrichten von Beispielvariablen

Einige Angaben verwenden wir wiederholt. Aus diesem Grund erstellen wir einige Variablen, um diese Informationen zu speichern.


```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myImageBuilerRGLinux
# Datacenter location - we are using West US 2 in this example
location=WestUS2
# Name for the image - we are using myBuilderImage in this example
imageName=myBuilderImage
# Run output name
runOutputName=aibLinux
```

Erstellen Sie eine Variable für Ihre Abonnement-ID. Diese können Sie mit `az account show | grep id` abrufen.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

## <a name="create-the-resource-group"></a>Erstellen Sie die Ressourcengruppe.
Diese wird verwendet, um das Artefakt und Image der Imagekonfigurationsvorlage zu speichern.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="set-permissions-on-the-resource-group"></a>Festlegen von Berechtigungen für die Ressourcengruppe
Erteilen Sie die Image Builder-Berechtigung „Mitwirkender“, um das Image in der Ressourcengruppe zu erstellen. Ohne die entsprechenden Berechtigungen schlägt das Erstellen des Images fehl. 

Der Wert `--assignee` ist die App-Registrierungs-ID für den Image Builder-Dienst. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```

## <a name="download-the-template-example"></a>Herunterladen der Beispielvorlage

Eine parametrisierte Beispielkonfigurationsvorlage für Images wurde für Sie erstellt. Laden Sie die JSON-Beispieldatei herunter, und konfigurieren Sie sie mit den Variablen, die Sie zuvor festgelegt haben.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json -o helloImageTemplateLinux.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateLinux.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateLinux.json
sed -i -e "s/<region>/$location/g" helloImageTemplateLinux.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateLinux.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateLinux.json
```

Sie können diese JSON-Beispieldatei bei Bedarf ändern. Sie können beispielsweise den Wert von `buildTimeoutInMinutes` erhöhen, um länger laufende Builds zu ermöglichen. Sie können die Datei in Cloud Shell mit `vi` bearbeiten.

```azurecli-interactive
vi helloImageTemplateLinux.json
```

> [!NOTE]
> Für das Quellimage müssen Sie immer [eine Version](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-version-failure) angeben. Sie können `latest` nicht verwenden.
>
> Wenn Sie die Ressourcengruppe, in der das Image verteilt wird, hinzufügen oder ändern, müssen Sie sicherstellen, dass die [Berechtigungen für die Ressourcengruppe festgelegt sind](#set-permissions-on-the-resource-group).


## <a name="submit-the-image-configuration"></a>Übermitteln der Imagekonfiguration
Senden der Imagekonfiguration an den VM-Image Builder-Dienst

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateLinux.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Wenn die Aktion erfolgreich abgeschlossen wurde, gibt sie eine Erfolgsmeldung zurück und erstellt in „$imageResourceGroup“ ein Artefakt mit einer Konfigurationsvorlage für Image Builder. Sie können die Ressourcengruppe im Portal anzeigen, wenn Sie „Ausgeblendete Typen anzeigen“ aktivieren.

Außerdem erstellt Image Builder im Hintergrund eine Stagingressourcengruppe in Ihrem Abonnement. Image Builder verwendet die Stagingressourcengruppe für den Imagebuild. Der Name der Ressourcengruppe hat das folgende Format: `IT_<DestinationResourceGroup>_<TemplateName>`.

> [!IMPORTANT]
> Löschen Sie die Stagingressourcengruppe nicht direkt. Wenn Sie das Artefakt der Imagevorlage löschen, wird automatisch die Stagingressourcengruppe gelöscht. Weitere Informationen finden Sie im Abschnitt [Bereinigen](#clean-up) am Ende dieses Artikels.

Wenn der Dienst während der Übermittlung der Imagekonfigurationsvorlage einen Fehler meldet, lesen Sie die Schritte zur [Problembehandlung](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#template-submission-errors--troubleshooting). Sie müssen auch die Vorlage löschen, bevor Sie versuchen, den Build erneut zu übermitteln. So löschen Sie die Vorlage

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

## <a name="start-the-image-build"></a>Starten des Imagebuilds

Starten Sie den Imagebuild.


```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateLinux01 \
     --action Run 
```

Warten Sie, bis der Build abgeschlossen ist, was bei diesem Beispiel 10 bis 15 Minuten dauern kann.

Wenn Fehler auftreten sollten, prüfen Sie die Schritte unter [Problembehandlung](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-build-errors--troubleshooting).


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie den virtuellen Computer mithilfe des erstellten Images.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name myVM \
  --admin-username azureuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

Ermitteln Sie die IP-Adresse aus der Ausgabe der VM-Erstellung, und stellen Sie damit eine SSH-Verbindung mit dem virtuellen Computer her.

```azurecli-interactive
ssh azureuser@<pubIp>
```

Sie sollten sehen, dass das Image angepasst wurde. Es wird eine „Nachricht des Tages“ ausgegeben, sobald die SSH-Verbindung hergestellt wurde.

```console

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

Geben Sie zum Abschluss `exit` ein, um die SSH-Verbindung zu schließen.

## <a name="check-the-source"></a>Überprüfen der Quelle

In der Image Builder-Vorlage werden unter „Properties“ das Quellimage, das ausgeführte Anpassungsskript und der Ort der Verteilung angegeben.

```azurecli-interactive
cat helloImageTemplateLinux.json
```

Weitere ausführliche Informationen zu dieser JSON-Datei finden Sie unter [Image Builder-Vorlagenreferenz](image-builder-json.md).

## <a name="clean-up"></a>Bereinigen

Wenn Sie fertig sind, können Sie die Ressourcen löschen.

Löschen Sie die Image Builder-Vorlage.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Löschen Sie die Imageressourcengruppe.

```bash
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Komponenten der in diesem Artikel verwendeten JSON-Datei finden Sie unter [Image Builder-Vorlagenreferenz](image-builder-json.md).
