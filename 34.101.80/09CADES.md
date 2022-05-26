# 9 <a name="Cades"></a>Формат CAdES

## 9.1 <a name="Cades1"></a>Общие положения

Формат CAdES определяет синтаксис РЭЦП и правила включения атрибутов, описанных
в разделе [6](06Attrs.md).

Данные формата CAdES — это подписанные данные, синтаксис которых определен в СТБ
34.101.23 (разделы 8 и 15). Данные описываются с помощью языка АСН.1. Для
кодирования данных ДОЛЖНЫ использоваться базовые (BER) или отличительные (DER)
правила. В большинстве случаев разрешается использовать базовые правила.
Некоторые атрибуты, сертификаты, списки и аттестаты отзыва и другие элементы
должны кодироваться с помощью отличительных правил.

Некоторые типы АСН.1 являются динамическими, т.е. фактический тип элемента
определяется во время декодирования и зависит от значения специального
компонента, содержащего идентификатор типа. Такой прием используется для
определения типов атрибутов.

<!--- 5.4; 4.4 -->
Подписанные данные представляются типом АСН.1 `SignedData`, определенным в СТБ
34.101.23 (пункт 8.2). Значение этого типа вкладывается в контейнер
`ContentInfo`, определенный в СТБ 34.101.23 (раздел 6), как компонент `content`.
<!--- 5.3; 4.3 -->
Значение компонента `contentType` ДОЛЖНО содержать идентификатор
`id-signedData`.

В отличие от формата XAdES, позволяющего подписывать несколько объектов, в
качестве подписываемого содержимого РЭЦП формата CAdES может выступать только
один документ.
<!--- 5.5; 4.5 -->
Идентификатор типа подписываемых данных указывается в компоненте `eContentType`
типа `EncapsulatedContentInfo`, определенной в СТБ 34.101.23 (пункт 8.3).
<!--- 5.2; 4.2 -->
В качестве значения компонента `eContentType` МОЖЕТ выступать идентификатор
`id-data`, определенный в СТБ 34.101.23 (раздел 7). В этом случае РЕКОМЕНДУЕТСЯ
кодировать содержимое с помощью формата MIME и использовать тип MIME для
определения формата представления данных.

Согласно СТБ 34.101.23 компонент `eContent` является необязательным. При
наличии, он содержит подписанный документ, и подпись является обертывающей.
Иначе подписанный документ хранится отдельно, а подпись является отделенной.

<!--- 5.6; 4.6 -->
Тип `SignedData` позволяет сохранить несколько РЭЦП, выработанных различными
подписантами. Значение РЭЦП одного подписанта представляется типом `SignerInfo`,
определенным в СТБ 34.101.23 (пункт 8.4). Подписанные атрибуты указываются в
компоненте `signedAttrs`. Набор подписанных атрибутов ДОЛЖЕН включать атрибуты
`ContentType` и `MessageDigest`. Неподписанные атрибуты указываются в компоненте
`unsignedAttrs`.

Отнесение атрибута к подписанту, документу или подписи является логическим, а не
имеет синтаксического отражения в РЭЦП формата CAdES.

## 9.2 <a name="Cades2"></a>Синтаксис базовых атрибутов РЭЦП

### 9.2.1 <a name="Cades21"></a>Атрибут `ContentType`
<!--- 5.7.1; 5.1.1 -->

Синтаксис и правила включения атрибута `ContentType` определены в СТБ 34.101.23
(пункт 15.2).
<!--- CMS (RFC 3852 [4]), пункт 11.1. -->

>Примечание — Значение атрибута `ContentType` ДОЛЖНО соответствовать значению
компонента `eContentType` типа `EncapsulatedContentInfo`.

### 9.2.2 <a name="Cades22"></a>Атрибут `MessageDigest`
<!--- 5.7.2; 5.1.2 -->

Синтаксис и правила включения атрибута `MessageDigest` определены в СТБ
34.101.23 (пункт 15.3).
<!--- CMS (RFC 3852 [4]). -->

### 9.2.3 <a name="Cades23"></a>Атрибут `SigningTime`
<!--- 5.9.1; 5.2.1 -->

Синтаксис и правила включения атрибута `SigningTime` определены в СТБ 34.101.23
(пункт 15.4).

### 9.2.4 <a name="Cades24"></a>Атрибут `SigningCertificate`
<!--- 5.7.3; 5.2.2 -->

В зависимости от используемого алгоритма хэширования атрибут
представляется одним из двух типов АСН.1: `SigningCertificate` или 
`SigningCertificateV2`. 

