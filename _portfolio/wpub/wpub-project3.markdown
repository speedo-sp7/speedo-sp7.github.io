---
layout: project
title: Projekt 3 - Prezentácia v XML
date: 2017-05-11 08:42:45 -08:00

category: "Webové publikovanie"
tag: wpub

project_url: https://github.com/speedo-sp7/speedo-sp7.github.io/blob/master/uploadedFiles/Z3-xvnencak.zip

show_related_projects: yes
---

# Zadanie projektu

### XML prezentácie

Analyzujte možnosti zápisu jednoduchej prezentácie v jazyku XML. Identifikujte základné súčasti prezentácie a navrhnite XML elementy pre ich označkovanie (metadátové, štrukturálne, inline). Dbajte na znovupoužiteľnosť a vyvarujte sa redundancií. Návrh elementov zrealizujte opísaním typu dokumentu pomocou vybraného jazyka (DTD, XSD, RELAX NG) spolu s vysvetlením účelu jednotlivých elementov. Vo vami navhrnutom jazyku vytvorte ukážkovú prezentáciu, ktorá bude demonštrovať možnosti tvorby prezentácií podľa definície typu dokumentu.

Navrhnite a vytvorte XSLT šablóny pre konverziu prezentácie z XML do XHTML+CSS a pre konverziu prezentácie z XML do PDF. Klaďte dôraz na znovupoužitie jednotlivých šablon pre viaceré výstupné formáty. Umožnite zadávanie parametrov transformácií.

Súčasťou požiadaviek na zadanie je vytvorenie správy o zadaní 3, v ktorej zdokumentujete splenie jednotlivých bodov zadania. Správa bude súčasťou vašej stránky o Webovom publikovaní na GitHube.

**Hodnotenie (max 14 bodov):**
* opis typu dokumentu + opis účelu navrhnutých elementov - možnosť výberu (práve jedného) z variantov:
  * DTD
  * XML Schema
  * RELAX NG
* vytvorenie ukážkovej XML prezentácie demonštrujúcej možnosti definície typu dokumentu
* základný návrh XSL transformácií, ich vhodnosť, parametrizácia
* vytvorenie XSLT pre konverziu prezentácie z XML -> XHTML+CSS
  * každý slide bude v samostatnom XHTML súbore
* vytvorenie XSLT pre konverziu prezentácie XML -> PDF

# Opis riešenia

Pre splnenie zadania sme sa rozhodli vytvoriť ukážkovú prezentáciu vo formáte, ktorú je možné zvalidovať prostredníctvom nami zadefinovanej schémy, a preložiť prostedníctvom nami vytvorenej XSLT transformácie.

## Opis typu dokumentu

Typ dokumentu sme opísali pomocou jazyka Schema (súbor presentationScheme.xsd v archíve projektu).

### Opis účelu navrhnutých elementov

#### presentation

Tento element predstavuje základný element prezentácie.

```
<xs:element name="presentation" type="presentationType"/>

<xs:complexType   name="presentationType">
  <xs:sequence>
    <xs:element   name="presentationInfo" type="presentationInfoType" minOccurs="1"       maxOccurs="1"/>
    <xs:choice    maxOccurs="unbounded">
      <xs:element name="toc"              type="tocType"              minOccurs="0"       maxOccurs="unbounded"/>
      <xs:element name="slide"            type="slideType"            minOccurs="1"       maxOccurs="unbounded"/>
    </xs:choice>
    <xs:element   name="bibliography"     type="bibliographyType"     maxOccurs="1"/>
  </xs:sequence>
  <xs:attribute name="backgroundColor"    type="xs:string"            use="optional"      default="gray"/>
</xs:complexType>
```
Medzi základné elementy tohto elementu patrí element presentationInfo, ktorý obsahuje základné informácie o vytváranej prezentácii. Tento element je nutné zadať pri vytváraní prezentácie. Obsah prezentácie a bibliografia je voliteľná (nemusí byť zadaná, prezentácia tak bude obsahovať iba prvý úvodný slide).

Jedn'm z atribútov prezentácie je aj backgroundColor, ktorý špecifikuje základnú farbu pozadia, ktorá sa použije v prezentácii. Tento atribút však nie je nutné zadávať, prednastavená hodnota je gray (sivá).

#### presentationInfo

