---
title: Bereitstellen einer eigenständigen Prometheus-Instanz in einem Azure Red Hat OpenShift-Cluster | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Prometheus-Instanz auf einem Azure Red Hat OpenShift-Cluster erstellen, um die Metriken Ihrer Anwendung zu überwachen.
author: Makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus, aro, openshift, metriken, red hat
ms.openlocfilehash: e66658151361edd43f61d398274c88c047928028
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343192"
---
# <a name="deploy-a-standalone-prometheus-in-an-azure-red-hat-openshift-cluster"></a>Bereitstellen einer eigenständigen Prometheus-Instanz in einem Azure Red Hat OpenShift-Cluster

In dieser Anleitung wird beschrieben, wie Sie eine eigenständige Prometheus-Instanz mit Dienstermittlung in einem Azure Red Hat OpenShift-Cluster konfigurieren.  Der Zugriff auf den Cluster durch den „Kundenadministrator“ ist NICHT erforderlich.

Die Zieleinrichtung sieht wie folgt aus:

- ein Projekt (prometheus-project) mit Prometheus und Alertmanager
- zwei Projekte (app-project1 und app-project2) mit den zu überwachenden Anwendungen

Sie bereiten einige Prometheus-Konfigurationsdateien lokal vor. Erstellen Sie einen Ordner zum Speichern dieser Dateien.
Diese Konfigurationsdateien werden im Cluster als Geheimnisse gespeichert, für den Fall, dass später geheime Token hinzugefügt werden.

