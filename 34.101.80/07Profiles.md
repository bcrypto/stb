# 7 Профили

<!---Вопросы:
trusted token - штампов времени (исходя из контекста)  
подпись == ЭЦП?? а что тогда signature document? смотреть профиль LT.  
опять проблема с отметками времени. Смотреть профиль LTA  
аттестат проверки  
---->

<!---## 6.1 todo
В настоящем стандарте определяется 12 профилей РЭЦП. В данном раздел описываются 4 из них, которые называются базовыми. Остальные находятся в Приложении A данного стандарта.  
В разделе `6.2` объявляются базовые профили РЭЦП: 
- профиль `B` предусматривает требования по включению в РЭЦП подписанных и некоторых неподписанных атрибутов после того, как подпись была создана;  
- профиль `T` предусматривает требования по созданию и включению в существующую подпись штампов времени, доказывающих, что подпись действительно существовала в определенный момент времени;  
- профиль `LT` предусматривает требования по включению в документ подписи всех данных, необходимых для проверки данной подписи. Данный профиль предназначен для решения проблем долгосрочной доступности аттестата проверки(???);  
- `LTA` профиль предусматривает требования по включению в подпись отметок времени, позволяющих проверить подпись в течение длительного периода времени после ее создания. Данный профиль предназначен для решения проблем долгосрочной доступности и целостности аттестата проверки(???).  
  **Замечание 1**
  Профили `T` и `LTA` подходят для случаев, когда необходимо сохранить валидность подписи даже в тот момент времени, когда срок действия СОК, состояние отзыва СОК и/или криптографическая стойкость алгоритма вызывают беспокойство. Конкретный профиль применяется в зависимости от контекста и случая.    
  **Замечание 2**  
  Использование профиля `LTA` считается подходящей техникой хранения и передачи подписанных данных.  --->

## 7.1 Общие положения

В данном разделе определяются 4 базовых профиля: B-B, B-T, B-LT и B-LTA. 
Профиль задается набором атрибутов. Атрибут может быть общим, 
специфичным для формата CAdES и специфичным для формата XAdES. Требования 
по управлению специфичными атрибутами задаются в разделах, посвященным 
форматам.

При реализации РЭЦП определенного формата необходимо использовать лишь 
общие атрибуты и атрибуты, специфичные для выбранного формата. Остальные 
атрибуты СЛЕДУЕТ игнорировать. 

## 7.2 Профиль B-B

РЭЦП профиля B-B ДОЛЖНА содержать следующие общие атрибуты:

- `SigningTime`;
- `SigningCertificate`.

РЭЦП профиля B-B формата XAdES ДОЛЖНА содержать следующие атрибуты, 
специфичные для данного формата:
 
- `DataObjectFormat`.

РЭЦП профиля B-B формата CAdES ДОЛЖНА содержать следующие атрибуты, 
специфичные для данного формата:
 
- `ContentType`;
- `MessageDigest`.

РЭЦП профиля B-B МОЖЕТ содержать следующие общие атрибуты:

- `SignerAttributes`;
- `CommitmentTypeIndication`; 
- `SignerLocation`;
- `CounterSignature`;
- `SignaturePolicyIdentifier`;
- `SignaturePolicyStore`.

РЭЦП профиля B-B формата XAdES МОЖЕТ содержать следующие атрибуты, 
специфичные для данного формата:

- `AllDataObjectsTimeStamp`;
- `IndividualDataObjectsTimeStamp`.

РЭЦП профиля B-B формата CAdES МОЖЕТ содержать следующие атрибуты, 
специфичные для данного формата:

- `ContentHints`;
- `MimeType`;
- `ContentTimeStamp`;
- `ContentReference`;
- `ContentIdentifier`.

В РЭЦП профиля B-B НЕ РЕКОМЕНДУЕТСЯ включать атрибуты
`SignatureTimeStamp`, `CompleteCertificateReferences`, 
`CertificateValues`, `CompleteRevocationReferences`, `RevocationValues`, 
`AttributeCertificateReferences`, `AttributeCertificateValues`, 
`AttributeRevocationReferences`, `AttributeRevocationValues`, 
`RefsOnlyTimeStamp`, `SigAndRefsTimeStamp`, `TimeStampValidationData`, 
`ArchiveTimeStamp`. 

В РЭЦП профиля B-B формата XAdES НЕ РЕКОМЕНДУЕТСЯ включать атрибут 
`RenewedDigest`. 

## 7.3 Профиль B-T

Профиль B-T уточняет и расширяет профиль B-B в следующих аспектах.

РЭЦП профиля B-T ДОЛЖНА содержать следующие атрибуты:

- `SignatureTimeStamp`.

Остальные требования остаются прежними.

## 7.4 Профиль B-LT 

Профиль B-LT уточняет и расширяет профиль B-T в следующих аспектах.

РЭЦП профиля B-LT формата XAdES ДОЛЖНА содержать либо атрибут 
`TimeStampValidationData`, либо СОК и информацию об отзыве для 
штампа времени непосредственно в штампе времени. РЕКОМЕНДУЕТСЯ помещать 
СОК и информацию об отзыве для штампа времени в атрибут 
`TimeStampValidationData` и включать данный атрибут в подпись.

РЭЦП профиля B-LT формата СAdES ДОЛЖНА содержать либо атрибут 
`SignedData.crls.crl`, если вся информация об отзыве представлена в 
формате СОС, либо атрибут `SignedData.crls.other`, если вся 
информация об отзыве представлена в формате OCSP-ответов. 

РЭЦП профиля B-LT НЕ ДОЛЖНА содержать следующие атрибуты:

- `CompleteCertificateReferences`;
- `AttributeCertificateReferences`;
- `SigAndRefsTimeStamp`;
- `RefsOnlyTimeStamp`.

РЭЦП данного профиля формата CAdES и формата XAdES имеют различные 
требования к включению атрибутов `CertificateValues`, 
`AttributeCertificateValues`, `RevocationValues` и `AttributeRevocationValues`. 
Данные атрибуты НЕ ДОЛЖНЫ включаться в РЭЦП 
формата CAdES, однако, МОГУТ включаться в РЭЦП формата XAdES при 
определенных условиях. Условия указаны в соответствующих данным атрибутам 
разделах главы [XAdES]. 

Остальные требования остаются прежними.

## 7.5 Профиль B-LTA

Профиль B-LTA уточняет и расширяет профиль B-LT в следующих аспектах.

РЭЦП профиля B-LTA ДОЛЖНА содержать атрибут `ArchiveTimestamp`.

РЭЦП профиля B-LTA формата XAdES МОЖЕТ содержать атрибут `RenewedDigests`.

Остальные требования остаются прежними.

<!----Сопоставления CAdES и XAdES:
signing-time = SigningTime
commitment-type-indication = CommitnentTypeIndication
signer-location = SignatureProductionPlace/SignerLocation
signer-attributes-v2 = SignerRole/SignerAttributes
countersignature = Countersignature
signature-policyidentifier = SignaturePolicyIdentifier
signature-policy-store = SignaturePolicyStore
signature-time-stamp = SignatureTimestamp
certificate-values = CertificateValues
revocation-values = RevocationValues
complete-revocationreferences = CompleteRevocationReferences
attribute-certificatereferences = AttributeCertificateRefs
attribute-revocationreferences  = AttributeRevocationReferences 
time-stamped-certs-crlsreferences = RefsOnlyTimeStamp(здесь можно еще включать ссылки на атрибутные сертификаты и иноформацию об отзыве атрибутного сертификата)???
-------->
