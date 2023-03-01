# Публичное соединение

Публичное соединение (public connection) обеспечивает доступ к [публичным сервисам](#pub-svc-list) {{ yandex-cloud }}. Публичное соединение организуется внутри [транка](./trunk.md) и имеет свой идентификатор — **VLAN-ID**. В транке может быть только одно публичное соединение через которое можно предоставить доступ к любой комбинации [сервисов](#pub-svc-list).

## Список публичных сервисов {#pub-svc-list}

У каждого сервиса в {{ yandex-cloud }} есть своя точка входа — `API Endpoint`. Ознакомиться со списком точек входа сервисов {{ yandex-cloud }} можно по [ссылке](https://{{ api-host }}/endpoints). У каждой точки входа есть свой идентификатор и адрес, который состоит из `FQDN API Endpoint` и номера порта на который этот сервис получает запросы.

Список сервисов {{ yandex-cloud }} к которым предоставляется доступ через публичное соединение:

{% include [pub-svc-list](../../_includes/interconnect/pub-svc-list.md) %}

Фактически, публичное соединение обеспечивает связность между вашей инфраструктурой и IP-адресом в который преобразуется `FQDN API Endpoint` соответствующего сервиса. Преобразование FQDN имени в IP-адрес выполняется через службу DNS.

Например, если вы хотите получить доступ из своей инфраструктуры в сервис [{{ objstorage-name }}](../../storage/) через публичное соединение, то со стороны оборудования {{ yandex-cloud }} в направлении вашего маршрутизатора по протоколу BGP будет анонсироваться префикс `213.180.193.243/32`, который соответствует FQDN API Endpoint `{{ s3-storage-host }}` для сервиса [{{ objstorage-name }}](../../storage/).

Вам необходимо настроить маршрутизацию трафика в собственной инфраструктуре таким образом, чтобы трафик к [публичным сервисам](#pub-svc-list) {{ yandex-cloud }} направлялся к устройствам, выполняющим [функции NAT](#pub-nat) для публичного соединения.


## Стыковая подсеть {#pub-address}

Публичное соединение организуется с использованием публичных IPv4-адресов из [адресного пула](../../vpc/concepts/ips.md) {{ yandex-cloud }}, которые маршрутизируются в интернет. При организации публичного соединения вам выделяется **стыковая подсеть** (point-to-point) размером `/31`.

{% note alert %}

[Cервисы](#pub-svc-list), к которым предоставляется доступ через публичное соединение, размещаются [в собственных ДЦ](../../overview/concepts/geo-scope.md). Трафик внутри публичного соединения между вашей инфраструктурой и [публичными сервисами](#pub-svc-list) не покидает периметр {{ yandex-cloud }}, несмотря на использование публичных IP-адресов.

{% endnote %}

{% include [bgp](../../_includes/interconnect/bgp.md) %}

## Функции NAT {#pub-nat}

Для публичного соединения необходимо использовать технологию трансляции сетевых адресов [NAT](https://ru.wikipedia.org/wiki/NAT) на стороне вашего оборудования. Допускаются следующие варианты реализации NAT:

* NAT-функция выполняется на вашем оборудовании (маршрутизаторе), к которому подключается [транк](./trunk.md). Весь трафик публичного соединения отправляется от IPv4-адреса на интерфейсе маршрутизатора в [стыковой подсети](#pub-address). В этом случае никаких анонсов префиксов по протоколу BGP со стороны маршрутизатора не требуется.

* NAT-функция выполняется на вашем оборудовании, которое не используется для подключения [транка](./trunk.md), например, сервере или межсетевом экране. В данном случае вам выделяется дополнительная публичная IPv4-подсеть размером `/30` и ваш маршрутизатор, к которому подключается [транк](./trunk.md), должен будет анонсировать префикс этой дополнительной подсети по протоколу BGP в сторону оборудования {{ yandex-cloud }}.

