<!--- todo:

Qualify
=======
qualify как "уточняют" считаю неудачно. Лучше "описывают".

Id
==
Перенес описание идентификаторов в преамбулу

-->

# 10 Формат XAdES

## 10.1 Общие положения

Язык XML, определенный в базовой спецификации [XML] и других подчиненных ей, 
описывает иерархически организованные структуры данных, называемые 
XML-документами. Документ содержит элементы, возможно вложенные друг в 
друга, элементы могут иметь атрибуты. Структура элементов и атрибутов 
задается XML-схемами.
<!-- в пдф атрибут с большой буквы-->

Правила подписания XML-документов определены в [XML-DSIG] и представлены в
СТБ 34.101.50 (приложение Е). Правила называются XML-DSig (от англ. XML
Digital Signature). Формат XAdES уточняет данные правила.

Атрибуты РЭЦП формата XAdES задаются XML-элементами, схемы которых 
определяются либо непосредственно в XML-DSig, либо в подразделах 10.6 - 
10.9. Атрибуты объединяются в XML-контейнеры, описанные в подразделе 10.2. 
Контейнер содержит XML-элементы, сам являясь XML-элементом. В подразделе 
10.3 определяются правила включения контейнеров атрибутов в РЭЦП.

Следует отличать атрибут подписи от атрибута XML-элемента.
В настоящем разделе под "атрибутом" понимается атрибут подписи, 
представленный XML-элементом. Термин "XML-атрибут" относится к атрибуту 
XML-элемента. 

Многие из определяемых далее XML-элементов содержат XML-атрибут `Id`.
Этот XML-атрибут идентифицирует элемент в пределах XML-документа
и может использоваться для ссылки на элемент из других частей документа.

Для однозначной глобальной идентификации элементов в XML используются
пространства имен. Пространство имен покрывает группу элементов с
отличающимися именами. Пространству назначается уникальный
URI-идентификатор, который выступает в роли имени пространства.
URI-идентификатор добавляется к именам элементов, определенных в
пространстве, делая имена уникальными уже за пределами пространства. В
определении пространства может задаваться короткий префикс, предназначенный
для замены длинного идентификатора.

В XAdES используются следующие пространства имен: 

- http://uri.etsi.org/01903/v1.3.2#;
- http://uri.etsi.org/01903/v1.4.1#;
- http://www.w3.org/2000/09/xmldsig# (префикс `ds`);
- http://www.w3.org/2001/XMLSchema (префикс `xsd`).

В частности, в пространстве http://www.w3.org/2000/09/xmldsig# 
определены элементы `ds:Object`, `ds:Signature` и `ds:Reference`, 
задействованные в XAdES. 

Пространство имен, определенное URI-идентификатором 
http://uri.etsi.org/01903/v1.3.2#, СЛЕДУЕТ вводить в XML-документ следующим 
образом:
 
    <xsd:schema targetNamespace="http://uri.etsi.org/01903/v1.3.2#"
    xmlns:ds="http://www.w3.org/2000/09/xmldsig#" xmlns="http://uri.etsi.org/01903/v1.3.2#"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">

Пространство имен, определенное URI-идентификатором 
http://uri.etsi.org/01903/v1.4.1#, СЛЕДУЕТ вводить в XML-документ следующим 
образом: 

    <xsd:schema targetNamespace="http://uri.etsi.org/01903/v1.4.1#"
    xmlns:ds="http://www.w3.org/2000/09/xmldsig#" xmlns="http://uri.etsi.org/01903/v1.4.1#"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xades="http://uri.etsi.org/01903/v1.3.2#"
    elementFormDefault="qualified">

Непосредственно перед выработкой ЭЦП атрибуты канонизируются.
Канонизация направлена на исключение избыточности, присущей 
XML-документам: два логически эквивалентных документа будут иметь один и 
тот же канонический вид. Некоторые атрибуты РЭЦП обеспечивают опциональные 
возможности для указания алгоритма канонизации, используемого при 
вычислении канонической формы. При создании новой РЭЦП, все такие атрибуты 
ДОЛЖНЫ содержать идентификатор алгоритма канонизации.

При дополнении РЭЦП новым атрибутом, XML-схема которого предусматривает 
идентификатор алгоритма канонизации, атрибут ДОЛЖЕН содержать этот
идентификатор.

## 10.2 Контейнеры атрибутов

### 10.2.1 Контейнер `QualifyingProperties`

Контейнер `QualifyingProperties` содержит атрибуты РЭЦП.

Синтаксис `QualifyingProperties` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="QualifyingProperties" type="QualifyingPropertiesType"/>
    <xsd:complexType name="QualifyingPropertiesType">
    <xsd:sequence>
    <xsd:element ref="SignedProperties" minOccurs="0"/>
    <xsd:element ref="UnsignedProperties" minOccurs="0"/>
    </xsd:sequence>
    <xsd:attribute name="Target" type="xsd:anyURI" use="required"/>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType>  

Вложенные контейнеры `SignedProperties`, `UnsignedProperties` описываются 
в п. 10.2.2, 10.2.3.

XML-атрибут `Target` ссылается на XML-атрибут `Id` 
соответствующего элемента `ds:Signature`. 

XML-атрибут `Target` содержит URI с простым именем фрагмента XPointer. 
Если РЭЦП оборачивает элемент `QualifyingProperties`, то часть значения 
XML-атрибута `Target`, которая не является фрагментом XPointer, ДОЛЖНА 
быть пустой. В противном случае - НЕ ДОЛЖНА.

>Примечание - Простое имя (от англ. bare-name) - упрощенная форма записи 
фрагмента XPointer. Например, запись #Signature является простым именем 
для #xpointer(id("Signature")). 

РЭЦП НЕ ДОЛЖНА содержать пустой контейнер `QualifyingProperties`.

### 10.2.2 Контейнер `SignedProperties`

Контейнер `SignedProperties`, вложенный в `QualifyingProperties`, 
содержит подписанные атрибуты РЭЦП. 

Синтаксис `SignedProperties` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="SignedProperties" type="SignedPropertiesType" />
    <xsd:complexType name="SignedPropertiesType">
    <xsd:sequence>
    <xsd:element ref="SignedSignatureProperties" minOccurs="0"/>
    <xsd:element ref="SignedDataObjectProperties" minOccurs="0"/>
    </xsd:sequence>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType> 

Вложенные контейнеры `SignedSignatureProperties`, 
`SignedDataObjectProperties` описываются  в п.п. 10.2.4, 10.2.5.

РЭЦП НЕ ДОЛЖНА содержать пустой контейнер `SignedProperties`.

### 10.2.3 Контейнер `UnsignedProperties`

Контейнер `UnsignedProperties`, вложенный в `QualifyingProperties`,
содержит неподписанные атрибуты РЭЦП.

Синтаксис `UnsignedProperties` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="UnsignedProperties" type="UnsignedPropertiesType" />
    <xsd:complexType name="UnsignedPropertiesType">
    <xsd:sequence>
    <xsd:element ref="UnsignedSignatureProperties" minOccurs="0"/>
    <xsd:element ref="UnsignedDataObjectProperties" minOccurs="0"/>
    </xsd:sequence>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType> 

Вложенные контейнеры `UnsignedSignatureProperties`, 
`UnsignedDataObjectProperties` описываются  в п.п. 10.2.6, 10.2.7.

РЭЦП НЕ ДОЛЖНА содержать пустой контейнер `UnsignedProperties`. 

### 10.2.4 Контейнер `SignedSignatureProperties`

Контейнер `SignedSignatureProperties`, вложенный в `SignedProperties`,
содержит атрибуты, которые описывают подпись или подписанта. 

Синтаксис `SignedSignatureProperties` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#"-->
    <xsd:element name="SignedSignatureProperties"
    type="SignedSignaturePropertiesType" />
    <xsd:element name="SignedSignatureProperties" type="SignedSignaturePropertiesType"/>
    <xsd:complexType name="SignedSignaturePropertiesType">
    <xsd:sequence>
    <xsd:element ref="SigningTime" minOccurs="0"/>
    <xsd:element ref="SigningCertificate" minOccurs="0"/>
    <xsd:element ref="SigningCertificateV2" minOccurs="0"/>
    <xsd:element ref="SignaturePolicyIdentifier" minOccurs="0"/>
    <xsd:element ref="SignatureProductionPlace" minOccurs="0"/>
    <xsd:element ref="SignatureProductionPlaceV2" minOccurs="0"/>
    <xsd:element ref="SignerRole" minOccurs="0"/>
    <xsd:element ref="SignerRoleV2" minOccurs="0"/>
    <xsd:any namespace="##other" minOccurs="0" maxOccurs="unbounded"/>
    </xsd:sequence>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType 

Вложенные элементы-атрибуты описываются в 10.5. Элементы `SignerRole` и
`SigningCertificate` являются устаревшими и НЕ ДОЛЖНЫ включаться в
контейнер.

<!-- todo: проверить fwd-ссылки -->

РЭЦП НЕ ДОЛЖНА содержать пустой контейнер `SignedSignatureProperties`.

### 10.2.5 Контейнер `SignedDataObjectProperties`

Контейнер `SignedDataObjectProperties`, вложенный в `SignedProperties`,
содержит атрибуты, которые описывают объекты подписанного документа. 