При использовании алгоритма хэширования SHA-1, определенного в
[[4]](99Biblio.md#SHA1)], ДОЛЖЕН применяться тип `SigningCertificate`, а
атрибуту назначается следующий идентификатор:

    id-aa-signingCertificate OBJECT IDENTIFIER ::= {
        iso(1) member-body(2) us(840) rsadsi(113549)
            pkcs(1) pkcs9(9) smime(16) id-aa(2) 12
    }

При использовании алгоритмов хэширования, отличных от SHA-1, ДОЛЖЕН применяться
тип `SigningCertificateV2`, а атрибуту назначается следующий идентификатор:

    id-aa-signingCertificateV2 OBJECT IDENTIFIER ::= {
        iso(1) member-body(2) us(840) rsadsi(113549)
            pkcs(1) pkcs9(9) smime(16) id-aa(2) 47
    }

Тип `SigningCertificate` определен в [[17]](99Biblio.md#ESS).
Тип `SigningCertificateV2` определен в [[21]](99Biblio.md#ESSUPD).

    SigningCertificate ::=  SEQUENCE {
        certs        SEQUENCE OF ESSCertID,
        policies     SEQUENCE OF PolicyInformation OPTIONAL
    }
    ESSCertID ::= SEQUENCE {
         certHash       Hash,
         issuerSerial   IssuerSerial OPTIONAL
    }
    SigningCertificateV2 ::=  SEQUENCE {
        certs       SEQUENCE OF ESSCertIDv2,
        policies    SEQUENCE OF PolicyInformation OPTIONAL
    }
    id-sha256  OBJECT IDENTIFIER  ::=  { 
        joint-iso-itu-t(2) country(16) us(840) organization(1) gov(101)
            csor(3) nistalgorithm(4) hashalgs(2) 1
    }
    ESSCertIDv2 ::= SEQUENCE {
         hashAlgorithm  AlgorithmIdentifier DEFAULT {algorithm id-sha256},
         certHash       Hash,
         issuerSerial   IssuerSerial OPTIONAL
    }
    Hash ::= OCTET STRING
    IssuerSerial ::= SEQUENCE {
         issuer         GeneralNames,
         serialNumber   CertificateSerialNumber
    }

Компонент `certs` содержит ссылки на сертификаты, используемые при проверке
подписи. Ссылка на сертификат представляется типом `ESSCertID` или `ESSCertIDv2`
в зависимости от алгоритма хэширования. Первой в списке должна следовать ссылка
на сертификат подписанта, при кодировании которой СЛЕДУЕТ указывать компонент
`issuerSerial`.

Компонент `policies` НЕ ДОЛЖЕН использоваться.

Хэш-значение, вычисленное от кодового представления сертификата, полученного с
использованием отличительных правил, указывается в компоненте `certHash`.

В типах `ESSCertID` и `ESSCertIDv2` компонент `issuerSerial` не является точной
ссылкой на сертификат, а только подсказкой для его определения. Точной ссылкой
является хэш-значение, заданное в компоненте `certHash`.

>Примечание — Введение двух типов для описания атрибута `SigningCertificate`
позволяет устранить недостатки при изначальном проектировании и поддержать
совместимость с устаревшими реализациями. В тех случаях когда совместимость не
нужна, следует избегать использования в атрибуте алгоритма хэширования SHA-1,
признанного криптографически нестойким.

### 9.2.5 <a name="Cades25"></a>Атрибут `CommitmentTypeIndication`
<!--- 5.11.1; 5.2.3 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-commitmentType OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 16
    }

Синтаксис атрибута описывается типом `CommitmentTypeIndication`:

    CommitmentTypeIndication ::= SEQUENCE {
        commitmentTypeId CommitmentTypeIdentifier,
        commitmentTypeQualifier SEQUENCE SIZE (1..MAX) 
                        OF CommitmentTypeQualifier OPTIONAL
    }
    CommitmentTypeIdentifier ::= OBJECT IDENTIFIER
    CommitmentTypeQualifier ::= SEQUENCE {
        commitmentTypeIdentifier CommitmentTypeIdentifier, 
        qualifier   ANY DEFINED BY commitmentTypeIdentifier
    }

Компонент `commitmentTypeId` содержит идентификатор обязательства.

Возможные значения компонента `commitmentTypeQualifier` типа
`CommitmentTypeIndication` не рассматриваются в настоящем стандарте.

Типам обязательств, объявленным при определении атрибута, 
назначаются идентификаторы:

    id-cti-ets-arc OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) cti(6)
    }
    id-cti-ets-proofOfOrigin OBJECT IDENTIFIER ::= { id-cti-ets-arc 1 }
    id-cti-ets-proofOfReceipt OBJECT IDENTIFIER ::= { id-cti-ets-arc 2 }
    id-cti-ets-proofOfDelivery OBJECT IDENTIFIER ::= { id-cti-ets-arc 3}
    id-cti-ets-proofOfSender OBJECT IDENTIFIER ::= { id-cti-ets-arc 4}
    id-cti-ets-proofOfApproval OBJECT IDENTIFIER ::= { id-cti-ets-arc 5}
    id-cti-ets-proofOfCreation OBJECT IDENTIFIER ::= { id-cti-ets-arc 6}

Этот компонент может содержать только распознаваемые типы обязательств.

### 9.2.6 <a name="Cades26"></a>Атрибут `ContentHints`
<!--- 5.10.3; 5.2.4.1 -->

Атрибуту назначается следующий идентификатор:

    id-aa-contentHint OBJECT IDENTIFIER ::= { iso(1) member-body(2) us(840)
        rsadsi(113549) pkcs(1) pkcs-9(9) smime(16) id-aa(2) 4}

