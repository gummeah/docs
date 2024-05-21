# Как начать работать с профилем WAF

{% include [note-preview-waf](../../_includes/smartwebsecurity/note-preview-waf.md) %}

Для защиты ваших веб-приложений от внешних угроз в {{ sws-full-name }} реализован межсетевой экран [Web Application Firewall (WAF)](../../glossary/waf.md). 

Создайте первый [профиль WAF](../concepts/waf.md) и подключите его к имеющемуся [профилю безопасности](../concepts/profiles.md) {{ sws-full-name }}.

Если у вас еще не настроен профиль безопасности, создайте его и подключите к [виртуальному хосту](../../application-load-balancer/concepts/http-router.md#virtual-host) L7-балансировщика {{ alb-full-name }}. Подробнее см. [{#T}](../quickstart.md).

Чтобы начать работу с WAF:
1. [Создайте профиль WAF](#profile-create).
1. [Настройте набор базовых правил](#configure-rules).
1. [Создайте правило-исключение](#create-exclusion).
1. [Подключите профиль WAF к профилю безопасности](#profile-connect).

## Подготовьте облако к работе {#before-you-begin}

{% include [before-you-begin](../../_tutorials/_tutorials_includes/before-you-begin.md) %}

## Создайте профиль WAF {#profile-create}

{% list tabs group=instructions %}

- Консоль управления {#console}

  1. В [консоли управления]({{ link-console-main }}) выберите каталог, в котором вы хотите создать профиль WAF.
  1. В списке сервисов выберите **{{ ui-key.yacloud.iam.folder.dashboard.label_smartwebsecurity }}**.
  1. Перейдите на вкладку ![image](../../_assets/smartwebsecurity/waf.svg) **Профили WAF** и нажмите кнопку **Создать профиль WAF**.
  1. Опишите сценарий использования функциональности WAF в своих проектах и нажмите кнопку **Отправить заявку**.

      После одобрения заявки вы сможете перейти к созданию профиля WAF.
  1. Введите имя профиля, например `test-waf-profile-1`.
  1. По умолчанию в профиле WAF включен набор базовых правил [OWASP Core Rule Set](https://coreruleset.org/). Чтобы посмотреть правила, которые включены в набор, нажмите на строку с его описанием.
  1. Нажмите кнопку **Создать**.

{% endlist %}

## Настройте набор базовых правил {#configure-rules}

{% list tabs group=instructions %}

- Консоль управления {#console}

  1. На открывшейся обзорной странице профиля WAF нажмите кнопку **Настроить набор базовых правил**.
  1. Установите необходимый **Порог аномальности** — суммарную [аномальность](../concepts/waf.md#anomaly) сработавших правил, при которой запрос будет заблокирован, например `Умеренный - 25 и выше`.

      Рекомендуется начинать с порога аномальности `25` и постепенно снижать его до `5`. Чтобы снизить порог аномальности, отработайте ложные срабатывания WAF на легитимные запросы. Для этого подберите правила из базового набора и настройте [правила-исключения](#create-exclusion). Также для тестирования разных порогов аномальности используйте в профиле безопасности режим **Только логирование (dry-run)**.

  1. Установите необходимый **Уровень паранойи**, например `2 и менее`. 

      [Уровень паранойи](../concepts/waf.md#paranoia) классифицирует правила по степени агрессивности. Чем выше уровень паранойи, тем лучше уровень защиты, но и больше вероятность ложных срабатываний WAF. 
  1. Проверьте включенные в набор правила, при необходимости включите дополнительные или уберите ненужные. При работе с правилами обращайте внимание на значение их аномальности и уровень паранойи. 

  Любое правило из набора можно сделать блокирующим. Запрос, соответствующий таком правилу, будет заблокирован независимо от установленного порога аномальности. Чтобы сделать правило блокирующим, нажмите ![image](../../_assets/console-icons/ban.svg) справа от него. Если в профиле безопасности включен режим **Только логирование (dry-run)**, запросы не будут заблокированы, даже если они соответствуют блокирующим правилам.

{% endlist %}

## Создайте правило-исключение {#create-exclusion}

{% list tabs group=instructions %}

- Консоль управления {#console}

  1. Перейдите на вкладку ![image](../../_assets/console-icons/file-xmark.svg) **Правила-исключения** и нажмите кнопку **Создать правило-исключение**.
  1. Введите имя [правила-исключения](../concepts/waf.md#exclusion-rules), например `exception-rule-1`.
  1. В блоке **Область применения** укажите правила из базового набора, для которых будет срабатывать исключение. Вы можете выбрать **Все правила** или указать конкретные.
  1. В блоке **Условия на трафик** выберите [условия](../concepts/conditions.md) для срабатывания правила-исключения.

      Если оставить поле **Условия** пустым, правило-исключение будет применено ко всему трафику.
  1. Нажмите кнопку **Создать**.

{% endlist %}

## Подключите профиль WAF к профилю безопасности {#profile-connect}

{% list tabs group=instructions %}

- Консоль управления {#console}

  1. Перейдите на вкладку ![image](../../_assets/console-icons/shield-check.svg) **Профили безопасности**.
  1. В списке выберите профиль безопасности, к которому вы хотите подключить профиль WAF, например `test-sp1`.
  1. Нажмите кнопку ![plus-sign](../../_assets/console-icons/plus.svg) **{{ ui-key.yacloud.smart-web-security.form.button_add-rule }}**.
  1. Введите имя правила, например `waf-rule-1`.
  1. В поле **Приоритет** задайте значение выше, чем у правил Smart Protection, уже имеющихся в профиле безопасности, например `888800`.
  1. (Опционально) Чтобы протестировать профиль WAF и отработать ложные срабатывания на легитимные запросы, используйте в профиле безопасности режим **Только логирование (dry-run)**.
  1. В поле **Тип правила** выберите **Web Application Firewall**.
  1. В поле **Профиль WAF** выберите `test-waf-profile-1`, созданный ранее.
  1. В поле **Действие** выберите **Полная защита**.
  1. При необходимости задайте [условия](../concepts/conditions.md) для сопоставления трафика.
  1. Нажмите кнопку **Добавить**.

{% endlist %}

### См. также {#see-also}

* [{#T}](../quickstart.md)
* [{#T}](../concepts/waf.md)