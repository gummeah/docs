| Услуга                                  | Цена за ГБ в месяц,<br>вкл. НДС                                        |
| ----- | ----: |
| Хранилище на сетевых HDD-дисках         | {{ sku|KZT|mdb.cluster.network-hdd.kafka|month|string }}               |
| Хранилище на нереплицируемых SSD-дисках | {{ sku|KZT|mdb.cluster.network-ssd-nonreplicated.kafka|month|string }} |
| Хранилище на сетевых SSD-дисках         | {{ sku|KZT|mdb.cluster.network-nvme.kafka|month|string }}              |
| Сверхбыстрое сетевое хранилище с тремя репликами (SSD) | {{ sku|KZT|mdb.cluster.network-ssd-io-m3.kafka|month|string }} |
| Хранилище на локальных SSD-дисках^*^ | {{ sku|KZT|mdb.cluster.local-nvme.kafka|month|string }}               |


^*^ Хранилище выделенных хостов оплачивается по тарифам, указанным в [документации {{ compute-full-name }}](../../compute/pricing.md#prices).