Синтаксис атрибута описывается типом `ContentHints`, определенным в 
[[ESS]](99Biblio.md#ESS). 

    ContentHints ::= SEQUENCE {
        contentDescription UTF8String (SIZE (1..MAX)) OPTIONAL,
        contentType ContentType
    }

Если этот атрибут используется для указания точного формата данных,
представляемых пользователю, применяются следующие правила:

- компонент `contentType` показывает тип связанного содержимого; это
идентификатор объекта (т. е. уникальная строка целых чисел), назначенная
службой, определяющей тип содержимого;
- если значением компонента `contentType` является идентификатор `id-data`,
определенный в СТБ 34.101.23, то компонент `contentDescription` должен
определять формат представления данных, который может быть задан типами MIME.

Если формат содержимого задан с помощью типов MIME, применяются следующие
правила:

- компонент `contentType` должен иметь значение `id-data`;
- компонент `contentDescription` должен использоваться для указания кодировки
данных в соответствии с правилами, определенными в [[16]](99Biblio.md#MIME).
<!--- Пример структурированного содержимого и типов MIME см. в приложении Е. -->

### 9.2.7 <a name="Cades27"></a>Атрибут `MimeType`
<!--- ; 5.2.4.2 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-mimeType OBJECT IDENTIFIER ::= { itu-t(0) identified-organization(4) etsi(0)
        electronic-signature-standard (1733) attributes(2) 1 }

Синтаксис атрибута описывается типом `MimeType`:

    MimeType::= UTF8String

Атрибут содержит MIME-тип подписанных данных. Значение атрибута должно
формироваться по правилам, определенным в [[16]](99Biblio.md#MIME).

>Примечание — Этот атрибут схож с компонентом `contentDescription` атрибута
`ContentHints`, но МОЖЕТ быть использован не только в многоуровневом документе.

### 9.2.8 <a name="Cades28"></a>Атрибут `SignerLocation`
<!--- 5.11.2; 5.2.5 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-signerLocation OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 17
    }
    
Синтаксис атрибута описывается типом `SignerLocation`:

    SignerLocation ::= SEQUENCE { 
        -- at least one of the following shall be present: 
        countryName [0] DirectoryString OPTIONAL,
        -- As used to name a Country in X.500 
        localityName [1] DirectoryString OPTIONAL,
        -- As used to name a locality in X.500 
        postalAddress [2] PostalAddress OPTIONAL
    }
    PostalAddress ::= SEQUENCE SIZE(1..6) OF DirectoryString

### 9.2.9 <a name="Cades29"></a>Атрибут `SignerAttributes`
<!--- 5.11.3; 5.2.6.1 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-signerAttrV2 OBJECT IDENTIFIER ::= { 
        itu-t(0) identified-organization(4)
        etsi(0) cades(19122) attributes(1) 1
    }

Синтаксис атрибута описывается типом `SignerAttributeV2`:

    SignerAttributeV2 ::= SEQUENCE {
        claimedAttributes [0] ClaimedAttributes OPTIONAL,
        certifiedAttributesV2 [1] CertifiedAttributesV2 OPTIONAL,
        signedAssertions [2] SignedAssertions OPTIONAL
    }
    ClaimedAttributes ::= SEQUENCE OF Attribute
    CertifiedAttributesV2 ::= SEQUENCE OF CHOICE {
        attributeCertificate [0] AttributeCertificate,
        otherAttributeCertificate [1] OtherAttributeCertificate
    }
    OtherAttributeCertificate ::= SEQUENCE {
        otherAttributeCertID OBJECT IDENTIFIER,
        otherAttributeCert ANY DEFINED BY otherAttributeCertID OPTIONAL
    }
    SignedAssertions ::= SEQUENCE OF SignedAssertion
    SignedAssertion ::= SEQUENCE {
        signedAssertionID OBJECT IDENTIFIER,
        signedAssertion ANY DEFINED BY signedAssertionID OPTIONAL
    }

Тип `AttributeCertificate` определен в СТБ 34.101.67.

Неудостоверенные атрибуты указываются в компоненте `claimedAttributes`.
Заявляемые SAML-утверждения могут указываться в компоненте `claimedAttributes`.
Следующий идентификатор атрибута определяет SAML-утверждения:

    id-aa-ets-claimedSAML OBJECT IDENTIFIER ::= { 
        itu-t(0) identified-organization(4)
        etsi(0) cades(19122) attributes(1) 2 
    }

Атрибут, содержащий SAML-утверждения, имеет следующий синтаксис:

    ClaimedSAMLAssertion ::= OCTET STRING

Удостоверенные атрибуты указываются в компоненте `certifiedAttributesV2`.
Удостоверенные атрибуты представляют собой атрибутные сертификаты. Атрибутный
сертификат согласно СТБ 34.101.67 указывается в компоненте
`attributeCertificate`. Атрибутный сертификат в ином синтаксисе указывается в
компоненте `otherAttributeCertificate`, при этом тип представления указывается в
компоненте `otherAttributeCertID`, а значение атрибутного сертификата — в
компоненте `otherAttributeCert`. Не рекомендуется использовать компонент
`otherAttributeCertificate`.

<!--- 
todo: Не рекомендуется использовать атрибутные сертификаты в ином синтаксисе.  
-->

Подписанные третьей стороной утверждения указываются в компоненте
`signedAssertions`. Тип представления утверждения указывается в компоненте
`signedAssertionID`, а значение утверждения — в компоненте `signedAssertion`.
Определения подписанных утверждений выходят за рамками настоящего стандарта.

### 9.2.10 <a name="Cades210"></a>Атрибут `CounterSignature`
<!--- 5.9.2; 5.2.7 -->

Синтаксис атрибута описывается типом `Countersignature`, который определен
(вместе с правилами включения) в СТБ 34.101.23 (пункт 15.5).
<!--- CMS (RFC 3852 [4]). -->

### 9.2.11 <a name="Cades211"></a>Атрибут `ContentTimeStamp`
<!--- 5.11.4; 5.2.8 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-contentTimestamp OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 20
    }

Синтаксис атрибута описывается типом `ContentTimestamp`, определенным в СТБ
34.101.82:

    ContentTimestamp::= TimeStampToken
    TimeStampToken ::= ContentInfo
     
Штамп времени вырабатывается от подписываемого содержимого. В случае
прикрепленной подписи хэшируется значение компонента `eContent`, без учета тега
и длины. В случае отделенной подписи хэшируются внешние данные.

>Примечание 1 — Атрибут `ContentTimeStamp` указывает, что подписанный документ
был сформирован до даты, заданной атрибутом `ContentTimeStamp`.

<!--- Замечание 5 -->
>Примечание 2 — Несколько значений атрибута, выработанных разными модулями
штампов времени, МОГУТ содержаться в РЭЦП. При этом в набор
`SignerInfo.signedAttrs` ДОЛЖЕН быть добавлен новый элемент, соответствующий
значению данного атрибута.

### 9.2.12 <a name="Cades212"></a>Атрибут `SignaturePolicyIdentifier`
<!--- 5.8.1; 5.2.9 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-sigPolicyId OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs9(9) smime(16) id-aa(2) 15
    }

