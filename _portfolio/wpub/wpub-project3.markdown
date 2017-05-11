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

<!-- PRESENTATION INFO IS REQUIRED -->
<!-- TABLE OF CONTENTS SLIDES AND NORMAL SLIDES CAN BE IN MIXED ORDER -->
<!-- BIBLIOGRAPHY CAN BE GENERATED ONLY AT THE END OF PRESENTATION (AUTOMATICALLY GENERATED WHEN SET, MAX ONCE)-->
<xs:complexType   name="presentationType">
  <xs:sequence>
    <xs:element   name="presentationInfo" type="presentationInfoType" minOccurs="1"       maxOccurs="1"/>
    <xs:choice    maxOccurs="unbounded">
      <xs:element name="toc"              type="tocType"              minOccurs="0"       maxOccurs="unbounded"/>
      <xs:element name="slide"            type="slideType"            minOccurs="1"       maxOccurs="unbounded"/>
    </xs:choice>
    <xs:element   name="bibliography"     type="bibliographyType"     maxOccurs="1"/>
  </xs:sequence>
  <!-- PRESENTATION BACKGROUND COLOR -->
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
<!-- NORMAL SLIDE HAS A TITLE, SUBTITLE IS OPTIONAL -->
<!-- CONTENT OF LIST IS MADE OF TEXTS OR LISTS ELEMENTS -->
<!-- EVERY SLIDE CAN HAVE MAX 1 IMAGE (OPTIONAL) -->
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

## Spôsob prekladu

Pre preloženie súboru presentation.xml umiestneného v tejto zložke zadajte v príkazovom riadku nasledujúci príkaz:
```
...\generate_presentation.bat presentation.xml
```
