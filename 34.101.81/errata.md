# Поправка к официальной редакции

| В каком месте | Напечатано | Должно быть |
|---------------|------------|-------------|
| Раздел 3, абзац 1 | В настоящем стандарте применяют термины, установленные в СТБ 34.101.19, ..., ГОСТ 34.973 а также следующие термины с соответствующими определениями: | В настоящем стандарте применяют термины, установленные в СТБ 34.101.19, ..., ГОСТ 34.973, а также следующие термины с соответствующими определениями: |
| Пункт 6.9 | СОК СЗД ДОЛЖЕН включать расширение `ExtKeyUsageSyntax` (см. СТБ 34.101.19 (пункт 6.2.11.2)) | СОК СЗД ДОЛЖЕН включать расширение `ExtKeyUsageSyntax` (см. СТБ 34.101.19 (пункт 6.2.1.12)) |
| Пункт 6.12 | СЗД ДОЛЖНА проверять действительность своего и заверяемых СОК по правилам СТБ 34.101.19 и СТБ 34.101.80. | 6.12 СЗД ДОЛЖНА проверять действительность своего и заверяемых СОК по правилам СТБ 34.101.19. |
| Пункт 7.1.5, абзац 2 | `osсpcertid [6] IMPLICIT CertId,` | `ocspcertid [6] IMPLICIT CertId,` |
| Пункт 7.1.5, абзац 2 | `oscpresponce [7] IMPLICIT OCSPResponse,` | `ocspresponce [7] IMPLICIT OCSPResponse,` |
| Пункт 7.1.5, абзац 3 | Компонент `certificate` задает сертификат, а компоненты `esscertid`, `certid` – ссылки на него. | Компонент `certificate` задает сертификат, а компоненты `esscertid`, `ocspcertid` – ссылки на него. |
| Пункт 7.1.5, абзац 7 | Компоненты `pkistatus` и `certstatus` задают результат проверки. | Компоненты `pkistatus` и `ocspcertstatus` задают результат проверки. |
| Пункт 7.1.7, абзац 2 | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), cpkc(3), ccpd(4)}` | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), vpkc(3), ccpd(4)}` |
| Пункт 7.1.10, предпоследний абзац | Компонент `transactionStatus` определяет результат обработки запроса. В случае ошибки вложенный в `transactionStatus` компонент `status` ДОЛЖЕН принимать значение `rejection`, а необязательный компонент `failureInfo` – одно из следующих значений: `badRequest` (запрос не разрешен или не поддерживается), `badTime` (время в запросе недостаточно близко к текущему), `badDataFormat` (неверный формат), `wrongAuthority` (служба, указанная в запросе, отличается от службы, давшей ответ), `incorrectData` (данные клиента некорректны). | Компонент `transactionStatus` определяет результат обработки запроса. В случае ошибки вложенный в `transactionStatus` компонент `status` ДОЛЖЕН принимать значение `rejection`, а необязательный компонент `failureInfo` – быть комбинацией следующих значений: `badRequest` (запрос не разрешен или не поддерживается), `badTime` (время в запросе недостаточно близко к текущему), `badDataFormat` (неверный формат), `wrongAuthority` (служба, указанная в запросе, отличается от службы, давшей ответ), `incorrectData` (данные клиента некорректны). Комбинация задается строкой битов (`BIT STRING`), в которой единичные биты могут устанавливаться в позициях с номерами 2 (`badRequest`), 3 (`badTime`), 5(`badDataFormat`) 6 (`wrongAuthority`) и 7(`incorrectData`). |
| Подраздел 7.3, абзац 2 | ...компонент `signerInfos`, вложеннный в `SignedData`... | ...компонент `signerInfos`, вложенный в `SignedData`... |
| Подраздел 7.3, абзац 6 | Вложеннный в `dvCertInfo` компонент `dvStatus`... | Вложенный в `dvCertInfo` компонент `dvStatus`... |
| Приложение A, тип `CertEtcToken` | `osсpcertid [6] IMPLICIT CertId,` | `ocspcertid [6] IMPLICIT CertId,` |
| Приложение A, тип `CertEtcToken` | `oscpresponce [7] IMPLICIT OCSPResponse,` | `ocspresponce [7] IMPLICIT OCSPResponse,` |
| Приложение A, тип `ServiceType` | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), cpkc(3), ccpd(4)}` | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), vpkc(3), ccpd(4)}` |
| Приложение A, инструкция `IMPORTS` | `GeneralName, PolicyInformation FROM PKIX1Implicit88` | `GeneralName, GeneralNames, PolicyInformation FROM PKIX1Implicit88` |
| Приложение A, инструкция `IMPORTS` | `ContentInfo FROM CryptographicMessageSyntax` | `ContentInfo, DigestAlgorithmIdentifier FROM CryptographicMessageSyntax` |

