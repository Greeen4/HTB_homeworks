### SL-**[[PI]]**-**[[IOS]]**-MISCONFIG-**[[VN]]**: не настроен механизм App Transport Security

>Критичность: info

**Описание**
В Приложении отключен механизм App Transport Security.

**Последовательность действий для демонстрации недостатка**
1. Изменить расширение установочного файла приложения с .ipa на .zip и распаковать как архив. Перейти в каталог /Payload/**[[App name]]**.app.
2. Убедиться, что в секции NSAppTransportSecurity файла Info.plist присутствует параметр NSAllowsArbitraryLoads, при этом отсутствует параметр NSExceptionDomains, что свидетельствует о глобальном отключении механизма App Transport Security

**Анализ критичности**

Механизм App Transport Security накладывает ряд ограничений при совершении соединений с использованием NSURLConnection, NSURLSession и CFUR
- Запрещаются HTTP-соединения.
- Сертификат X.509 сервера должен иметь отпечаток SHA-256 и должен быть подписан как минимум 2048-битным RSA-ключом или 256-битным ECC-ключом.
- Должна использоваться версия TLS не ниже 1.2 с поддержкой PFS и одним из следующих шифров:
 - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
 - TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
 - TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
 - TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
 - TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
 - TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
 - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
 - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
 - TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
 - TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
 - TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA

Недостаток не является уязвимостью, однако использование механизма App Transport Security повышает уровень защищённости сетевого соединения и соответствует требованиям «best practices». Кроме того, Apple планирует требовать обязательного обоснования необходимости глобального отключения механизма App Transport Security при проверке перед публикацией приложения в AppStore. 

Критичность недостатка оценивается как **информационная.**

**Рекомендации по устранению**

Для устранения недостатка рекомендуется настроить механизм App Transport Security для всех подконтрольных доменов, а для доменов сторонних сервисов добавить соответствующие исключения (если они не соответствуют всем требованиям App Transport Security).
