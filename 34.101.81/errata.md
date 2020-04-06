# Поправка к официальной редакции

| В каком месте | Напечатано | Должно быть |
|---------------|------------|-------------|
| Раздел 3, абзац 1 | В настоящем стандарте применяют термины, установленные в СТБ 34.101.19, ..., ГОСТ 34.973 а также следующие термины с соответствующими определениями: | В настоящем стандарте применяют термины, установленные в СТБ 34.101.19, ..., ГОСТ 34.973, а также следующие термины с соответствующими определениями: |
| Пункт 7.1.2, абзац 2 | `osсpcertid [6] IMPLICIT CertId,` | `ocspcertid [6] IMPLICIT CertId,` |
| Пункт 7.1.2, абзац 2 | `oscpresponce [7] IMPLICIT OCSPResponse,` | `ocspresponce [7] IMPLICIT OCSPResponse,` |
| Пункт 7.1.2, абзац 3 | Компонент `certificate` задает сертификат, а компоненты `esscertid`, `certid` – ссылки на него. | Компонент `certificate` задает сертификат, а компоненты `esscertid`, `ocspcertid` – ссылки на него. |
| Пункт 7.1.2, абзац 7 | Компоненты `pkistatus` и `certstatus` задают результат проверки. | Компоненты `pkistatus` и `ocspcertstatus` задают результат проверки. |
| Пункт 7.1.7, абзац 2 | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), cpkc(3), ccpd(4)}` | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), vpkc(3), ccpd(4)}` |
| Подраздел 7.3, абзац 2 | ...компонент `signerInfos`, вложеннный в `SignedData`... | ...компонент `signerInfos`, вложенный в `SignedData`... |
| Подраздел 7.3, абзац 6 | Вложеннный в `dvCertInfo` компонент `dvStatus`... | Вложенный в `dvCertInfo` компонент `dvStatus`... |
| Приложение A, тип `CertEtcToken` | `osсpcertid [6] IMPLICIT CertId,` | `ocspcertid [6] IMPLICIT CertId,` |
| Приложение A, тип `CertEtcToken` | `oscpresponce [7] IMPLICIT OCSPResponse,` | `ocspresponce [7] IMPLICIT OCSPResponse,` |
| Пункт 7.1.7, тип `ServiceType` | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), cpkc(3), ccpd(4)}` | `ServiceType ::= ENUMERATED {cpd(1), vsd(2), vpkc(3), ccpd(4)}` |