Синтаксис `SignedDataObjectProperties` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:complexType name="SignedDataObjectPropertiesType">
    <xsd:sequence>
    <xsd:element ref="DataObjectFormat" minOccurs="0" maxOccurs="unbounded"/>
    <xsd:element ref="CommitmentTypeIndication" minOccurs="0" maxOccurs="unbounded"/>
    <xsd:element ref="AllDataObjectsTimeStamp" minOccurs="0" maxOccurs="unbounded"/>
    <xsd:element ref="IndividualDataObjectsTimeStamp" minOccurs="0" maxOccurs="unbounded"/>
    <xsd:any namespace="##other" minOccurs="0" maxOccurs="unbounded"/>
    </xsd:sequence>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType>  

Вложенные элементы-атрибуты описываются в 10.5.

<!-- todo: проверить fwd-ссылки -->

РЭЦП НЕ ДОЛЖНА содержать пустой элемент `SignedDataObjectProperties`.

### 10.2.6 Контейнер `UnsignedSignatureProperties`

Контейнер `UnsignedSignatureProperties`, вложенный в `UnsignedProperties`,
содержит атрибуты, которые описывают подпись или подписанта. 

Синтаксис `UnsignedSignatureProperties` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="UnsignedSignatureProperties"
    type="UnsignedSignaturePropertiesType"/>
    <xsd:complexType name="UnsignedSignaturePropertiesType">
    <xsd:choice maxOccurs="unbounded">
    <xsd:element ref="CounterSignature" />
    <xsd:element ref="SignatureTimeStamp" />
    <xsd:element ref="CompleteCertificateRefs"/>
    <xsd:element ref="CompleteRevocationRefs"/>
    <xsd:element ref="AttributeCertificateRefs"/>
    <xsd:element ref="AttributeRevocationRefs" />
    <xsd:element ref="SigAndRefsTimeStamp" />
    <xsd:element ref="RefsOnlyTimeStamp" />
    <xsd:element ref="CertificateValues" />
    <xsd:element ref="RevocationValues"/>
    <xsd:element ref="AttrAuthoritiesCertValues" />
    <xsd:element ref="AttributeRevocationValues"/>
    <xsd:element ref="ArchiveTimeStamp" />
    <xsd:any namespace="##other"/>
    </xsd:choice>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType>  

Вложенные элементы-атрибуты описываются в 10.5 - 10.7.

<!-- todo: проверить fwd-ссылки -->

РЭЦП НЕ ДОЛЖНА содержать пустой элемент `UnsignedSignatureProperties`.

РЭЦП НЕ ДОЛЖНА содержать экземпляров `xsd:any`.

<!-- todo: что это значит? -->

### 10.2.7 Контейнер `UnsignedDataObjectProperties`

Контейнер `UnsignedDataObjectProperties`, вложенный в `UnsignedProperties`,
содержит неподписанные атрибуты, которые описывают объекты подписанного
документа.

Синтаксис `UnsignedDataObjectProperties` определяется следующей XML-схемой: 
    
     <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="UnsignedDataObjectProperties" type="UnsignedDataObjectPropertiesType"/>
    <xsd:complexType name="UnsignedDataObjectPropertiesType">
    <xsd:sequence>
    <xsd:element name="UnsignedDataObjectProperty" type="AnyType" maxOccurs="unbounded"/>
    </xsd:sequence>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType>  

Вложенные элементы-атрибуты имеют тип `AnyType`, описанный в 10.4.1.

<!-- todo: проверить fwd-ссылки -->

РЭЦП НЕ ДОЛЖНА содержать пустой элемент `UnsignedDataObjectProperties`.

### 10.2.8 Элемент `QualifyingPropertiesReference`

Элемент `QualifyingPropertiesReference` содержит ссылку на контейнер 
`QualifyingProperties`, который не является потомком элемента `ds:Signature` 
(например, хранится в другом XML-документе).

Синтаксис `QualifyingPropertiesReference` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="QualifyingPropertiesReference"
    type="QualifyingPropertiesReferenceType"/>
    <xsd:complexType name="QualifyingPropertiesReferenceType">
    <xsd:attribute name="URI" type="xsd:anyURI" use="required"/>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType>  

XML-атрибут `URI` содержит простое имя фрагмента XPointer и 
ссылается на контейнер `QualifyingProperties`. Часть значения 
данного XML-атрибута, которая не является фрагментом XPointer, ДОЛЖНА 
идентифицировать документ, в котором содержится контейнер, а фрагмент 
XPointer ДОЛЖЕН идентифицировать сам контейнер. 

<!--- todo: написано непонятно (фрагмент XPointer) -->

## 10.3 Включение атрибутов в подпись

### 10.3.1 Способы включения

Для включения атрибутов в РЭЦП используется элемент `ds:Object`. 
Включение выполняется 2 способами: 
 
1. Прямое включение. В элемент `ds:Object` вкладывается элемент 
`QualifyingProperties`.
2. Непрямое включение. В элемент `ds:Object` вкладывается один или более
элементов `QualifyingPropertiesReference`. Каждый из этих элементов
содержит ссылку на один элемент `QualifyingProperties`. При этом
элемент `QualifyingProperties` НЕ ДОЛЖЕН быть потомком элемента 
`ds:Signature`. 

На включение накладываются следующие ограничения:

- все экземпляры `QualifyingProperties`, напрямую включенные в РЭЦП, и все 
экземпляры `QualifyingPropertiesReference` ДОЛЖНЫ содержаться в элементе 
`ds:Object`;
- в элементе `ds:Object` МОЖЕТ содержаться не более одного экземпляра 
`QualifyingProperties`; 
- все подписанные атрибуты ДОЛЖНЫ содержаться в одном элементе 
`QualifyingProperties`. Этот элемент ДОЛЖЕН быть либо дочерним для 
элемента `ds:Object` (прямое включение), либо на него должен ссылаться
элемент `QualifyingPropertiesReference` (непрямое включение);
- в элементе `ds:Object` МОЖЕТ содержаться любое число 
(в том числе ни одного) экземпляров `QualifyingPropertiesReference`.

Кроме элементов `ds:Object`, содержащих `QualifyingProperties` или 
`QualifyingPropertiesReference`, РЭЦП МОЖЕТ содержать элементы 
`ds:Object` с другим содержимым. Ограничения на порядок следования 
элементов `ds:Object` в элементе `ds:Signature`, представляющем РЭЦП, не 
накладываются.

### 10.3.2 Подписанные атрибуты

Все подписанные атрибуты ДОЛЖНЫ быть дочерними для элемента 
`SignedProperties`, вложенного в элемент `QualifyingProperties`. 

Элемент `ds:Reference`, вложенный в РЭЦП, ДОЛЖЕН быть составлен таким 
образом, чтобы элемент `SignedProperties` учитывался при вычислении
хэш-значения.

Элемент `ds:Reference` ДОЛЖЕН включать XML-атрибут `Type` со 
значением "http://uri.etsi.org/01903#SignedProperties". Это значение 
говорит о том, что данные, использованные для вычисления хэш-значения, 
являются элементом `SignedProperties`.

## 10.4 Вспомогательные типы данных

### 10.4.1 Тип `AnyType`

Тип `AnyType` описывает последовательность произвольных XML-элементов, 
которые имеют неограниченную длину. В элементах типа допускается 
неограниченное число XML-атрибутов.

Тип `AnyType` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="Any" type="AnyType"/>   
    <xsd:complexType name="AnyType" mixed="true">  
    <xsd:sequence minOccurs="0" maxOccurs="unbounded">  
    <xsd:any namespace="##any" processContents="lax"/>  
    </xsd:sequence>  
    <xsd:anyAttribute namespace="##any"/>  
    </xsd:complexType>  

### 10.4.2 Тип `ObjectIdentifierType`

Тип `ObjectIdentifierType` описывает уникальный постоянный идентификатор
объекта данных. Тип дополнительно поддерживает описание объекта и ссылки на
документы, в которых имеется дополнительная информация об объекте.

Тип `ObjectIdentifierType` определяется следующей XML-схемой:

     <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="ObjectIdentifier" type="ObjectIdentifierType"/>  
    <xsd:complexType name="ObjectIdentifierType">  
    <xsd:sequence>  
    <xsd:element name="Identifier" type="IdentifierType"/>  
    <xsd:element name="Description" type="xsd:string" minOccurs="0"/>  
    <xsd:element name="DocumentationReferences"  
    type="DocumentationReferencesType" minOccurs="0"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:complexType name="IdentifierType">  
    <xsd:simpleContent>  
    <xsd:extension base="xsd:anyURI">  
    <xsd:attribute name="Qualifier" type="QualifierType"  
    use="optional"/>  
    </xsd:extension>  
    </xsd:simpleContent>  
    </xsd:complexType>  
    <xsd:simpleType name="QualifierType">  
    <xsd:restriction base="xsd:string">  
    <xsd:enumeration value="OIDAsURI"/>  
    <xsd:enumeration value="OIDAsURN"/>  
    </xsd:restriction>  
    </xsd:simpleType>  
    <xsd:complexType name="DocumentationReferencesType">  
    <xsd:sequence maxOccurs="unbounded">  
    <xsd:element name="DocumentationReference" type="xsd:anyURI"/>  
    </xsd:sequence>  
    </xsd:complexType>  

Элемент `Identifier` cодержит постоянный (неизменяемый)
идентификатор. Поддерживаются два механизма идентификации: 

- объекту назначается URI-идентификатор. В этом случае 
элемент `Identifier` ДОЛЖЕН содержать URI-идентификатором, 
а XML-атрибут `Qualifier` НЕ ДОЛЖЕН присутствовать;
- объекту назначается идентификатор АСН.1. В этом случае 
значение элемента `Identifier` ДОЛЖНО содержать идентификатор АСН.1, 
закодированный как URI или как URN. Если идентификатор закодирован как 
URN, то XML-атрибут `Qualifier` ДОЛЖЕН принимать значение "OIDAsURN". 
Если объектный идентификатор закодирован как URI, который не является URN, 
то XML-атрибут `Qualifier` ДОЛЖЕН принимать значение "OIDAsURI". 