## <a name="step-1-sign-in-to-the-cluster-using-the-oc-tool"></a>Schritt 1: Melden Sie sich mithilfe des `oc`-Tools beim Cluster an.
Navigieren Sie in einem Webbrowser zur Webkonsole Ihres Clusters (https://openshift.*random-id*.*region*.azmosa.io).
Melden Sie sich mit Ihren Azure-Anmeldeinformationen an.
Klicken Sie oben rechts auf Ihren Benutzernamen, und wählen Sie „Copy Login Command“ (Anmeldebefehl kopieren) aus. Fügen Sie das Kopierte in das von Ihnen verwendete Terminal ein.

Sie können mit dem `oc whoami -c`-Befehl überprüfen, ob Sie mit dem richtigen Benutzer angemeldet sind.

## <a name="step-2-prepare-the-projects"></a>Schritt 2: Vorbereiten der Projekte

Erstellen Sie Projekte.
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> Sie können entweder den Parameter `-n` oder `--namespace` verwenden oder mit dem `oc project`-Befehl ein aktives Projekt auswählen.

## <a name="step-3-prepare-prometheus-config"></a>Schritt 3: Vorbereiten der Prometheus-Konfiguration
Erstellen Sie eine Datei namens „prometheus.yml“ mit folgendem Inhalt.
```
global:
  scrape_interval: 30s
  evaluation_interval: 5s

scrape_configs:
    - job_name: prom-sd
      scrape_interval: 30s
      scrape_timeout: 10s
      metrics_path: /metrics
      scheme: http
      kubernetes_sd_configs:
      - api_server: null
        role: endpoints
        namespaces:
          names:
          - prometheus-project
          - app-project1
          - app-project2
```
Erstellen Sie ein Geheimnis namens „prom“ mit der Konfiguration.
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

Bei der oben aufgeführten Datei handelt es sich um eine grundlegende Prometheus-Konfigurationsdatei.
Sie legt die Intervalle fest und konfiguriert die automatische Ermittlung in drei Projekten (prometheus-project, app-project1, app-project2).
In diesem Beispiel werden die automatisch ermittelten Endpunkte über HTTP ohne Authentifizierung ausgelesen.
Weitere Informationen zum Auslesen von Endpunkten finden Sie unter https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config.


## <a name="step-4-prepare-alertmanager-config"></a>Schritt 4: Vorbereiten der Alertmanager-Konfiguration
Erstellen Sie eine Datei namens „alertmanager.yml“ mit folgendem Inhalt.
```
global:
  resolve_timeout: 5m
route:
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 12h
  receiver: default
  routes:
  - match:
      alertname: DeadMansSwitch
    repeat_interval: 5m
    receiver: deadmansswitch
receivers:
- name: default
- name: deadmansswitch
```
Erstellen Sie ein Geheimnis namens „prom-alerts“ mit der Konfiguration.
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

Bei der oben aufgeführten Datei handelt es sich um die Alert Manager-Konfigurationsdatei.

> [!NOTE]
> Sie können die beiden vorherigen Schritte mit `oc get secret -n prometheus-project` überprüfen.

## <a name="step-5-start-prometheus-and-alertmanager"></a>Schritt 5: Starten von Prometheus und Alertmanager
Laden Sie die Vorlage [prometheus-standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml) aus [openshift/origin repository](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) herunter, und wenden Sie diese auf das prometheus-project an.
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
Bei der Datei „prometheus-standalone.yaml“ handelt es sich um eine OpenShift-Vorlage, die eine Prometheus-Instanz mit oauth-proxy davor sowie eine Alertmanager-Instanz erstellt, die ebenfalls durch ein oauth-proxy geschützt ist.  In dieser Vorlage wird oauth-proxy so konfiguriert, dass jeder Benutzer zugelassen wird, der den „prometheus-project“-Namespace abrufen kann (siehe Flag `-openshift-sar`).

> [!NOTE]
> Sie können mit dem Befehl `oc get statefulset -n prometheus-project` überprüfen, ob das StatefulSet „prom“ über gleiche *DESIRED*- und *CURRENT*-Nummernreplikate verfügt.
> Sie können auch alle Ressourcen im Projekt mit `oc get all -n prometheus-project` überprüfen.

## <a name="step-6-add-permissions-to-allow-service-discovery"></a>Schritt 6: Hinzufügen von Berechtigungen zum Zulassen der Dienstermittlung
Erstellen Sie die Datei „prometheus-sdrole.yml“ mit folgendem Inhalt.
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus Service Discovery Role
    description: |
      A role and rolebinding adding permissions required to perform Service Discovery in a given project.
    iconClass: fa fa-cogs
    tags: "monitoring,prometheus,alertmanager,time-series"
parameters:
- description: The project name, where a standalone Prometheus is deployed
  name: PROMETHEUS_PROJECT
  value: prometheus-project
objects:
- apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: prometheus-sd
  rules:
  - apiGroups:
    - ""
    resources:
    - services
    - endpoints
    - pods
    verbs:
    - list
    - get
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: prometheus-sd
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: prometheus-sd
  subjects:
  - kind: ServiceAccount
    name: prom
    namespace: ${PROMETHEUS_PROJECT}
```
Wenden Sie die Vorlage auf alle Projekte an, bei denen Sie die Dienstermittlung zulassen möchten.
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
```
Wenn Sie zudem möchten, dass Prometheus Metriken von sich aus erfassen kann, wenden Sie die Berechtigungen auch auf prometheus-project an.

> [!NOTE]
> Sie können entsprechend mit den Befehlen `oc get role` und `oc get rolebinding` überprüfen, ob die Rolle und Rollenbindung (RoleBinding) richtig erstellt wurden.

## <a name="optional-deploy-example-application"></a>Optional: Bereitstellen einer Beispielanwendung
Alles funktioniert, jedoch sind keine Metrikquellen vorhanden. Wechseln Sie zur Prometheus-URL (https://prom-prometheus-project.apps.*random-id*.*region*.azmosa.io/), die Sie mithilfe des folgenden Befehls finden.
```
oc get route prom -n prometheus-project
```
Denken Sie daran, dem Hostnamen „https://“ voranzustellen.

Die Seite **Status > Service Discovery** (Status > Dienstermittlung) zeigt 0/0 aktive Ziele an.

Führen Sie unter dem Endpunkt „/metrics“ die folgenden Befehle aus, um eine Beispielanwendung bereitzustellen, die grundlegende Python-Metriken verfügbar macht.
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
Die neuen Anwendungen werden auf der Seite „Service Discovery“ (Dienstermittlung) innerhalb von 30 Sekunden nach der Bereitstellung als gültige Ziele angezeigt. Weitere Informationen erhalten Sie auf der Seite **Status > Targets** (Status > Ziele).

> [!NOTE]
> Für jedes erfolgreich ausgelesene Ziel fügt Prometheus einen Datenpunkt in der Metrik „up“ hinzu. Klicken Sie in der linken oberen Ecke auf **Prometheus**, geben Sie „up“ als Ausdruck ein, und klicken Sie auf **Execute** (Ausführen).

## <a name="next-steps"></a>Nächste Schritte

Sie können benutzerdefinierte Prometheus-Instrumentierung zu Ihren Anwendungen hinzufügen.

Die Prometheus-Clientbibliothek, die das Vorbereiten von Prometheus-Metriken vereinfacht, unterstützt verschiedene Programmiersprachen.

 - Java https://github.com/prometheus/client_java
 - Python https://github.com/prometheus/client_python
 - Go https://github.com/prometheus/client_golang
 - Ruby https://github.com/prometheus/client_ruby
