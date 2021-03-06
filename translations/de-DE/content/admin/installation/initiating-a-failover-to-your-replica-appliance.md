---
title: Failover zu Ihrer Replikat-Appliance initiieren
intro: 'Sie können an der Befehlszeile zu Wartungs- und Testzwecken oder beim Fehlschlagen der primären Appliance ein Failover zu einer {% data variables.product.prodname_ghe_server %}-Replikat-Appliance durchführen.'
redirect_from:
  - /enterprise/admin/installation/initiating-a-failover-to-your-replica-appliance
versions:
  enterprise-server: '*'
---

Die für das Failover erforderliche Zeit hängt davon ab, wie lange es dauert, das Replikat manuell hochzustufen und den Traffic weiterzuleiten. Die Durchschnittszeit beträgt zwischen 2 und 10 Minuten.

{% data reusables.enterprise_installation.promoting-a-replica %}

1. Versetzen Sie die primäre Appliance in den Wartungsmodus, um den Abschluss der Replikation zuzulassen, bevor Sie die Appliances wechseln.
    - Informationen zum Verwenden der Managementkonsole finden Sie unter  „[Wartungsmodus aktivieren und planen](/enterprise/admin/guides/installation/enabling-and-scheduling-maintenance-mode/)“.
    - Darüber hinaus können Sie den Befehl `ghe-maintenance -s` verwenden.
      ```shell
      $ ghe-maintenance -s
      ```
2. Wenn die Anzahl der aktiven Git-Vorgänge null erreicht, sollten Sie 30 Sekunden lang warten.
3. Führen Sie den Befehl `ghe-repl-status -vv` aus, um zu verifizieren, dass alle Replikationskanäle `OK` ausgeben.
  ```shell
  $ ghe-repl-status -vv
  ```
4. Führen Sie den Befehl `ghe-repl-promote` aus, um die Replikation anzuhalten und die Replikations-Appliance auf den primären Status hochzustufen. Dadurch wird der primäre Knoten automatisch in den Wartungsmodus versetzt, sofern er erreichbar ist.
  ```shell
  $ ghe-repl-promote
  ```
5. Aktualisieren Sie den DNS-Eintrag so, dass er auf die IP-Adresse des Replikats verweist. Nach dem Verstreichen des TTL-Zeitraums wird der Traffic an das Replikat geleitet. Stellen Sie bei der Verwendung eines Load-Balancers sicher, dass er so konfiguriert ist, den Traffic an das Replikat zu senden.
6. Benachrichtigen Sie die Benutzer, dass sie die normalen Vorgänge wieder aufnehmen können.
7. Richten Sie bei Bedarf die Replikation von der neuen primären Instanz auf die bestehenden Appliances und die vorherige primäre Instanz ein. Weitere Informationen finden Sie unter „[Informationen zur Hochverfügbarkeitskonfiguration](/enterprise/{{ currentVersion }}/admin/guides/installation/about-high-availability-configuration/#utilities-for-replication-management)“.

### Weiterführende Informationen

- „[Dienstprogramme zur Replikationsverwaltung](/enterprise/{{ currentVersion }}/admin/guides/installation/about-high-availability-configuration/#utilities-for-replication-management)“