Если объекту назначен и идентификатор АСН.1, и URI-идентификатор, 
то в элементе `Identifier` СЛЕДУЕТ отдавать предпочтение 
URI-идентификатору.

Элемент `Description` содержит неформальный текст, описывающий объект. 

Элемент `DocumetationReferences` содержит произвольное количество 
ссылок, указывающих на пояснительную документацию по объекту данных. 

### 10.4.3 Тип `EncapsulatedPKIDataType`

Тип `EncapsulatedPKIDataType` используется для включения в РЭЦП 
аттестатов. 

Тип `EncapsulatedPKIDataType` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="EncapsulatedPKIData" type="EncapsulatedPKIDataType"/>  
    <xsd:complexType name="EncapsulatedPKIDataType">  
      <xsd:simpleContent>  
        <xsd:extension base="xsd:base64Binary">  
          <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
          <xsd:attribute name="Encoding" type="xsd:anyURI" use="optional"/>  
        </xsd:extension>  
      </xsd:simpleContent>  
    </xsd:complexType> 

Тип инкапсулирует кодовое представление аттестата. Правила кодирования
определяются XML-атрибутом `Encoding`. Этот атрибут может принимать 
следующие значения: 

- "http://uri.etsi.org/01903/v1.2.2#DER" -- аттестат представляет собой 
АСН.1-данные и для их кодирования используются отличительные правила; 
- "http://uri.etsi.org/01903/v1.2.2#BER" -- аттестат представляет собой 
АСН.1-данные и для их кодирования используются базовые правила.

<!-- todo: убрал 3 набора правил (нужны доп. ссылки)

- "http://uri.etsi.org/01903/v1.2.2#CER" -- 
используются канонические правила кодирования, определенные в [CER];
- "http://uri.etsi.org/01903/v1.2.2#PER" -- 
используются packed правила кодирования, определенные в [PER];
- "http://uri.etsi.org/01903/v1.2.2#XER" -- 
используются XML-правила кодирования, определенные в [XER].
-->

Отсутствие `Encoding` означает, что аттестат представляет собой 
АСН.1-данные, закодированные с помощью отличительных правил. 

Полученная в результате кодирования строка октетов кодируется еще раз 
по правилам Base64. 

### 10.4.4 Типы данных для управления штампами времени

#### 10.4.4.3 Тип `GenericTimeStampType`

Тип `GenericTimeStampType` является базовым для остальных типов,
описывающих штампы времени. Тип спроектирован так, что в РЭЦП можно
включать штампы времени как в формате СТБ 34.101.ts, так и в формате XML.
Можно включать несколько штампов для одних и тех же данных (например,
штампы времени выпущенные различными службами). Штампы можно проставлять на
элементы РЭЦП, на объекты подписанного документа, на внешние данные. 
Можно указывать, какие элементы покрывает штамп и как следует обрабатывать 
эти элементы, чтобы вычислить хэш-значение, отсылаемое службе штампов 
времени. 

Тип `GenericTimeStampType` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="Include" type="IncludeType"/>  
    <xsd:complexType name="IncludeType">  
    <xsd:attribute name="URI" type="xsd:anyURI" use="required"/>  
    <xsd:attribute name="referencedData" type="xsd:boolean" use="optional"/>  
    </xsd:complexType>  
    <xsd:element name="ReferenceInfo" type="ReferenceInfoType"/>  
    <xsd:complexType name="ReferenceInfoType">  
    <xsd:sequence>  
    <xsd:element ref="ds:DigestMethod"/>  
    <xsd:element ref="ds:DigestValue"/>  
    </xsd:sequence>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
    <xsd:attribute name="URI" type="xsd:anyURI" use="optional"/>  
    </xsd:complexType>  
    <xsd:complexType name="GenericTimeStampType" abstract="true">  
    <xsd:sequence>  
    <xsd:choice minOccurs="0">  
    <xsd:element ref="Include" minOccurs="0" maxOccurs="unbounded"/>  
    <xsd:element ref="ReferenceInfo" maxOccurs="unbounded"/>  
    </xsd:choice>  
    <xsd:element ref="ds:CanonicalizationMethod" minOccurs="0"/>  
    <xsd:choice maxOccurs="unbounded">  
    <xsd:element name="EncapsulatedTimeStamp" 
    type="EncapsulatedPKIDataType"/>  
    <xsd:element name="XMLTimeStamp" type="AnyType"/>  
    </xsd:choice>  
    </xsd:sequence>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
    </xsd:complexType>  

Элемент `ds:CanonicalizationMethod` определяет алгоритм канонизации 
XML-элементов, которые покрывает штамп. 

Элемент `EncapsulatedTimeStamp` содержит штамп времени в формате,
определенном в СТБ 34.101.ts. 

Элемент `XMLTimeStamp` содержит штамп времени в формате XML. 

#### 10.4.4.4 Тип `XAdESTimeStampType`

Тип `XAdESTimeStampType` используется для включения в РЭЦП штампов 
времени, проставленных на элементы РЭЦП и, возможно, подписанного документа.

Тип `XAdESTimeStampType` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="XAdESTimeStamp" type="XAdESTimeStampType"/>  
    <xsd:complexType name="XAdESTimeStampType">  
    <xsd:complexContent>  
    <xsd:restriction base="GenericTimeStampType">  
    <xsd:sequence>  
    <xsd:element ref="Include" minOccurs="0" maxOccurs="unbounded"/>  
    <xsd:element ref="ds:CanonicalizationMethod" minOccurs="0"/>  
    <xsd:choice maxOccurs="unbounded">  
    <xsd:element name="EncapsulatedTimeStamp" type="EncapsulatedPKIDataType"/>  
    <xsd:element name="XMLTimeStamp" type="AnyType"/>  
    </xsd:choice>  
    </xsd:sequence>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
    </xsd:restriction>  
    </xsd:complexContent>  
    </xsd:complexType> 

Штампы времени, определенные в п.п. 10.7.8.1, 10.8.1, 10.10.2.2, 10.9.5 и 
10.9.6, неявно определяют покрытые ими элементы РЭЦП или подписанного 
документа.

<!--todo: пересмотреть кросс-ссылки после перенумерации-->

Элемент `Include` определяет покрытие явно. Элемент содержит набор ссылок 
на элементы, которые учитываются при вычислении хэш-значения, 
используемого в дальнейшем при постановке штампа времени. Порядок ссылок 
задает порядок следования элементов при их хэшировании.

Ссылка задается в XML-атрибуте `URI` элемента `Include`.

Если штамп времени и покрытый им элемент находятся в одном документе,
то ссылка ДОЛЖНА быть простым именем фрагмента XPointer.

Если штамп времени и покрытый им элемент находятся в разных документах, то
ссылка ДОЛЖНА представлять собой путь с простым именем фрагмента XPointer.

При этом если покрытый штампом элемент обернут РЭЦП, то основная (т.е. та,
которая не является фрагментом XPointer) часть ссылки ДОЛЖНА быть равна
основной части XML-атрибута `Target` элемента `QualifyingProperties`.

Если же покрытый штампом элемент не обернут РЭЦП, то основная часть ссылки 
ДОЛЖНА быта равна основной части XML-атрибута `URI` элемента 
`QualifyingPropertiesReference`, который ссылается на элемент 
`QualifyingProperties`.

<!-- todo: правила серъезно переписаны. Надо проверить --> 

Если элемент, на который ссылается XML-атрибут `URI`, не является элементом
`ds:Reference`, то XML-атрибут `referencedData` НЕ ДОЛЖЕН включаться в
`Include`. В противном случае, XML-атрибут `referencedData` МОЖЕТ
включаться.

<!--- раздел вроде очевидный, но в то же время без него чувствуется некая 
неполнота описания. Под сомнением, так как использует много терминологии 
XPointer и XPath. Upd: перечитывая, убедился, что и без этого текст выглядит 
нормально.

#######10.6.4.4.2.2 Обработка XML-атрибута `URI`

Полученный ресурс ДОЛЖЕН быть разобран, а потом простое имя XPointer ДОЛЖНО быть обработано.

Простое имя XPointer ДОЛЖНО быть обработано следующим образом: 
-  1) Использовать в качестве контекста XPointer корневой элемент XML-документа, а который ссылается основная часть XML-атрибута `URI`;  
-  2) Получить набор узлов XPath, как показано ниже:
-     b) удалить все комментарии.

#######10.6.4.4.2.2 Обработка элемента `Include`

Каждый элемент `Include` внутри атрибута для штампа времени ДОЛЖЕН быть 
обработан следующим образом: 
-  1) Получить объект данных, указанный в XML-атрибуте URI, как указано в 
8.6.4.4.2.2; 
-  2) если полученный объект данных является элементом `ds:Reference` и 
XML-атрибут `referencedData` установлен в значение "true", то обработать 
элемент в соответствии с моделью описанной в XMLDSIG [1] глава 4.4.3.2. В 
противном случае оставить в том же виде; 
-  3) если полученный объект является набором XML-узлов, то необходимо 
канонизировать этот объект, как указано в 8.2; 
-  4) объединить полученные октеты с другими входными данными, на которые 
необходимо проставить штамп времени. 
-->