Синтаксис атрибута описывается типом `SignaturePolicyIdentifier`:

    SignaturePolicyIdentifier ::= CHOICE {
        signaturePolicyId SignaturePolicyId,
        signaturePolicyImplied SignaturePolicyImplied -- not used in this version
    }
    SignaturePolicyId ::= SEQUENCE {
        sigPolicyId SigPolicyId,
        sigPolicyHash SigPolicyHash,
        sigPolicyQualifiers SEQUENCE SIZE (1..MAX) 
                    OF SigPolicyQualifierInfo OPTIONAL
    }
    SignaturePolicyImplied ::= NULL

Ссылка на политику подписи указывается явно, поэтому вариант
`signaturePolicyImplied` НЕ ДОЛЖЕН использоваться.

Компонент `sigPolicyId` содержит идентификатор объекта, который однозначно
определяет версию политики подписи, и имеет тип:

    SigPolicyId ::= OBJECT IDENTIFIER
    
Компонент `sigPolicyHash` может содержать идентификатор алгоритма хэширования и
хэш-значение политики подписи.
<!--- В оригинале допускается использовать пустое хэш-значение,
в случае, если оно не известно. Так было сделано для обратной
совместимости. Но сейчас нам нет смысла вводить такую опциональность.
-->

Если политика подписи определена с помощью синтаксиса АСН.1, то хэш-значение
вычисляется без значений внешнего типа и длины, при этом должен использоваться
алгоритм хэширования, идентификатор которого указан в компоненте
`sigPolicyHash`. Тип компонента `sigPolicyHash` определен следующим образом:

    SigPolicyHash ::= OtherHashAlgAndValue
    OtherHashAlgAndValue ::= SEQUENCE {
        hashAlgorithm   AlgorithmIdentifier,
        hashValue   OtherHashValue 
    }
    OtherHashValue ::= OCTET STRING

Идентификатор политики подписи может быть уточнен с помощью квалификаторов,
указываемых в компоненте `sigPolicyQualifiers`. Квалификатор имеет следующий
тип:

    SigPolicyQualifierInfo ::= SEQUENCE {
        sigPolicyQualifierId  SigPolicyQualifierId,
        sigQualifier ANY DEFINED BY sigPolicyQualifierId 
    }

В настоящем стандарте определены следующие квалификаторы:

- URI или URL документа, содержащего политику подписи. Определяется
идентификатором `id-spq-ets-uri`;
- примечание, отображаемое пользователю при каждой проверке подписи.
Определяется идентификатором `id-spq-ets-unotice`;
- идентификатор технической спецификации, определяющей синтаксис, используемый
для создания документа, содержащего политику подписи. Определяется
идентификатором `id-spq-ets-docspec`.

Квалификаторы имеют следующий синтаксис:

    SigPolicyQualifierId ::= OBJECT IDENTIFIER

    id-spq-ets-uri OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs9(9) smime(16) id-spq(5) 1
    }
    SPuri ::= IA5String

    id-spq-ets-unotice OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs9(9) smime(16) id-spq(5) 2
    }
    SPUserNotice ::= SEQUENCE {
        noticeRef NoticeReference OPTIONAL, 
        explicitText DisplayText OPTIONAL
    }
    NoticeReference ::= SEQUENCE {
        organization DisplayText, 
        noticeNumbers SEQUENCE OF INTEGER
    }
    DisplayText ::= CHOICE {
        visibleString VisibleString  (SIZE (1..200)),
        bmpString BMPString          (SIZE (1..200)),
        utf8String UTF8String        (SIZE (1..200))
    }

    id-spq-ets-docspec OBJECT IDENTIFIER ::= { 
        itu-t(0) identified-organization(4)
        etsi(0) cades(19122) id-spq (2) 1
    }
    SPDocSpecification ::= CHOICE {
        oid OBJECT IDENTIFIER,
        uri IA5String 
    }


### 9.2.13 <a name="Cades213"></a>Атрибут `SignaturePolicyStore`
<!--- ; 5.2.10 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-sigPolicyStore OBJECT IDENTIFIER ::= {
        itu-t(0) identified-organization(4)
        etsi(0) cades(19122) attributes(1) 3
    }

Синтаксис атрибута описывается типом `SignaturePolicyStore`:

    SignaturePolicyStore ::= SEQUENCE {
        spDocSpec SPDocSpecification,
        spDocument SignaturePolicyDocument
    }
    SPDocSpecification ::= CHOICE {
        oid OBJECT IDENTIFIER,
        uri IA5String
    }
    SignaturePolicyDocument ::= CHOICE {
        sigPolicyEncoded OCTET STRING,
        sigPolicyLocalURI IA5String
    }

Компонент `spDocument` содержит либо закодированный документ с политикой подписи
в компоненте `sigPolicyEncoded`, либо URI, указывающий на документ в локальном
хранилище, в компоненте `sigPolicyLocalURI`.

>Примечание — В отличие от `SPuri`, `sigPolicyLocalURI` указывает на локальный 
файл.

Компонент `spDocSpec` указывает на техническую спецификацию, определяющую
синтаксис политики подписи.

>Примечание 1 — Ответственность за доступность корректного документа ложится на
участника, добавляющего политику подписи в хранилище.

>Примечание 2 — Атрибут `SignaturePolicyStore` является неподписанным. Это
обусловлено тем, что любые изменения документа с политикой подписи или ссылки на
него приведут к признанию подписи недействительной.

<!--- Требование k -->
>Примечание 3 — Данный атрибут включается только в случае, когда атрибут
`SignaturePolicyIdentifier` также включен и содержит в компоненте
`sigPolicyHash` хэш-значение документа политики подписи. Иначе данный атрибут
включаться НЕ ДОЛЖЕН.

### 9.2.14 <a name="Cades214"></a>Атрибут `ContentReference`
<!--- 5.10.1; 5.2.11 -->

Атрибуту назначается следующий идентификатор:

    id-aa-contentReference   OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 10 
    }