Tento element obsahuje informácie o názve a autorovi prezentácie (povinné), a dobrovoľné informácie o mieste a dátume prezentovania.

#### slide

Tento element predstavuje samotný slide prezentácie.

```
<xs:complexType   name="slideType">
  <xs:sequence>
    <xs:element   name="title"          type="xs:string"            minOccurs="1"       maxOccurs="1"/>
    <xs:choice    minOccurs="0"         maxOccurs="1">
      <xs:element name="subtitle"       type="xs:string"/>
    </xs:choice>
    <xs:choice    minOccurs="0"         maxOccurs="unbounded">
      <xs:element name="text"           type="mixedTextType"            minOccurs="0"       maxOccurs="unbounded"/>
      <xs:element name="list"           type="listType"             minOccurs="0"       maxOccurs="unbounded"/>
    </xs:choice>
    <xs:choice    minOccurs="0"         maxOccurs="1">
      <xs:element name="image"          type="imageType"            minOccurs="1"       maxOccurs="1"/>
    </xs:choice>
  </xs:sequence>
  <xs:attribute name="id"               type="xs:string"            use="optional"/>
  <xs:attribute name="showPageNumber"   type="xs:boolean"           use="optional"      default="false"/>
  <xs:attribute name="slideTemplate"    type="template"             use="required"/>
</xs:complexType>
```

Každý slide musí mať nadpis (_title_), podnadpis (_subtitle_) je nepovinný.
Obsah prezentácie môže tvoriť kombinácia elementov text (v ktorom je možné kombinovať elementy citácie (_citation_ - na použitú literatúru), _href_ (externý odkaz) a _pref_ (odkaz v prezentácii)), zoznam (list) alebo obrázok (max 1).

Samotné rozloženie slidu si používateľ môže nastaviť na tri predvolené hodnoty:
* textový slide (_text_)
* slide s textom a obrázkom (_textImage_)
* obrázkový slide (_image_)

Pre zobrazenie čísla slidu na konkrétnom slide je nutné nastaviť atribút _showPageNumber_ na hodnotu _true_. Defaulte je tento parameter prednastavený na hodnotu _false_.

#### text

Predstavuje element obsahu slidu. Pozostáva zo zmiešaného obsahu textu, citácií (_citation_), referencií mimo (_href_) a v rámci dokumentu (_pref_).

```
<xs:complexType name="mixedTextType" mixed="true">
  <xs:sequence>
    <xs:element ref="citation"  minOccurs="0" maxOccurs="unbounded"/>
    <xs:element ref="pref"      minOccurs="0" maxOccurs="unbounded"/>
    <xs:element ref="href"      minOccurs="0" maxOccurs="unbounded"/>
  </xs:sequence>
  <xs:attribute name="fontSize" type="fontSize" use="optional"/>
</xs:complexType>
```

Pre text je môžné zadať veľkosť písma (v rozmedí 1 až 40) pomocou atribútu _fontSize_.

#### list

Jeden z elementov obsahu slidu. Predstavuje zoznam, ktorý môže byť čislovaný alebo nečíslovaný (na základe parametra _ordered_ - default nečíslovaný).

```
<xs:complexType name="listType">
  <xs:sequence minOccurs="1">
    <xs:element name="item"             type="xs:string"            minOccurs="1"       maxOccurs="unbounded"/>
  </xs:sequence>
  <xs:attribute name="ordered"          type="xs:boolean"           use="optional"      default="false"/>
  <xs:attribute name="fontSize"         type="fontSize"            use="optional"      default="18"/>
</xs:complexType>
```

List môže byť rovnako ako aj text upravený zmenou veľkosti textu (atribút _fontSize_).

## Ukážková XML prezentácia

V tejto časti je opísané vytvorenie XML prezentácie na základe opísaného typu dokumentu.

### Informácie o prezentácii

Pre vytvorenú prezentáciu sme nastavili názov prezentácie, autora, miesto aj dátum.

```
<presentationInfo>
  <title>Ako vytvoriť HTML prezentáciu</title>
  <author>
    <firstname>Stanislav</firstname>
    <surname>Vnenčák</surname>
  </author>
  <place>Fakulta informatiky a informačných technológií, STU Bratislava</place>
  <date>Máj 2017</date>
</presentationInfo>
```

### Ukážka slidu

