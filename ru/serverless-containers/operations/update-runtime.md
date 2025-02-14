# Изменить режим работы контейнера

{% list tabs group=instructions %}

- CLI {#cli}

    Чтобы изменить [режим работы контейнера](../concepts/container.md#runtime), выполните команду:

    ```bash
    yc serverless container revision deploy \
      --container-id <идентификатор_контейнера> \
      --image <URL_Docker-образа> \
      --service-account-id <идентификатор_сервисного_аккаунта>
      --runtime <режим_работы>
    ```

    Где:
    * `--container-id` — идентификатор контейнера. Чтобы узнать идентификатор, [получите](list.md) список контейнеров.
    * `--image` — URL Docker-образа из {{ container-registry-full-name }}.
    * `--service-account-id` — [идентификатор сервисного аккаунта](../../iam/operations/sa/get-id.md), у которого есть права на скачивание Docker-образа.
    * `--runtime` — режим работы контейнера. Возможные значения: `http`, `task`. Значение по умолчанию: `http`.


    Результат:

    ```text
    done (3s)
    id: bba8faed420g********
    container_id: bbab5i36nvda********
    created_at: "2024-12-13T12:12:09.057Z"
    image:
      image_url: cr.yandex/crpp35hht729********/ubuntu:hello
      image_digest: sha256:f6f34c6cf5f56e594e9828274592f1cc4d9383b1689057ccaf188d41********
    resources:
      memory: "134217728"
      cores: "1"
      core_fraction: "100"
    execution_timeout: 3s
    concurrency: "1"
    service_account_id: aje07l4q4vmo********
    status: ACTIVE
    log_options:
      folder_id: b1gc1t4cb638********
    runtime:
      task: {}
    ```

- API {#api}

  Чтобы изменить [режим работы контейнера](../concepts/container.md#runtime), воспользуйтесь методом REST API [deployRevision](../containers/api-ref/Container/deployRevision.md) для ресурса [Container](../containers/api-ref/Container/index.md) или вызовом gRPC API [ContainerService/DeployRevision](../containers/api-ref/grpc/Container/deployRevision.md).

{% endlist %}