Синтаксис атрибута описывается типом `ContentReference`, определенным в
[[ESS]](99Biblio.md#ESS):

    ContentReference ::= SEQUENCE {
        contentType ContentType,
        signedContentIdentifier ContentIdentifier,
        originatorSignatureValue OCTET STRING 
    }

В РЭЦП, на которую ссылается данный атрибут, должен быть указан атрибут
`ContentIdentifier`, а соответствующее значение указывается в компоненте
`signedContentIdentifier` данного атрибута. Компонент `contentType` содержит 
тип содержимого, а `originatorSignatureValue` — значение подписи РЭЦП, на 
которую ссылается атрибут.

### 9.2.15 <a name="Cades215"></a>Атрибут `ContentIdentifier`
<!--- 5.10.2; 5.2.12 -->

Атрибуту назначается следующий идентификатор:

    id-aa-contentIdentifier OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 7
    }

Синтаксис атрибута описывается типом `ContentIdentifier`, 
определенным в [[ESS]](ESS):

    ContentIdentifier ::= OCTET STRING

Атрибут содержит уникальный идентификатор, связанный с документом.

Для обеспечения уникальности, атрибут `ContentIdentifier` СЛЕДУЕТ формировать
как конкатенацию идентификационной информации пользователя (такой как имя
пользователя или открытый ключ), строки типа `GeneralizedTime` и случайного
числа.

## 9.3 <a name="Cades3"></a>Синтаксис атрибута штампа времени

### 9.3.1 <a name="Cades31"></a>Атрибут `SignatureTimeStamp`
<!--- 6.1.1; 5.3 -->

Атрибуту назначается следующий идентификатор:

    id-aa-signatureTimeStampToken OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 14
    }

Синтаксис атрибута описывается типом `SignatureTimeStampToken`:

    SignatureTimeStampToken ::= TimeStampToken

Тип `TimeStampToken` определен в СТБ 34.101.82.

Штамп времени вырабатывается от значения подписи, указываемого в компоненте
`SignerInfo.signature`, без учета тега и длины. РЭЦП МОЖЕТ содержать несколько
экземпляров этого атрибута от разных служб штампов времени.

>Примечание — В случае нескольких подписей штампы времени 
МОГУТ проставляться на значения подписей только некоторых или всех
подписантов, содержащихся в `SignedData`.

<!--- Требование l -->
При кодировании значений атрибута ДОЛЖНЫ использоваться отличительные правила.

<!--- Требование m -->
Штампы времени должны выставляться до истечения срока действия или отзыва
сертификата подписанта.

## 9.4 <a name="Cades4"></a>Синтаксис атрибутов проверочных данных

### 9.4.1 <a name="Cades41"></a>Атрибут `CompleteCertificateReferences`
<!--- 6.2.1; A.1.1.1 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-certificateRefs OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 21
    }

Синтаксис атрибута описывается типом `CompleteCertificateRefs`:

    CompleteCertificateRefs ::=  SEQUENCE OF OtherCertID
    OtherCertID ::= SEQUENCE {
        otherCertHash OtherHash,
        issuerSerial IssuerSerial OPTIONAL 
    }
    OtherHash ::= CHOICE {
        sha1Hash OtherHashValue, -- This contains a SHA-1 hash
        otherHash OtherHashAlgAndValue 
    }

Ссылка представляет собой хэш-значение, вычисленное от кодового представления
сертификата, полученного с помощью отличительных правил. Хэш-значение
указывается в компоненте `otherCertHash`.
<!--- Требование g -->
Компонент `issuerSerial` НЕ ДОЛЖЕН быть включен в значения типа `OtherCertID`.

Хэш-значения, вычисленные по алгоритму SHA-1, указываются в компоненте
`sha1Hash`, другие хэш-значения указываются в компоненте `otherCertHash`.

>Примечание — Копии значений сертификатов могут храниться 
в атрибуте `CertificateValues` либо в компоненте `SignedData.certificates`.

Атрибут МОЖЕТ содержать ссылки на цепочки сертификатов модулей штампов времени,
предоставляющих штампы времени. В этом случае ссылки на сертификаты указываются
как атрибуты соответствующих штампов времени.

### 9.4.2 <a name="Cades42"></a>Атрибут `CertificateValues`
<!--- 6.3.3; A.1.1.2 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-certValues OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 23
    }

Синтаксис атрибута описывается типом `CertificateValues`:

    CertificateValues ::= SEQUENCE OF Certificate

Тип `Certificate` определен в СТБ 34.101.19. 

Атрибут ДОЛЖЕН содержать только сертификаты, ссылки на которые указаны в
атрибутах `CompleteCertificateReferences`, `AttributeCertificateReferences` и
`SigningCertificate`, и которые не содержатся в `SignedData.certificates`.
Атрибут `AttributeCertificateValues` в формате CAdES не используется.

Атрибут МОЖЕТ содержать информацию о сертификатах модулей штампов времени,
предоставляющих штампы времени, если эти сертификаты еще не включены в них как
часть подписей служб штампов времени. В этом случае неподписанный атрибут
добавляется к структуре `SignedData` соответствующего штампа времени.
<!--- Требований нет -->

### 9.4.3 <a name="Cades43"></a>Атрибут `CompleteRevocationReferences`
<!--- 6.2.2; A.1.2.1 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-revocationRefs OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 22
    }

Синтаксис атрибута описывается типом `CompleteRevocationRefs`:

    CompleteRevocationRefs ::=  SEQUENCE OF CrlOcspRef
    CrlOcspRef ::= SEQUENCE {
        crlids   [0] CRLListID   OPTIONAL,
        ocspids  [1] OcspListID  OPTIONAL,
        otherRev [2] OtherRevRefs OPTIONAL 
    }