#### 10.4.4.5 Тип `OtherTimeStampType`

Тип `OtherTimeStampType` описывает штампы времени на внешние объекты 
данных.

Тип `OtherTimeStampType` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="OtherTimeStamp" type="OtherTimeStampType"/>  
    <xsd:complexType name="OtherTimeStampType">  
    <xsd:complexContent>  
    <xsd:restriction base="GenericTimeStampType">  
    <xsd:sequence>  
    <xsd:element ref="ReferenceInfo" maxOccurs="unbounded"/>  
    <xsd:element ref="ds:CanonicalizationMethod" minOccurs="0"/>  
    <xsd:choice>  
    <xsd:element name="EncapsulatedTimeStamp"  
    type="EncapsulatedPKIDataType"/>  
    <xsd:element name="XMLTimeStamp" type="AnyType"/>  
    </xsd:choice>  
    </xsd:sequence>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:restriction>
    </xsd:complexContent 
    </xsd:complexType>  

Каждый элемент `ReferenceInfo` содержит хэш-значение одного внешнего 
объекта данных. Элемент `ds:DigestMethod` определяет используемый алгоритм 
хэширования. Элемент `ds:DigestValue` содержит хэш-значение объекта данных.
Хэш-значение кодируется по правилам Base64. 

## 10.5 Базовые атрибуты

### 10.5.1 Атрибут `SigningTime`

Атрибут `SigningTime` задается одноименным элементом.

Синтаксис элемента `SigningTime` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="SigningTime" type="xsd:dateTime"/>

### 10.5.2 Атрибут `SigningCertificate`

Атрибут `SigningCertificate` задается элементом `SigningCertificateV2`. 

Синтаксис `SigningCertificateV2` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="SigningCertificateV2" type="CertIDListV2Type"/>  
    <xsd:complexType name="CertIDListV2Type">  
    <xsd:sequence>
    <xsd:element name="Cert" type="CertIDTypeV2" maxOccurs="unbounded"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:complexType name="CertIDTypeV2">  
    <xsd:sequence>  
    <xsd:element name="CertDigest" type="DigestAlgAndValueType"/>  
    <xsd:element name="IssuerSerialV2" type="xsd:base64Binary" minOccurs="0"/>  
    </xsd:sequence>  
    <xsd:attribute name="URI" type="xsd:anyURI" use="optional"/>  
    </xsd:complexType>  
    <xsd:complexType name="DigestAlgAndValueType">  
    <xsd:sequence>  
    <xsd:element ref="ds:DigestMethod"/>  
    <xsd:element ref="ds:DigestValue"/>  
    </xsd:sequence>  
    </xsd:complexType>  

Элемент `CertDigest` содержит хэш-значение целевого сертификата. При этом
вложенный в `CertDigest` элемент `DigestMethod` определяет алгоритм
хэширования, а вложенный элемент `DigestValue` - хэш-значение сертификата,
закодированное по правилам Base64.

Элемент `IssuerSerialV2` содержит кодовое представление экземпляра типа
`IssuerSerial`. При кодировании сначала применяются отличительные правила
(DER), а затем правила Base64.

XML-атрибут `URI` содержит ссылку, по которой размещается целевой
сертификат. Ссылка не является обязательной, поскольку существуют и другие
способы получения сертификата.

Подписант НЕ ДОЛЖЕН создавать XML-атрибут `URI` для дочерних элементов
элемента `Cert`.

Ссылки на сертификаты НЕ ДОЛЖНЫ содержать элемент `IssuerSerialV2`.

### 10.5.3 Атрибут `CommitmentTypeIndication`

Атрибут `CommitmentTypeIndication` задается одноименным элементом.

Синтаксис `CommitmentTypeIndication` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="CommitmentTypeIndication"type="CommitmentTypeIndicationType"/>  
    <xsd:complexType name="CommitmentTypeIndicationType">  
    <xsd:sequence>  
    <xsd:element name="CommitmentTypeId"  
    type="ObjectIdentifierType"/>  
    <xsd:choice>  
    <xsd:element name="ObjectReference" type="xsd:anyURI"  
    maxOccurs="unbounded"/>  
    <xsd:element name="AllSignedDataObjects"/>  
    </xsd:choice>  
    <xsd:element name="CommitmentTypeQualifiers"  
    type="CommitmentTypeQualifiersListType" minOccurs="0"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:complexType name="CommitmentTypeQualifiersListType">  
    <xsd:sequence>  
    <xsd:element name="CommitmentTypeQualifier"  
    type="AnyType" minOccurs="0" maxOccurs="unbounded"/>  
    </xsd:sequence>  
    </xsd:complexType>  

Элемент `CommitmentTypeId` описывает обязательство подписанта. Вложенный
элемент `Identifier` содержит URI-идентификатор обязательства. При этом
элемент `Identifier` НЕ ДОЛЖЕН иметь XML-атрибута `Qualifier`, т.е.
URI-идентификатор НЕ ДОЛЖЕН представлять идентификатор АСН.1.

Каждый элемент `ObjectReference` ссылается на один элемент `ds:Reference`
внутри элемента `ds:SignedInfo` или `ds:Manifest`.

Если обязательство касается только части подписанных данных, то
элемент `CommitmentTypeIndication` ДОЛЖЕН содержать элемент
`ObjectReference` для каждого из подписанных объектов данных, на которые
направлено данное обязательство. Если определенное обязательство направлено
на все подписанные объекты, то элемент `CommitmentTypeIndication` ДОЛЖЕН
содержать один пустой элемент `AllSignedDataObjects` или один элемент
`ObjectReference` для каждого из подписанных объектов, кроме элемента
`SignedProperties`.

Элемент `CommitmentTypeQualifiers` позволяет задать дополнительную 
информации об обязательстве. 

### 10.5.4 Атрибут `DataObjectFormat`

Атрибут `DataObjectFormat` задается одноименным элементом.

Синтаксис `DataObjectFormat` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="DataObjectFormat" type="DataObjectFormatType"/>
    <xsd:complexType name="DataObjectFormatType">
    <xsd:sequence>
    <xsd:element name="Description" type="xsd:string" minOccurs="0"/>
    <xsd:element name="ObjectIdentifier" type="ObjectIdentifierType"
    minOccurs="0"/>
    <xsd:element name="MimeType" type="xsd:string" minOccurs="0"/>
    <xsd:element name="Encoding" type="xsd:anyURI" minOccurs="0"/>
    </xsd:sequence>
    <xsd:attribute name="ObjectReference" type="xsd:anyURI"
    use="required"/>
    </xsd:complexType>

Элемент `Description` содержит текстовую информацию, связанную с 
целевым объектом. 

Элемент `ObjectIdentifier` содержит идентификатор типа целевого 
объекта. Идентификатор назначается службой, которая объявляла тип. 

Элемент `MimeType` описывает MIME-тип целевого объекта. 

Элемент `Encoding` описывает кодировкк целевого объекта. 

Элемент `DataObjectFormat` ДОЛЖЕН содержать, по крайней мере, один из 
элементов `Description`, `ObjectIdentifier` или `MimeType`. 

XML-атрибут `ObjectReference` ссылается на элемент `ds:Reference`, 
вложенный в `ds:SignedInfo`, или на подписанный элемент `ds:Manifest`, 
ссылающийся на целевой объект.

Если элемент `DataObjectFormat` ссылается на элемент `ds:Reference`, 
который в свою очередь ссылается на элемент `ds:Object`, и если этот 
элемент `ds:Object` содержит элемент `MimeType` и атрибут `Encoding`, 
то эти элемент `MimeType` и XML-атрибут ДОЛЖНЫ иметь одинаковые значения. 

Для каждого целевого подписанного объекта, кроме элемента 
`SignedProperties`, ДОЛЖЕН быть создан один элемент `DataObjectFormat`. 

Элемент `DataObjectFormat` ДОЛЖЕН содержать:

- не более одного экземпляра `Description`;
- не более одного экземпляра `ObjectIdentifier`;
- ровно один экземпляр `MimeType`;
- не более одного экземпляра `Encoding`;
- ровно один экземпляр `ObjectReference`.

### 10.5.5 Атрибут `SignerLocation`

Атрибут `SignerLocation` задается элементом `SignatureProductionPlaceV2`.

Синтаксис `SignatureProductionPlaceV2` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="SignatureProductionPlaceV2"   type="SignatureProductionPlaceV2Type"/>  
    <xsd:complexType name="SignatureProductionPlaceV2Type">  
    <xsd:sequence>  
    <xsd:element name="City" type="xsd:string" minOccurs="0"/>  
    <xsd:element name="StreetAddress" type="xsd:string" minOccurs="0"/>  
    <xsd:element name="StateOrProvince" type="xsd:string" minOccurs="0"/>  
    <xsd:element name="PostalCode" type="xsd:string" minOccurs="0"/>  
    <xsd:element name="CountryName" type="xsd:string" minOccurs="0"/>  
    </xsd:sequence>  
    </xsd:complexType>  

Пустой элемент `SignatureProductionPlaceV2` НЕ ДОЛЖЕН включаться в РЭЦП.

### 10.5.6 Атрибут `SignerAttributes`

Атрибут `SignerAttributes` задается элементом `SignerRoleV2`.

