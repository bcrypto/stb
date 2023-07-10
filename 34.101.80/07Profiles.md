# 7 <a name="Profiles"></a>Профили

## 7.1 <a name="Profiles1"></a>Общие положения

В данном разделе определяются 4 базовых профиля: B-B, B-T, B-LT и B-LTA. Профиль
задается набором атрибутов. Атрибут может быть общим, специфичным для формата
CAdES и специфичным для формата XAdES. Требования по управлению специфичными
атрибутами задаются в разделах, посвященных форматам.

При реализации РЭЦП определенного формата необходимо использовать лишь общие
атрибуты и атрибуты, специфичные для выбранного формата. Остальные атрибуты
СЛЕДУЕТ игнорировать.

## 7.2 <a name="Profiles2"></a>Профиль B-B

РЭЦП профиля B-B ДОЛЖНА содержать следующие общие атрибуты:

- `SigningTime`;
- `SigningCertificate`.

РЭЦП профиля XAdES-B-B МОЖЕТ содержать следующие атрибуты, специфичные для 
данного формата:
 
- `DataObjectFormat`.

При этом подпись ДОЛЖНА содержать атрибут `DataObjectFormat`, если подпись
покрывает объекты неопределенного в настоящем стандарте формата и НЕ ДОЛЖНА,
если формат каждого подписанного объекта определен (подписана другая подпись или
стандартизированные атрибуты).

РЭЦП класса CAdES-B-B ДОЛЖНА содержать следующие атрибуты, 
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

РЭЦП класса XAdES-B-B МОЖЕТ содержать следующие атрибуты, 
специфичные для данного формата:

- `AllDataObjectsTimeStamp`;
- `IndividualDataObjectsTimeStamp`.

РЭЦП класса CAdES-B-B МОЖЕТ содержать следующие атрибуты, 
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

В РЭЦП класса XAdES-B-B НЕ РЕКОМЕНДУЕТСЯ включать атрибут `RenewedDigests`.

## 7.3 <a name="Profiles3"></a>Профиль B-T

Профиль B-T уточняет и расширяет профиль B-B в следующих аспектах.

РЭЦП профиля B-T ДОЛЖНА содержать следующие атрибуты:

- `SignatureTimeStamp`.

Остальные требования остаются прежними.

## 7.4 <a name="Profiles4"></a>Профиль B-LT 

Профиль B-LT уточняет и расширяет профиль B-T в следующих аспектах.

РЭЦП класса XAdES-B-LT ДОЛЖНА содержать либо атрибут `TimeStampValidationData`,
либо СОК и информацию об отзыве для штампа времени непосредственно в штампе
времени. РЕКОМЕНДУЕТСЯ помещать СОК и информацию об отзыве для штампа времени в
атрибут `TimeStampValidationData` и включать данный атрибут в подпись.

РЭЦП класса СAdES-B-LT ДОЛЖНА содержать либо атрибут `SignedData.crls.crl`, если
вся информация об отзыве представлена в формате СОС, либо атрибут
`SignedData.crls.other`, если вся информация об отзыве представлена в формате
OCSP-ответов.

РЭЦП профиля B-LT НЕ ДОЛЖНА содержать следующие атрибуты:

- `CompleteCertificateReferences`;
- `AttributeCertificateReferences`;
- `SigAndRefsTimeStamp`;
- `RefsOnlyTimeStamp`.

РЭЦП данного профиля формата CAdES и формата XAdES имеют различные требования к
включению атрибутов `CertificateValues`, `AttributeCertificateValues`,
`RevocationValues` и `AttributeRevocationValues`. Данные атрибуты НЕ ДОЛЖНЫ
включаться в РЭЦП формата CAdES, однако, МОГУТ включаться в РЭЦП формата XAdES
при определенных условиях. Условия указаны в соответствующих данным атрибутам
частях раздела [10](10XADES.md).

Остальные требования остаются прежними.

## 7.5 <a name="Profiles5"></a>Профиль B-LTA

Профиль B-LTA уточняет и расширяет профиль B-LT в следующих аспектах.

РЭЦП профиля B-LTA ДОЛЖНА содержать атрибут `ArchiveTimeStamp`.

РЭЦП класса XAdES-B-LTA МОЖЕТ содержать атрибут `RenewedDigests`.

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
time-stamped-certs-crlsreferences = RefsOnlyTimeStamp 
(здесь можно еще включать ссылки на атрибутные сертификаты 
и информацию об отзыве атрибутного сертификата)???
-------->