Атрибут содержит элементы типа `CrlOcspRef`, соответствующие списку 1 из
описания атрибута `CompleteRevocationReferences`, и ДОЛЖЕН начинаться с
аттестатов отзыва сертификата подписанта. Аттестаты отзыва ДОЛЖНЫ включаться в
порядке перечисления соответствующих сертификатов в атрибуте
`CompleteCertificateReferences`.
Затем МОГУТ следовать аттестаты отзыва из списка 2.

    CRLListID ::= SEQUENCE {
        crls SEQUENCE OF CrlValidatedID 
    }
    CrlValidatedID ::= SEQUENCE {
        crlHash OtherHash,
        crlIdentifier CrlIdentifier OPTIONAL 
    }
    CrlIdentifier ::= SEQUENCE {
        crlissuer Name,
        crlIssuedTime UTCTime,
        crlNumber INTEGER OPTIONAL 
    }

При создании значения типа `CrlValidatedID` значение `crlHash` ДОЛЖНО быть
вычислено по кодовому представлению СОС, полученному с использованием
отличительных правил. Компонент `crlIdentifier` СЛЕДУЕТ указывать, если СОС
нельзя определить другим способом.

Компонент `crlIdentifier` ДОЛЖЕН идентифицировать СОС с использованием имени
издателя, времени выпуска списка, которое ДОЛЖНО соответствовать времени в
компоненте `thisUpdate` искомого СОС, и компонента `crlNumber`, если он задан.
Если идентифицированный СОС является ПСОС, то ДОЛЖНЫ быть включены ссылки на
весь набор ПСОС для формирования полного СОС, включаемого в проверочные данные.

    OcspListID ::= SEQUENCE {
        ocspResponses SEQUENCE OF OcspResponsesID 
    }
    OcspResponsesID ::= SEQUENCE {
        ocspIdentifier OcspIdentifier,
        ocspRefHash OtherHash OPTIONAL  -- must be included
    }
    OcspIdentifier ::= SEQUENCE {
        ocspResponderID ResponderID, -- As in OCSP response data
        producedAt GeneralizedTime  -- As in OCSP response data
    }
    OtherRevRefs ::= SEQUENCE {
        otherRevRefType OtherRevRefType, 
        otherRevRefs ANY DEFINED BY otherRevRefType 
    }
    OtherRevRefType ::= OBJECT IDENTIFIER

Компонент `ocspIdentifier` ДОЛЖЕН идентифицировать ответ OCSP с помощью имени
издателя и времени выдачи ответа OCSP, которое должно соответствовать времени в
выданном ответе OCSP.
Компонент `ocspRefHash` является необязательным для совместимости; РЕКОМЕНДУЕТСЯ
всегда включать этот компонент.
Реализации, проверяющие подпись, могут принимать подписи без этого элемента. Но
следует учитывать, что его отсутствие позволяет проводить атаки с подменой
ответов OCSP, например, при компрометации ключей службы OCSP. Реализации,
принимающие подписи без этого элемента, МОГУТ использовать сторонние механизмы
для гарантии того, что ни один из ключей службы OCSP не был скомпрометирован на
время проверки.

>Примечание 1 — Копии СОС и ответов OCSP МОГУТ храниться в атрибуте
`RevocationValues`.

>Примечание 2 — Рекомендуется использовать данный атрибут вместо атрибута
`OtherRevocationInfoFormat`, определенного в СТБ 34.101.23, для обеспечения
обратной совместимости.

Синтаксис и семантика других ссылок на СОС выходят за рамки настоящего
стандарта. Для определения других форм информации об отозванных сертификатах
используется тип `OtherRevRefType`.

Атрибут МОЖЕТ содержать ссылки на полный набор СОС и ответов OCSP, которые
использовались для проверки цепочки сертификатов модулей штампов времени. В этом
случае неподписанный атрибут добавляется к структуре `SignedData`
соответствующего штампа времени.
<!--- Требований нет -->

### 9.4.4 <a name="Cades44"></a>Атрибут `RevocationValues`
<!--- 6.3.4; A.1.2.1 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-revocationValues OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 24
    }

Синтаксис атрибута описывается типом `RevocationValues`:

    RevocationValues ::=  SEQUENCE {
        crlVals      [0] SEQUENCE OF CertificateList OPTIONAL,
        ocspVals     [1] SEQUENCE OF BasicOCSPResponse OPTIONAL,
        otherRevVals [2] OtherRevVals OPTIONAL
    }
    OtherRevVals ::= SEQUENCE {
        otherRevValType OtherRevValType, 
        otherRevVals ANY DEFINED BY OtherRevValType 
    }
    OtherRevValType ::= OBJECT IDENTIFIER

Атрибут ДОЛЖЕН содержать аттестаты отзыва, ссылки на которые перечислены в
атрибутах `CompleteRevocationReferences` и `AttributeRevocationReferences`, и
которые не содержатся в компоненте `SignedData.crls`.
Другие элементы НЕ ДОЛЖНЫ быть включены в этот атрибут.
Атрибут `AttributeRevocationValues` в формате CAdES не используется.

Синтаксис и семантика других аттестатов отзыва сертификатов, указываемых в типе
`OtherRevVals`, выходят за рамки настоящего стандарта.

Для определения других форм информации об отозванных сертификатах используется
тип `OtherRevRefType`.

Тип `CertificateList` определен в СТБ 34.101.19.
Тип `BasicOCSPResponse` определен в СТБ 34.101.26.

Атрибут МОЖЕТ содержать аттестаты отзыва сертификатов модулей штампов времени,
включая СОС и ответы OCSP, если эти сертификаты еще не включены как часть
подписей модулей штампов времени. В этом случае неподписанный атрибут
добавляется к структуре `SignedData` соответствующего штампа времени.

>Примечание — РЕКОМЕНДУЕТСЯ использовать данный атрибут вместо типа
`OtherRevocationInfoFormat`, определенного в СТБ 34.101.23, для обеспечения
обратной совместимости.

### 9.4.5 <a name="Cades45"></a>Атрибут `AttributeCertificateReferences`
<!--- 6.2.3; A.1.3 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-attrCertificateRefs OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 44
    }

Синтаксис атрибута описывается типом `AttributeCertificateRefs`:

    AttributeCertificateRefs ::=  SEQUENCE OF OtherCertID