Синтаксис `SignerRoleV2` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="SignerRoleV2" type="SignerRoleV2Type"/>  
    <xsd:complexType name="SignerRoleV2Type">  
    <xsd:sequence>  
    <xsd:element ref="ClaimedRoles" minOccurs="0"/>  
    <xsd:element ref="CertifiedRolesV2" minOccurs="0"/>  
    <xsd:element ref="SignedAssertions" minOccurs="0"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:element name="ClaimedRoles" type="ClaimedRolesListType"/>  
    <xsd:element name="CertifiedRolesV2" type="CertifiedRolesListTypeV2"/>  
    <xsd:element name="SignedAssertions" type="SignedAssertionsListType"/>  
    <xsd:complexType name="ClaimedRolesListType">  
    <xsd:sequence>  
    <xsd:element name="ClaimedRole" type="AnyType" maxOccurs="unbounded"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:complexType name="CertifiedRolesListTypeV2">  
    <xsd:sequence>  
    <xsd:element name="CertifiedRole" type="CertifiedRoleTypeV2"   maxOccurs="unbounded"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:complexType name="CertifiedRoleTypeV2">  
    <xsd:choice>  
    <xsd:element ref="X509AttributeCertificate"/>  
    <xsd:element ref="OtherAttributeCertificate"/>  
    </xsd:choice>  
    </xsd:complexType>  
    <xsd:element name="X509AttributeCertificate" type="EncapsulatedPKIDataType"/>  
    <xsd:element name="OtherAttributeCertificate" type="AnyType"/>  
    <xsd:complexType name="SignedAssertionsListType">  
    <xsd:sequence>  
    <xsd:element ref="SignedAssertion" maxOccurs="unbounded"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:element name="SignedAssertion" type="AnyType"/>  

Элемент `ClaimedRoles` содержит непустую последовательность 
ролей, заявленную пользователем, но не заверенную.

Элемент `CertifiedRolesV2` содержит непустую последовательность 
заверенных ролей пользователя. Заверенные роли представляют
собой атрибутные сертификаты. Сертификаты СТБ 34.101.67 ДОЛЖНЫ кодироваться 
по правилам Base64 и размещаться в элементе `X509AttributeCertificate`.
Атрибутные сертификаты других типов должны размещаться в 
`OtherAttributeCertificate`. Их содержание выходит за рамки настоящего
стандарта. 

Элемент `SignedAssertions` содержит непустую последовательность
утверждений, подписанных некоторым поставщиком услуг доверия. Подписанное
утверждение сильнее, чем заявленная роль, но слабее, чем атрибутный
сертификат. Содержание `SignedAssertions` выходит за рамки 
настоящего стандарта.

Пустой элемент `SignerRoleV2` не ДОЛЖЕН создаваться.

### 10.5.7 Контрподписи

### 10.5.7.1 Контрподпись с идентификатором

Контрподписи назначается URI-идентификатор 
"http://uri.etsi.org/01903#CountersignedSignature".
Если РЭЦП содержит элемент `ds:Reference`, в XML-атрибуте `Type` которого 
указан данный идентификатор, то РЭЦП является контрподписью, а 
элемент `ds:Reference` при этом ссылается на основную подпись. 

Элемент `ds:Reference` контрподписи ДОЛЖЕН быть построен так, чтобы 
подписывался элемент `ds:SignatureValue` основной подписи. 
При обработке `ds:Reference` ДОЛЖНЫ применяться все правила XML-DSig. 

### 10.5.7.2 Атрибут `CounterSignature`

Атрибут `CounterSignature` задается одноименным элементом.

Синтаксис `CounterSignature` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="CounterSignature" type="CounterSignatureType" />
    <xsd:complexType name="CounterSignatureType">
    <xsd:sequence>
    <xsd:element ref="ds:Signature"/>
    </xsd:sequence>
    </xsd:complexType>

Содержимым данного элемента является подпись в формате XML-DSig или 
XAdES, в которой элемент `ds:SignedInfo` содержит один элемент 
`ds:Reference`, ссылающийся на элемент `ds:SignatureValue` вложенной и 
контрподписанной РЭЦП.

<!-- todo: плохой абзац, переписать -->

Элемент `ds:DigestValue` вышеупомянутого элемента `ds:Reference` 
контрподписи содержит хэш-значение полного (и канонизированного) 
элемента `ds:SignatureValue`(т.е. включая открывающий и закрывающий теги).
Хэш-значение кодируется по правилам Base64. 

### 10.5.8 Штампы времени на объекты данных

<!-- todo: объекты данных? -->

#### 10.5.8.1 Атрибут `AllDataObjectsTimeStamp`

Атрибут `AllDataObjectsTimeStamp` задается одноименным элементом.

Синтаксис `AllDataObjectsTimeStamp` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="AllDataObjectsTimeStamp" type="XAdESTimeStampType"/>

Элемент `AllDataObjectsTimeStamp` описывает штамп времени от всех 
элементов `ds:Reference` внутри элемента `ds:SignedInfo`, кроме одного, 
ссылающегося на элемент `SignedProperties`. Покрытые элементы ДОЛЖНЫ 
описываться в штампе неявно. 

Покрытые элементы ДОЛЖНЫ обрабатываться в порядке их появления. ДОЛЖНА 
применяться модель обработки ссылок, определенная в [XML-DSIG] (раздел 
4.4.3.2). После обработки (перед хэшированием) элементы ДОЛЖНЫ 
канонизироваться. 

#### 10.5.8.2 Атрибут `IndividualDataObjectsTimeStamp`

Атрибут `IndividualDataObjectsTimeStamp` задается одноименным элементом.

Синтаксис `IndividualDataObjectsTimeStamp` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="IndividualDataObjectsTimeStamp" type="XAdESTimeStampType"/>

Элемент `IndividualDataObjectsTimeStamp` описывает штамп времени от
объектов данных, заданных явно во вложенных в штамп элементах `Include`.
Элементы `Include` ДОЛЖНЫ быть скомпонованы так, чтобы ссылаться на те
элементы `ds:Reference`, которые ссылаются на объекты данных, на которые
будет проставлен штамп времени.

<!-- todo: очень непонятная фраза -->

В каждом элементе `Include` ДОЛЖЕН присутствовать XML-атрибут 
`referencedData` ДОЛЖЕН присутствовать в каждом элементе со значением 
"true". 

Обработка данных перед хэшированием выполняется также, как в п. 10.5.8.1. 

<!--todo: нумерация-->

## 10.5.9 Атрибут `SignaturePolicyIdentifier`

### 10.5.9.1 Синтаксис

Атрибут `SignaturePolicyIdentifier` задается одноименным элементом.

Синтаксис `SignaturePolicyIdentifier` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="SignaturePolicyIdentifier" type="SignaturePolicyIdentifierType"/>
    <xsd:complexType name="SignaturePolicyIdentifierType">
    <xsd:choice>
    <xsd:element name="SignaturePolicyId" type="SignaturePolicyIdType"/>
    <xsd:element name="SignaturePolicyImplied"/>
    </xsd:choice>
    </xsd:complexType>
    <xsd:complexType name="SignaturePolicyIdType">
    <xsd:sequence>
    <xsd:element name="SigPolicyId" type="ObjectIdentifierType"/>
    <xsd:element ref="ds:Transforms" minOccurs="0"/>
    <xsd:element name="SigPolicyHash" type="DigestAlgAndValueType"/>
    <xsd:element name="SigPolicyQualifiers"
    type="SigPolicyQualifiersListType" minOccurs="0"/>
    </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="SigPolicyQualifiersListType">
    <xsd:sequence>
    <xsd:element name="SigPolicyQualifier" type="AnyType"
    maxOccurs="unbounded"/>
    </xsd:sequence>
    </xsd:complexType>

Элемент `SignaturePolicyId` используется для ссылки на политику подписи в 
явном виде. 

Элемент `SigPolicyId` идентифицирует конкретную версию политики подписи.

Элемент `ds:Transforms` содержит преобразования, выполненные над
документом политики подписи, перед вычислением хэш-значения. Модель
обработки для данных преобразований (трансформаций) ДОЛЖНА быть такой, как
задано в [XML-DSIG].

Элемент `SigPolicyHash` содержит идентификатор алгоритма 
хэширования и хэш-значение, полученное после обработки 
`SigPolicyId` и `ds:Transforms`, если таковые присутствуют.

### 10.5.9.2 Квалификаторы политики подписи

Определены три квалификатора политики подписи:

- URL, по которому можно получить копию политики подписи (элемент `SPURI`, 
определенный в пространстве имен "http://uri.etsi.org/01903/v1.3.2#");
- примечание, отображаемое пользователю при каждой проверке подписи 
(элемент `SPUserNotice`, определенный в пространстве имен 
"http://uri.etsi.org/01903/v1.3.2#");
- идентификатор технической спецификации, определяющий синтаксис, 
используемый для создания документа, содержащего политику подписи 
(элемент `SPDocSpecification`, определенный в пространстве имен 
"http://uri.etsi.org/01903/v1.4.1#").

Синтаксис элементов `SPURI` и `SPUserNotice` определяется следующей
XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="SPURI" type="xsd:anyURI"/>
    <xsd:element name="SPUserNotice" type="SPUserNoticeType"/>
    <xsd:complexType name="SPUserNoticeType">
    <xsd:sequence>
    <xsd:element name="NoticeRef" type="NoticeReferenceType"
    minOccurs="0"/>
    <xsd:element name="ExplicitText" type="xsd:string"
    minOccurs="0"/>
    </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="NoticeReferenceType">
    <xsd:sequence>
    <xsd:element name="Organization" type="xsd:string"/>
    <xsd:element name="NoticeNumbers" type="IntegerListType"/>
    </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="IntegerListType">
    <xsd:sequence>
    <xsd:element name="int" type="xsd:integer" minOccurs="0"
    maxOccurs="unbounded"/>
    </xsd:sequence>
    </xsd:complexType>

