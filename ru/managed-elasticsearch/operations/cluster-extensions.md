# Управление расширениями {{ ES }}

Пользовательские расширения это любые текстовые данные (словари слов, переносов и т. п.), ключи для интеграции с другими кластерами, прочие данные для работы кластера. Подробнее см. в [документации {{ ES }}](https://www.elastic.co/guide/en/cloud/current/ec-plugins-guide.html).

## Получить список установленных пользовательских расширений {#list}

{% list tabs %}

* CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы получить список расширений кластера, выполните команду:

    ```bash
    {{ yc-mdb-es }} extensions list <идентификатор или имя кластера>
    ```

    Идентификатор и имя кластера можно [получить со списком кластеров в каталоге](cluster-list.md#list-clusters).

* API

    Воспользуйтесь методом API [list](../api-ref/Extension/list) и передайте в запросе идентификатор кластера в параметре `clusterId`.

    Идентификатор кластера можно [получить со списком кластеров в каталоге](cluster-list.md#list-clusters).

{% endlist %}

## Добавить пользовательское расширение {#add}

{% list tabs %}

* CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы добавить расширение в кластер, выполните команду:

    ```bash
    {{ yc-mdb-es }} extensions create <идентификатор или имя кластера> \
       --name <имя расширения> \
       --uri <URI zip-архива с расширением>
    ```

    Идентификатор и имя кластера можно [получить со списком кластеров в каталоге](cluster-list.md#list-clusters), идентификатор расширения — [со списком расширений в кластере](#list).

    Чтобы получить ссылку на zip-архив с файлами расширения в {{ objstorage-full-name }}, [воспользуйтесь инструкцией](../../storage/operations/objects/link-for-download.md). Доступ к {{ objstorage-full-name }} [можно настроить](./s3-access.md) с помощью сервисного аккаунта.

    {% include [Terraform timeouts](../../_includes/mdb/mes/terraform/timeouts.md) %}

* API

    Воспользуйтесь методом API [create](../api-ref/Extension/create) и передайте в запросе:

    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
    * Имя расширения в параметре `name`.
    * [Ссылку](../../storage/operations/objects/link-for-download.md) на zip-архив с файлами расширения в {{ objstorage-full-name }} в параметре `uri`. Доступ к {{ objstorage-full-name }} [можно настроить](./s3-access.md) с помощью сервисного аккаунта.
    * Статус пользовательского расширения в параметре `disabled`. После добавления оно будет выключено при значении `true` и включено при значении `false`.

{% endlist %}

## Включить или выключить пользовательское расширение {#update}

{% list tabs %}

* CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы включить или выключить расширение, выполните команду:

    ```bash
    {{ yc-mdb-es }} extensions update <идентификатор расширения> \
       <идентификатор или имя кластера> \
       --active <статус расширения: true|false>
    ```

    Идентификатор и имя кластера можно [получить со списком кластеров в каталоге](cluster-list.md#list-clusters), идентификатор расширения — [со списком расширений в кластере](#list).

    Чтобы включить пользовательское расширение, передайте в параметре `--active` значение `true`, чтобы выключить — `false`.

* API

    Воспользуйтесь методом API [update](../api-ref/Extension/update) и передайте в запросе:

    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
    * Идентификатор пользовательского расширения в параметре `extensionId`. Чтобы узнать идентификатор, [получите список установленных пользовательских расширений](#list).
    * Статус пользовательского расширения в параметре `active`: `true` — включено, `false` — выключено.

{% endlist %}

## Удалить пользовательское расширение {#delete}

{% list tabs %}

* CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы удалить расширение, выполните команду:

    ```bash
    {{ yc-mdb-es }} extensions delete <идентификатор расширения> \
       <идентификатор или имя кластера>
    ```

    Идентификатор и имя кластера можно [получить со списком кластеров в каталоге](cluster-list.md#list-clusters), идентификатор расширения — [со списком расширений в кластере](#list).

* API

    Воспользуйтесь методом API [delete](../api-ref/Extension/delete) и передайте в запросе:

    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
    * Идентификатор пользовательского расширения в параметре `extensionId`. Чтобы узнать идентификатор, [получите список установленных пользовательских расширений](#list).

{% endlist %}