>Примечание — Копии значений сертификатов МОГУТ содержаться в атрибуте
`CertificateValues` или в компоненте `SignedData.certificates`. Атрибутный
сертификат ДОЛЖЕН содержаться в атрибуте `SignerAttributes`.

<!--- Требование n -->
Данный атрибут НЕ ДОЛЖЕН использоваться, если атрибут `SignerAttributes` не
содержит удостоверенные атрибуты или подписанные утверждения подписанта.

### 9.4.6 <a name="Cades46"></a>Атрибут `AttributeCertificateValues`
<!--- ; XAdES 5.4.3 -->

Атрибут `AttributeCertificateValues` в формате CAdES не используется,
а соответствующие сертификаты указываются в атрибуте 
`CertificateValues`.

### 9.4.7 <a name="Cades47"></a>Атрибут `AttributeRevocationReferences`
<!--- 6.2.4; A.1.4 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-attrRevocationRefs OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 45
    }

Синтаксис атрибута описывается типом `AttributeRevocationRefs`:
    
    AttributeRevocationRefs ::=  SEQUENCE OF CrlOcspRef

>Примечание — Копии значений аттестатов отзыва МОГУТ содержаться
в атрибуте `RevocationValues` или в компоненте `SignedData.crls`.

Если один или несколько СОС не являются полными, а содержат приращение к
предыдущему, то атрибут ДОЛЖЕН содержать полный набор ПСОС, необходимый для
построения полного СОС.

<!--- Требование n -->
Данный атрибут НЕ ДОЛЖЕН использоваться, если атрибут `SignerAttributes` не
содержит удостоверенные атрибуты или подписанные утверждения подписанта.

### 9.4.8 <a name="Cades48"></a>Атрибут `AttributeRevocationValues`
<!--- ; XAdES 5.4.3 -->

Атрибут `AttributeRevocationValues` в формате CAdES не используется, а
соответствующие аттестаты отзыва указываются в атрибуте `RevocationValues`.

### 9.4.9 <a name="Cades49"></a>Атрибут `RefsOnlyTimeStamp`
<!--- 6.3.6; A.1.5.1 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-certCRLTimestamp OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 26
    }

Синтаксис атрибута описывается типом `TimestampedCertsCRLs`:

    TimestampedCertsCRLs ::= TimeStampToken

Штамп времени вырабатывается от конкатенации в указанном порядке значений
атрибутов `CompleteCertificateReferences` и `CompleteRevocationReferences`,
включая компоненты `attrType` и `attrValues` (с типом и длиной), но не включая
тип и длину внешнего контейнера `SEQUENCE`.

Атрибуты, к которым применяется штамп времени, СЛЕДУЕТ кодировать с
использованием отличительных правил. Если отличительные правила не используются,
то двоичное кодирование объектов АСН.1, к которым применяется штамп времени,
ДОЛЖНО быть сохранено с целью обеспечения согласованного повторного вычисления
хэш-значения.

<!--- Замечание 5 -->
>Примечание — Несколько значений данного атрибута, выработанных разными модулями
штампов времени, МОГУТ содержаться в РЭЦП. При этом в набор
`SignerInfo.unsignedAttrs` ДОЛЖЕН быть добавлен новый элемент, соответствующий
значению данного атрибута.

### 9.4.10 <a name="Cades410"></a>Атрибут `SigAndRefsTimeStamp`
<!--- 6.3.5; A.1.5.2 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-escTimeStamp OBJECT IDENTIFIER ::= { 
        iso(1) member-body(2) us(840) rsadsi(113549) 
        pkcs(1) pkcs-9(9) smime(16) id-aa(2) 25
    }

Синтаксис атрибута описывается типом `ESCTimeStampToken`:

    ESCTimeStampToken ::= TimeStampToken

Штамп времени вырабатывается от конкатенации в указанном порядке представления
компонента `SignerInfo.signature`, включая поля тега и длины, и значений
атрибутов `SignatureTimeStamp`, `CompleteCertificateReferences` и
`CompleteRevocationReferences`, включая компоненты `attrType` и `attrValues`,
включая поля тега и длины, но не включая поля тега и длины внешнего типа
`SEQUENCE`.

Атрибуты, к которым применяется штамп времени, СЛЕДУЕТ кодировать с
использованием отличительных правил. Если отличительные правила не используются,
то двоичное кодирование объектов АСН.1, к которым применяется штамп времени,
ДОЛЖНО быть сохранено с целью обеспечения согласованного повторного вычисления
хэш-значения.

<!--- Замечание 5 -->
>Примечание — Несколько значений данного атрибута, выработанных разными модулями
штампов времени, МОГУТ содержаться в РЭЦП. При этом в набор
`SignerInfo.unsignedAttrs` ДОЛЖЕН быть добавлен новый элемент, соответствующий
значению данного атрибута.

## 9.5 <a name="Cades5"></a>Синтаксис атрибутов архивных проверочных данных


### 9.5.1 <a name="Cades51"></a>Атрибут `TimeStampValidationData`
<!--- ; 5.5.2 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ATSHashIndex-v3 OBJECT IDENTIFIER ::= {
        itu-t(0) identified-organization(4)
        etsi(0) cades(19122) attributes(1) 5
    }

Синтаксис атрибута описывается типом `ATSHashIndexV3`:

    ATSHashIndexV3 ::= SEQUENCE {
        hashIndAlgorithm AlgorithmIdentifier,
        certificatesHashIndex SEQUENCE OF OCTET STRING,
        crlsHashIndex SEQUENCE OF OCTET STRING,
        unsignedAttrValuesHashIndex SEQUENCE OF OCTET STRING 
    }

Значение атрибута должно быть закодировано с использованием отличительных
правил.