Элемент `ExplicitText` содержит текст отображаемого уведомления.

<!-- todo: пропущено описание многих полей. Почему? --> 

### 10.5.10 Атрибут `SignaturePolicyStore`

Атрибут `SignaturePolicyStore` задается одноименным элементом.

Синтаксис `SignaturePolicyStore` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#" -->  
    <xsd:element name="SignaturePolicyStore" type="SignaturePolicyStoreType"/>  
    <xsd:complexType name="SignaturePolicyStoreType">  
    <xsd:sequence>  
    <xsd:element ref="SPDocSpecification"/>  
    <xsd:choice>  
    <xsd:element name="SignaturePolicyDocument" type="xsd:base64Binary"/>  
    <xsd:element name="SigPolDocLocalURI" type="xsd:anyURI"/>  
    </xsd:choice>  
    </xsd:sequence>  
    </xsd:complexType>  

Элемент `SignaturePolicyDocument` содержит описание политики подписи,
закодированное по правилам Base64.

Элемент `SigPolDocLocalURI` содержит URI-идентификатор, который ссылается 
на локальное хранилище, в котором размещено описание политики подписи.
В отличие от `SPuri`, `SigPolDocLocalURI` указывает на локальный файл.

Элемент `SPDocSpecification` указывает на техническую спецификацию, 
определяющую синтаксис политики подписи.

Атрибут `SignaturePolicyStore` может содержаться в подписи только в паре с 
атрибутом `SignaturePolicyIdentifier`, который содержит хэш-значение 
описания политики подписи в элементе `SigPolicyHash`. В противном случае 
атрибут `SignaturePolicyStore` НЕ ДОЛЖЕН включаться в подпись.

### 10.6 Атрибут `SignatureTimeStamp`

Синтаксис элемента `SignatureTimeStamp` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="SignatureTimeStamp" type="XAdESTimeStampType"/>

Элемент описывает штамп времени от элемента `ds:SignatureValue`. 
Покрытый элемент ДОЛЖЕН описываться в штампе неявно.

## 10.7 Атрибуты-аттестаты

### 10.7.1 Атрибут `CompleteCertificateReferences`

Атрибут `CompleteCertificateReferences` задается элементом 
`CompleteCertificateRefsV2`.

Синтаксис `CompleteCertificateRefsV2` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#"    -->
    <xsd:element name="CompleteCertificateRefsV2" type="xades:CertIDListV2Type"/>

Если атрибут включен в подпись, то все указанные в нем сертификаты
ДОЛЖНЫ присутствовать либо в элементе `ds:KeyInfo`, либо в атрибуте 
`CertificateValues`.

### 10.7.2 Атрибут `CompleteRevocationReferences`

Атрибут `CompleteRevocationReferences` задается элементом 
`CompleteRevocationRefs`. 

Синтаксис `CompleteRevocationRefs` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="CompleteRevocationRefs"
    type="CompleteRevocationRefsType"/>
    <xsd:complexType name="CompleteRevocationRefsType">
    <xsd:sequence>
    <xsd:element name="CRLRefs" type="CRLRefsType" minOccurs="0"/>
    <xsd:element name="OCSPRefs" type="OCSPRefsType" minOccurs="0"/>
    <xsd:element name="OtherRefs" type="OtherCertStatusRefsType"
    minOccurs="0"/>
    </xsd:sequence>
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>
    </xsd:complexType>
    <xsd:complexType name="CRLRefsType">
    <xsd:sequence>
    <xsd:element name="CRLRef" type="CRLRefType"
    maxOccurs="unbounded"/>
    </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="CRLRefType">
    <xsd:sequence>
    <xsd:element name="DigestAlgAndValue"
    type="DigestAlgAndValueType"/>
    <xsd:element name="CRLIdentifier" type="CRLIdentifierType"
    minOccurs="0"/>
    </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="CRLIdentifierType">
    <xsd:sequence>
    <xsd:element name="Issuer" type="xsd:string"/>
    <xsd:element name="IssueTime" type="xsd:dateTime" />
    <xsd:element name="Number" type="xsd:integer" minOccurs="0"/>
    </xsd:sequence>
    <xsd:attribute name="URI" type="xsd:anyURI" use="optional"/>
    </xsd:complexType>
    <xsd:complexType name="OCSPRefsType">
    <xsd:sequence>
    <xsd:element name="OCSPRef" type="OCSPRefType"
    maxOccurs="unbounded"/>
    </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="OCSPRefType">
    <xsd:sequence>
    <xsd:element name="OCSPIdentifier" type="OCSPIdentifierType"/>
    <xsd:element name="DigestAlgAndValue"
    type="DigestAlgAndValueType"
    minOccurs="0"/>
    </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="ResponderIDType">
    <xsd:choice>
    <xsd:element name="ByName" type="xsd:string"/>
    <xsd:element name="ByKey" type="xsd:base64Binary"/>
    </xsd:choice>
    </xsd:complexType>
    <xsd:complexType name="OCSPIdentifierType">
    <xsd:sequence>
    <xsd:element name="ResponderID" type="ResponderIDType"/>
    <xsd:element name="ProducedAt" type="xsd:dateTime"/>
    </xsd:sequence>
    <xsd:attribute name="URI" type="xsd:anyURI" use="optional"/>
    </xsd:complexType>
    <xsd:complexType name="OtherCertStatusRefsType">
    <xsd:sequence>
    <xsd:element name="OtherRef" type="AnyType"
    maxOccurs="unbounded"/>
    </xsd:sequence>
    </xsd:complexType>

Пустой атрибут `CompleteRevocationRefs` НЕ ДОЛЖЕН включаться в подпись.

Элемент `CRLRefs` содержит последовательность ссылок на СОС. 

Каждый элемент `CRLRef`, дочерний для `CRLRefs`, содержит ссылку на СОС. 

Вложенный в `CRLRef` элемент `DigestAlgAndValue` 
содержит идентификатор алгоритма хэширования и хэш-значение СОС.
Хэш-значение кодируется по правилам Base64. Перед хэшированием СОС 
кодируется по отличительным правилам (DER).

Вложенный в `CRLRef` элемент `CRLIdentifier` содержит имя эмитента в 
элементе `Issuer`, время создания СОС в элементе `IssueTime` и
номер СОС в необязательном элементе `Number`.

XML-атрибут `URI` элемента `CRLRef` является адресом, по которому
можно получить СОС.

Если один или более из указанных СОС является ПСОС, 
то атрибут `CompleteRevocationRefs` содержит ссылки на все СОС,
необходимые для формирования полного списка.

Элемент `OCSPRefs` содержит последовательность ссылок на OCSP-ответы.
Каждый элемент `OCSPRef`, вложенный в `OCSPRefs`, содержит одну ссылку на 
один OCSP-ответ. 

Вложенный в `OCSPRef` элемент `OCSPIdentifier` содержит
идентификатор OCSP-сервера в элементе `ResponderID`.
Если OCSP-сервер идентифицируется по имени, то это имя 
включается в элемент `ByName`, дочерний для `ResponderID`.

<!--- todo: предлагаю не говорить здесь 
Включение атрибута `RevocationValues` в РЭЦП профиля B-LT или B-LTA
опредяется согласно требованиям, определенным в пункте 10.10.2.2 
(шаг 4.b). 
-->
<!--todo: нумерация-->

### 10.7.3 Атрибут `AttributeCertificateReferences`

Атрибут `AttributeCertificateReferences` задается элементом 
`AttributeCertificateRefsV2`.

Синтаксис `AttributeCertificateRefsV2` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#"-->
    <xsd:element name="AttributeCertificateRefsV2" type="xades:CertIDListV2Type"/>

### 10.7.4 Атрибут `AttributeRevocationReferences`

Атрибут `AttributeRevocationReferences` задается элементом 
`AttributeRevocationRefs`.

Синтаксис `AttributeRevocationRefs` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="AttributeRevocationRefs" type="CompleteRevocationRefsType"/>
    
<!--- todo: предлагаю не говорить здесь 
Атрибут `AttributeRevocationRefs` ДОЛЖЕН включаться в РЭЦП профилей B-LT и 
B-LTA  в соответствии с требованиям, описанными в п. 10.10.2.2, шаг 4.d.
-->
<!---нумерация-->

<!-- todo: в посл. абзаце было RevocationValues -->

### 10.7.5 Атрибут `SigAndRefsTimeStamp`

Атрибут `SigAndRefsTimeStamp` задается элементом `SigAndRefsTimeStampV2`. 

Синтаксис `SigAndRefsTimeStampV2` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#"-->
    <xsd:element name="SigAndRefsTimeStampV2" type="xades:XAdESTimeStampType"/>

<!-- todo: заголовок -- атрибут, затем атрибут |-> элемент, затем разговор 
об элементе. Везде ли так? Правильно ли так?  --> 

Элемент `SigAndRefsTimeStampV2` описывает штамп времени, 
который покрывает следующие элементы РЭЦП: 

- элемент `ds:SignatureValue`; 
- все атрибуты `SignatureTimeStamp`, входящие в РЭЦП;
- атрибут `СompleteCertificateReferences`; 
- атрибут `CompleteRevocationReferences`;
- атрибут `AttributeCertificateReferences` (если присутствует);
- атрибут `AttributeRevocationReferences` (если присутствует).

Покрытые элементы ДОЛЖНЫ хэшироваться в том порядке, в котором они указаны 
выше. Перед хэшированием каждый из элементов ДОЛЖЕН канонизироваться.