Na ukážku uvádzame slide, ktorý je tvorený textovým obsahom, zoznamom a obrázkom (rozloženie: _textImage_)

Tento slide má nastavený atribút zobrazenia čislovania strán.

```
<slide slideTemplate="textImage" showPageNumber="true" id="SLI5">
  <title>Snímok obrázku a textu</title>
  <subtitle>Podnadpis (voliteľný)</subtitle>
  <text>
    Tento slide je určený na zobrazenie textov a zoznamov s obrázkom.
  </text>
  <text fontSize="22">
    Dávajte si pozor na zlé referencie zdrojov <citation>JANKUBU</citation> a prelikov na iné slidy <pref>STU</pref>.
  </text>
  <list ordered="true" fontSize="20">
    <item>Ordered 1</item>
    <item>Ordered 2</item>
  </list>
  <image style="width:280px; height=200px">
    <title>Obrázok</title>
    <href>https://images2.alphacoders.com/438/438454.jpg</href>
  </image>
</slide>
```

V prípade, že sa používateľ bude chcieť referencovať na tento slide, môže použiť element _pref_, v ktorom zadá hodnotu atribútu _id_ (vytvorí sa referencia na tento slide).

### Generovanie slidu s obsahom

V prípade, že sa používateľ rozhodne vygenerovať obsah pre nasledujúce slidy prezentácie, stačí ak použije element _toc_ a nastaví mu nadpis (_title_). Obsah sa vygeneruje automaticky (aj s referenciami na slidy v obsahu).

```
<toc>
  <title>Už sme skoro na konci...</title>
</toc>
```

Obsah je možné v prezentácii vygenerovať viackrát.

### Literatúra

Literatúra môže a nemusí byť uvedená na konci xml prezentácie. V prípade že je, vygeneruje sa pre ňu slide.

```
<bibliography>
  <title>Literatúra</title>

  <source>
    <mixed>WPUB, FIIT STU Bratislava</mixed>
    <href>https://www.fiit.stuba.sk</href>
    <abbrew>FIIT</abbrew>
  </source>
  <source>
    <mixed>Google</mixed>
    <href>https://www.google.com</href>
    <abbrew>GGL</abbrew>
  </source>
  <source>
    <mixed>Janka a Kubko. Vydavateľstvo: Martinus. 2006. ISBN 87989-424-89865</mixed>
    <href>https://www.jankakubko.sk</href>
    <abbrew>JANKUB</abbrew>
  </source>
</bibliography>
```

Na literatúru je možné sa referencovať pomocou elementu _citation_ (musí byť nastavená hodnota elementu abbrew, ak je zadaná referencia nesprávne, v texte sa vygeneruje upozornenie).


## Návrh konverzie prezentácie (XSLT) do formátu XHMTL + CSS

V tejto časti je opísaný návrh a spôsob konverzie prezentácie z formátu XML do formátu XHTML + CSS

### XSLT transformácia

Táto transformácia vykonáva na základe preddefinovaných pravidiel (uvedených v súbore presentationTransform.xsl) konverziu prezentácie do XHTML podoby.

Pri konverzii je vygenerovaný priečinok pre prezentáciu, do ktorého sú vygenerované html súbory (snímky prezentácie).

Pre spustenie transformácie je možne využiť skript priložený v archíve projektu (__generate_presentation.bat__). Pre transformáciu vami vytvorenej prezentácie v XML súbore stačí tento súbor použiť ako vstupný parameter uvedeného skriptu.

Medzi jednotlivými snímkami prezentácie je možné prepínať pomocou kláves right (vpred) a left (späť), táto funkcia však nefunguje vo všetkých prehliadačoch (testované na Safari, Chrome).

### CSS

Súbor CSS sme vytvorili samostatne a je vložený do elementu head každého vygenerované slidu.

```
<xsl:template name="slideHead">
  <link rel="stylesheet" type="text/css" href="presentationStyle.css"/>
  <title>
    <xsl:value-of select="title"/>
  </title>
</xsl:template>
```

V CSS dokumente je zadefinované formátovanie elementov vytvoreného HTML dokumentu (dodáva prezentácii finálny vzhľad).

## Spôsob prekladu

Pre preloženie súboru presentation.xml umiestneného v tejto zložke zadajte v príkazovom riadku nasledujúci príkaz:
```
...\generate_presentation.bat presentation.xml
```