Компонент `hashIndAlgorithm` содержит идентификатор алгоритма хэширования,
используемого для вычисления хэш-значений, содержащихся в компонентах
`certificatesHashIndex`, `crlsHashIndex`, `unsignedAttrValuesHashIndex`. Этот же 
алгоритм хэширования должен использоваться при вычислении штампа времени
атрибута `ArchiveTimeStamp`.

Компонент `certificatesHashIndex` содержит список хэш-значений, вычисленных от
всех элементов типа `CertificateChoices`, содержащихся в компоненте
`certificates` объекта типа `SignedData` на момент выработки штампа времени
`ArchiveTimeStamp`. Другие хэш-значения не могут содержаться в списке
`certificatesHashIndex`.

Компонент `crlsHashIndex` содержит список хэш-значений, вычисленных от всех
элементов типа `RevocationInfoChoice`, содержащихся в компоненте `crls` объекта
типа `SignedData` на момент выработки штампа времени `ArchiveTimeStamp`. Другие
хэш-значения не могут содержаться в списке `crlsHashIndex`.

Компонент `unsignedAttrValuesHashIndex` содержит список хэш-значений, 
вычисленных от элементов типа `Attribute`, содержащихся в компонентах 
`unsignedAttrs` всех объектов типа `SignerInfo` на момент выработки штампа 
времени `ArchiveTimeStamp`. Для каждого элемента `Attribute` и для каждого 
значения в его компоненте `Attribute.attrValues` это значение предваряется 
значением `Attribute.attrType` и хэшируется. Другие хэш-значения не могут 
содержаться в списке `unsignedAttrValuesHashIndex`. При обработке списка 
проверяется, что он содержит хэш-значения всех пар (`Attribute.attrType`, 
значение в `Attribute.attrValues`) и только эти хэш-значения. 

Все перечисленные хэш-значения вычисляются от закодированных элементов, включая
поля тега и длины. Элементы типа `OtherCertificateFormat` должны быть
закодированы с использованием отличительных правил, за исключением случая
использования варианта `otherCert`, при котором способ кодирования сохраняется.

>Примечание — Использование атрибута `TimeStampValidationData` позволяет
добавлять сертификаты, информацию об отзыве, неподписанные атрибуты после
выработки штампа времени `ArchiveTimeStamp`. Поэтому при проверке атрибута
`TimeStampValidationData` требуется для каждого хэш-значения искать
соответствующий элемент, а не наоборот.

### 9.5.2 <a name="Cades52"></a>Атрибут `ArchiveTimeStamp`
<!--- ; 5.5.3 -->

Атрибуту назначается следующий идентификатор:

    id-aa-ets-archiveTimestampV3 OBJECT IDENTIFIER ::= {
        itu-t(0) identified-organization(4)
        etsi(0) electronic-signature-standard(1733) attributes(2) 5
    }

Синтаксис атрибута описывается типом `ArchiveTimeStampToken`:

    ArchiveTimeStampToken ::= TimeStampToken

Входным значением при вычислении хэш-значения `messageImprint` штампа времени
является конкатенация хэш-значения от подписанных данных и определенных
элементов в закодированном виде, включая тег и длину, в порядке, перечисленном
ниже:

1. Компонент `SignedData.encapContentInfo.eContentType`.
2. Хэш-значение от исходного подписываемого документа. Хэш-значение вычисляется
для тех же исходных данных, что и хэш-значение, содержащееся в подписанном
атрибуте `MessageDigest`. Алгоритм хэширования ДОЛЖЕН совпадать с алгоритмом,
используемым при выработке штампа времени `ArchiveTimeStamp`. Идентификатор
алгоритма хэширования ДОЛЖЕН быть включен в компонент
`SignedData.digestAlgorithms`.
3. Компоненты `version`, `sid`, `digestAlgorithm`, `signedAttrs`,
`signatureAlgorithm` и `signature` внутри элемента `SignedData.signerInfos`, для
которого вырабатывается штамп времени, в указанном порядке.
4. Единственный элемент типа `ATSHashIndexV3`, содержащийся в атрибуте
`TimeStampValidationData`.

Штамп времени `ArchiveTimeStamp` ДОЛЖЕН содержать один неподписанный атрибут
`TimeStampValidationData`, использованный на шаге 4.

>Примечание 1 — Включение атрибута `TimeStampValidationData` на шаге 4
гарантирует, что все существенные компоненты РЭЦП защищены штампом времени.

>Примечание 2 — При проверке подписи атрибут `TimeStampValidationData` выступает
в качестве доказательства существования на момент выработки штампа времени всех
элементов, использованных при вычислении хэш-значения `messageImprint`. Это
гарантирует, что проверка подписи основана на аттестатах, которые действительно
существовали в прошлом.

>Примечание 3 — Удостоверяющие подписи, содержащиеся в неподписанном атрибуте
`Countersignature`, МОГУТ не содержать независимый атрибут `ArchiveTimeStamp`,
так как они защищены штампом времени `ArchiveTimeStamp` как неподписанные
атрибуты.

Перед формированием атрибута `ArchiveTimeStamp` структура `SignedData` ДОЛЖНА
быть дополнена всеми аттестатами, еще не включенными в структуру. Аттестаты
МОГУТ включать сертификаты, списки отзыва сертификатов, OCSP ответы, требуемые
для проверки всех подписанных объектов внутри РЭЦП, включая подписи, в том числе
удостоверяющие, штампы времени, OCSP ответы, сертификаты, атрибутные
сертификаты, подписанные утверждения и списки отзыва.

>Примечание 4 — Аттестаты, уже включенные в объект типа `SignedData`, например,
как часть штампа времени, не требуется включать повторно.

При создании атрибута `ArchiveTimeStamp` ДОЛЖНЫ использоваться отличительные
правила кодирования с сохранением кодирования любого подписанного элемента,
содержащегося в атрибуте. Кодовое представление также ДОЛЖНО быть сохранено для
всех существующих неподписанных атрибутов и компонентов, используемых при
вычислении хэш-значения `messageImprint` данного атрибута.