<!-- todo: требование канонизации многократно повторяется? М.б. один раз в 
начале? --> 

Если штамп `SigAndRefsTimeStampV2` и все неподписанные атрибуты, 
на которые он проставлен, имеют общий родительский элемент, 
то покрытые элементы ДОЛЖНЫ описываться в штампе неявно. 
Хэш-значение штампа ДОЛЖНО вычисляться от элемента `ds:SignatureValue`
и тех из элементов, которые встречаются до штампа.

Если штамп `SigAndRefsTimeStampV2` и некоторые неподписанные атрибуты, 
на которые он проставлен, не имеют общего родительского элемента, 
то ссылка на `ds:SignatureValue` ДОЛЖНА задаваться неявно, а ссылки 
на все остальные элементы - явно, с помощью вложенных в штамп элементов 
`Include`.

### 10.7.6 Атрибут `RefsOnlyTimeStamp`

Атрибут `RefsOnlyTimeStamp` задается элементом `RefsOnlyTimeStampV2`. 

Синтаксис `RefsOnlyTimeStampV2` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#"-->
    <xsd:element name="RefsOnlyTimeStampV2" type="xades:XAdESTimeStampType"/>

Элемент `RefsOnlyTimeStampV2` описывает штамп времени, 
который покрывает следующие элементы РЭЦП: 

- все атрибуты `SignatureTimeStamp`, присутствующие в РЭЦП;
- атрибут `СompleteCertificateReferences`; 
- атрибут `CompleteRevocationReferences`;
- атрибуты `AttributeCertificateReferences` (если присутствует);
- атрибут `AttributeRevocationReferences`  (если присутствует).

Покрытые элементы ДОЛЖНЫ хэшироваться в том порядке, в котором они указаны 
выше. Перед хэшированием каждый из элементов ДОЛЖЕН канонизироваться.

Если штамп `RefsOnlyTimeStampV2` и все неподписанные атрибуты, 
на которые он проставлен, имеют общий родительский элемент, 
то покрытые элементы ДОЛЖНЫ описываться в штампе неявно. 
Хэш-значение штампа ДОЛЖНО вычисляться от элемента `ds:SignatureValue`
и тех из элементов, которые встречаются до штампа.

Если штамп `RefsOnlyTimeStampV2` и некоторые неподписанные атрибуты, 
на которые он проставлен, не имеют общего родительского элемента, 
то ссылки на атрибуты ДОЛЖНЫ задаваться явно, с помощью вложенных в штамп 
элементов `Include`.

### 10.7.7 Атрибут `CertificateValues`

Атрибут `CertificateValues` задается одноименным элементом.

Синтаксис `CertificateValues` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="CertificateValues" type="CertificateValuesType"/>  
    <xsd:complexType name="CertificateValuesType">  
    <xsd:choice minOccurs="0" maxOccurs="unbounded">  
    <xsd:element name="EncapsulatedX509Certificate"  
    type="EncapsulatedPKIDataType"/>  
    <xsd:element name="OtherCertificate" type="AnyType"/>  
    </xsd:choice>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
    </xsd:complexType>

Элемент `EncapsulatedX509Certificate` содержит СОК в формате, 
определеннном в СТБ 34.101.19. Сертификат ДОЛЖЕН кодироваться по правилам 
Base64. 

Элемент `OtherCertificate` зарезервирован для будущих альтернативных версий 
сертификата.

<!--- todo: предлагаю не говорить здесь 
Атрибут `CertificateValues` ДОЛЖЕН включаться в РЭЦП профилей B-LT и B-LTA 
в соответствии с требованиям, описанными в п. 10.10.2.2, шаг 4.а.
-->
<!--todo: нумерация-->

### 10.7.8 Атрибут `RevocationValues`

Атрибут `RevocationValues` задается одноименным элементом.

Синтаксис `RevocationValues` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="RevocationValues" type="RevocationValuesType"/>  
    <xsd:complexType name="RevocationValuesType">  
    <xsd:sequence>  
    <xsd:element name="CRLValues" type="CRLValuesType"  
    minOccurs="0"/>
    <xsd:element name="OCSPValues" type="OCSPValuesType"  
    minOccurs="0"/>  
    <xsd:element name="OtherValues" type="OtherCertStatusValuesType"
    minOccurs="0"/>  
    </xsd:sequence>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
    </xsd:complexType>  
    <xsd:complexType name="CRLValuesType">  
    <xsd:sequence>  
    <xsd:element name="EncapsulatedCRLValue"
    type="EncapsulatedPKIDataType"
    maxOccurs="unbounded"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:complexType name="OCSPValuesType">  
    <xsd:sequence>  
    <xsd:element name="EncapsulatedOCSPValue"
    type="EncapsulatedPKIDataType" maxOccurs="unbounded"/>  
    </xsd:sequence>  
    </xsd:complexType>  
    <xsd:complexType name="OtherCertStatusValuesType">  
    <xsd:sequence>
    <xsd:element name="OtherValue" type="AnyType"
    maxOccurs="unbounded"/>
    </xsd:sequence>  
    </xsd:complexType>

Элемент `CRLValues` содержит последовательность СОС в 
формате, определенном в СТБ 34.101.19. Каждый вложенный в `CRLValues` 
элемент `EncapsulatedCRLValue` содержит отдельный СОС. СОС ДОЛЖЕН 
кодироваться по правилам Base64.

Элемент `OCSPValues` содержит последовательность OCSP-ответов.
Каждый вложенный в `CRLValues` элемент `EncapsulatedOCSPValue` содержит 
отдельный OCSP-ответ. 

Элемент `OtherValues` зарезервирован для будущих альернативных версий 
аттестатов отзыва. Семантика и синтаксис этого элемента выходят 
за рамки настоящего стандарта. 

### 10.7.9 Атрибут `AttributeCertificateValues`

Атрибут `AttributeCertificateValues` задается элементом
`AttrAuthoritiesCertValues`.

<!-- todo: правильно? (было одноименным) -->

Синтаксис `AttributeCertificateValues` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->  
    <xsd:element name="AttrAuthoritiesCertValues" type="CertificateValuesType"/>

<!--- todo: предлагаю не говорить здесь 
Атрибут `AttributeCertificateValues` ДОЛЖЕН включаться в РЭЦП профилей 
B-LT и B-LTA в соответствии с требованиям, описанными в п. 10.10.2.2, шаг 4.а.
-->
<!--todo: нумерация-->

### 10.7.10 Атрибут `AttributeRevocationValues`  

Атрибут `AttributeRevocationValues` задается одноименным элементом.

Синтаксис `AttributeRevocationValues` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.3.2#" -->
    <xsd:element name="AttributeRevocationValues" type="RevocationValuesType"/>

## 10.8 Атрибуты архивной проверки

### 10.8.1 Атрибут `TimeStampValidationData`

Если РЭЦП должна включать все аттестаты, необходимые для проверки штампа 
времени, но некоторые из аттестатов отсутствуют, то ДОЛЖЕН быть создан 
атрибут `TimeStampValidationData`, содержащий недостающие аттестаты.
Этот атрибут ДОЛЖЕН быть добавлен в контейнер `UnsignedSignatureProperties` 
сразу после целевого штампа времени.

Атрибут `TimeStampValidationData` описывается одноименным элементом.

Синтаксис `TimeStampValidationData` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#"-->  
    <xsd:element name="TimeStampValidationData" type="ValidationDataType"/>  
    <xsd:complexType name="ValidationDataType">  
    <xsd:sequence>  
    <xsd:element ref="xades:CertificateValues" minOccurs="0"/>  
    <xsd:element ref="xades:RevocationValues" minOccurs="0"/>  
    </xsd:sequence>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
    <xsd:attribute name="URI" type="xsd:anyURI" use="optional"/>  
    </xsd:complexType>

Элемент `CertificateValues` содержит сертификаты, 
используемые при проверке штампов времени.

Элемент `RevocationValues` содержит аттестаты отзыва, 
используемые при проверке штампов времени.

Если включенные в `TimeStampValidationData` аттестаты относятся к штампу
`SignatureTimeStamp`, `RefsOnlyTimeStamp`, `SigAndRefsTimeStamp` или 
`ArchiveTimeStamp`, то атрибут `URI` НЕ ДОЛЖЕН присутствовать.

Если аттестаты относятся к штампам `IndividualDataObjectsTimeStamp` или 
`AllDataObjectTimeStamp`, то XML-атрибут `URI` ДОЛЖЕН присутствовать и 
ссылаться на элемент, содержащий штамп.

### 10.8.2 Атрибут `ArchiveTimeStamp`

Перед созданием и добавлением к РЭЦП нового атрибута `ArchiveTimeStamp`
в подпись ДОЛЖНЫ быть включены все аттестаты, необходимые для проверки:

- СОК подписанта документа;
- СОК подписанта каждой контрподписи, включенной в РЭЦП;
- каждого атрибутного сертификата и подписанного заявления, включенных в 
РЭЦП; 
- СОК службы штампов времени для каждого штампа, включенного в РЭЦП.

Атрибут `ArchiveTimeStamp` МОЖЕТ содержать более одного штампа 
времени, выданных различными службами штампов времени.

Атрибут `ArchiveTimeStamp` задается одноименным элементом.

Синтаксис `ArchiveTimeStamp` определяется следующей XML-схемой: 

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#-->
    <xsd:element name="ArchiveTimeStamp" type="xades:XAdESTimeStampType"/> 

Если штамп `ArchiveTimeStamp` и все неподписанные атрибуты, на 
которые он проставлен, имеют общий родительский элемент, 
то покрытые элементы ДОЛЖНЫ описываться в штампе неявно. 
Строка октетов, которая хэшируется перед постановкой штампа,
ДОЛЖНА строиться следующим образом:

1. Начать с пустой результирующей строки октетов.
2. Обработать все ссылки `ds:Reference` в порядке их появления внутри 
элемента `SignedInfo`, в том числе ссылку на `SignedProperties`. 
Для каждой ссылки:
  - применить модель обработки ссылок, определенную в [XML-DSIG], 
    пункт 4.4.3.2; 
  - если результатом обработки является набор XML-элементов, то 
    канонизировать его; 
  - объединить полученные октеты с результирующей строкой октетов.
3. Обработать последовательно элементы `ds:SignedInfo`, 
`ds:SignatureValue`, `ds:KeyInfo` (если присутствует). 
Канонизировать каждый элемент и объединить полученные октеты с 
результирующей строкой октетов.
4. Обработать неподписанные атрибуты, которые встречаются до текущего
штампа `ArchiveTimeStamp`, в порядке их появления внутри элемента
`UnsignedSignatureProperties`. Канонизировать каждый из атрибутов и
объединить полученные октеты с результирующей строкой октетов. 
Применить следующие правила:
  + атрибут `CertificateValues` ДОЛЖЕН быть добавлен к РЭЦП, 
    если в подписи отсутствуют некоторые из СОК, необходимых для ее 
    проверки;
  + атрибут `RevocationValues` ДОЛЖЕН быть добавлен к РЭЦП, 
    если в подписи отсутствуют некоторые из аттестатов отзыва, 
    необходимых для ее проверки;
  + атрибут `AttributeCertificateValues` ДОЛЖЕН быть добавлен к РЭЦП, 
    если в подписи отсутствуют некоторые из СОК, необходимых для проверки 
    атрибутных сертификатов и подписанных утверждений, включенных в подпись. 
  + атрибут `AttributeRevocationValues` ДОЛЖЕН быть добавлен к РЭЦП,
    если в подписи отсутствуют некоторые из аттестатов отзыва, необходимых 
    для проверки атрибутных сертификатов и подписанных утверждений, 
    включенных в подпись.
5. Обработать все элементы `ds:Object`, кроме того, который содержит 
элемент `QualifyingProperties`, в порядке их появления. Канонизировать 
каждый из элементов и добавить к результирующей строке октетов.

Если штамп `ArchiveTimeStamp` и некоторые неподписанные атрибуты, 
на которые он проставлен, не имеют общего родительского элемента, 
то ссылки на неподписанные атрибуты ДОЛЖНЫ задаваться явно, с помощью 
вложенных в штамп элементов `Include`. 

Элемент `Include` ДОЛЖЕН создаваться для каждого неподписанного атрибута, 
уже включенного в РЭЦП. Элементы `Include` ДОЛЖНЫ добавляться в том же 
порядке, в котором неподписанные атрибуты обрабатываются при вычислении 
хэш-значения штампа времени. Элементы `Include` ДОЛЖНЫ создаваться только 
для неподписанных атрибутов. 

<!-- todo: а в каком порядке неподписанные атрибуты обрабатываются? 
я так и не понял -->

Строка октетов, по которой строится хэш-значение штампа, должна строиться
по описанному выше алгоритму. Только на шаге 4 атрибуты обрабатываются 
в порядке, заданном ссылками `Include`.

<!-- todo: важный вопрос. В чем отличие алгоритмов? Я так и не понял. 
Удаляются комментарии? Но это же часть канонизации. -->

### 10.8.3 Атрибут `RenewedDigests`

Атрибут `RenewedDigests` задается одноименным элементом.

Синтаксис `RenewedDigests` определяется следующей XML-схемой:

    <!-- targetNamespace="http://uri.etsi.org/01903/v1.4.1#" -->  
    <xsd:element name="RenewedDigests" type="RenewedDigestsType"/>  
    <xsd:complexType name="RenewedDigestsType">  
    <xsd:sequence>  
    <xsd:element ref="ds:DigestMethod"/>  
    <xsd:element ref="RecomputedDigestValue" maxOccurs="unbounded"/>  
    </xsd:sequence>  
    <xsd:attribute name="Id" type="xsd:ID" use="optional"/>  
    </xsd:complexType>  
    <xsd:element name="RecomputedDigestValue" type="RecomputedDigestValueType"/>  
    <xsd:complexType name="RecomputedDigestValueType">  
    <xsd:simpleContent>  
    <xsd:extension base="ds:DigestValueType">  
    <xsd:attribute name="Order" type="xsd:integer" use="required"/>  
    </xsd:extension>  
    </xsd:simpleContent>  
    </xsd:complexType>  

Элемент `ds:DigestMethod` задает алгоритм хеширования.

Каждый элемент `RecomputedDigestValue` содержит хэш-значение одного из
подписанных объекта данных, на которые имеются ссылки внутри элемента
`ds:Manifest`. Хэш-значение кодируется по правилам Base64.

При проверке РЭЦП, каждый элемент `RenewedDigests` ДОЛЖЕН обрабатываться 
следующим образом: 

1. Взять первый элемент `RecomputedDigestValue`.
2. Взять элемент `ds:Reference`, указанный в XML-атрибуте `Order` 
обрабатываемого элемента `RecomputedDigestValue`.
3. Обработать элемент `ds:Reference` в соответствии с моделью обработки 
ссылок, заданной в [XML-DSIG], раздел 4.4.3.2. Если невозможно получить 
ссылочные октеты, то уведомить об этом и перейти к шагу 6. 
4. Вычислить хэш-значение от полученных октетов.
5. Если вычисленное хэш-значение не совпадает с заявленным, то уведомить 
об этом и перейти к шагу 6.
6. Если остались необработанные элементы `RecomputedDigestValue`, взять 
следующий  такой элемент и перейти к шагу 2; в противном случае - 
закончить.

## 10.9 Требования по реализация РЭЦП формата XAdES

<!-- todo: суть дела не понял, при личной встрече. Почему имена со слешем? 
-->

Описанные в этом подразделе требования относятся ко всем базовым профилям.

РЭЦП любого базового профиля формата XAdES ДОЛЖНА содержать элементы, 
включенные только с использованием механизма явного включения.

Помимо атрибутов, указанных в разделе 7, РЭЦП ДОЛЖНА содержать следующие 
элементы:

- элемент `ds:KeyInfo/X509Data` (ровно один экземпляр);
- элемент `ds:SignedInfo/ds:CanonicalizationMethod` (ровно один 
экземпляр);
- элемент `ds:Reference`(более двух экземпляр).

Также РЭЦП МОЖЕТ содержать элемент `ds:Reference/ds:Transforms` 
(не более одного экземпляра).

К указанным выше элементам выдвигаются следующие 
требования (в скобках указан элемент к которому выдвигаются требования).

Подписант ДОЛЖЕН включать свой сертификат в 
`ds:KeyInfo/X509Data/X509Certificate`(элемент `ds:KeyInfo/X509Data`).

Подписанту РЕКОМЕНДУЕТСЯ включать в `ds:KeyInfo/X509Data` все сертификаты, 
которые могут понадобиться верификатору при построении цепочки 
сертификатов (элемент `ds:KeyInfo/X509Data`). 

Значение алгоритма канонизации, ДОЛЖНО быть выбрано из следующих 
вариантов (элемент `ds:SignedInfo/ds:CanonicalizationMethod`):

-  "http://www.w3.org/2006/12/xml-c14n11";
-  "http://www.w3.org/2001/10/xml-exc-c14n#";
-  "http://www.w3.org/TR/2001/REC-xml-c14n-20010315";
-  "http://www.w3.org/2006/12/xml-c14n11#WithComments";
-  "http://www.w3.org/2001/10/xml-exc-c14n#WithComments";
-  "http://www.w3.org/TR/2001/REC-xml-c14n-20010315#WithComments".

Подписанту НЕ РЕКОМЕНДУЕТСЯ использовать алгоритмы 
канонизации `"WithComments"`(элемент `ds:SignedInfo/ds:CanonicalizationMethod`).

Если выбранная трансформация является канонизацией, то XML-атрибут 
`Algorithm` ДОЛЖЕН принимать одно из значений, указанных в пункте 
для элемента `ds:SignedInfo/ds:CanonicalizationMethod`, и подписанту 
НЕ СЛЕДУЕТ использовать алгоритмы канонизации `"WithComments"` 
(элемент `ds:Reference/ds:Transforms`). 

Если трансформация, указанная атрибутом `Transform`, не является 
канонизацией, то атрибут `Algorithm` ДОЛЖЕН принимать одно из 
следующих значений (элемент `ds:Reference/ds:Transforms`):

-  "http://www.w3.org/2000/09/xmldsig#base64";
-  "http://www.w3.org/2001/10/xml-exc-c14n#";
-  "http://www.w3.org/TR/2001/REC-xml-c14n-20010315";
-  "http://www.w3.org/2006/12/xml-c14n11#WithComments";
-  "http://www.w3.org/2001/10/xml-exc-c14n#WithComments";
-  "http://www.w3.org/TR/2001/REC-xml-c14n-20010315#WithComments".

Аттестат проверки штампов времени ДОЛЖЕН быть включен в атрибут 
`TimeStampValidationData` или содержаться в самом штампе времени.

Аттестат проверки штампов времени СЛЕДУЕТ включать в 
атрибут `TimeStampValidationData`. 

СЛЕДУЕТ избегать дублирования сертификатов в пределах одной подписи.
