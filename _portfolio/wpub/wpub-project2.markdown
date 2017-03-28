---
layout: project
title: Projekt 2 - Transformácia vybraného dokumentu do formátu DocBook
date: 2017-03-27 11:23:12 -08:00

category: "Webové publikovanie"
tag: wpub

project_url: https://github.com/speedo-sp7/speedo-sp7.github.io/blob/master/uploadedFiles/Z2-xvnencak.zip

show_related_projects: yes
---

# Zadanie projektu

### Transformácia vybraného dokumentu do formátu DocBook

Predmetom 2. zadania je spracovanie vybraného dokumentu (ideálne bakalárskeho projektu) z pôvodného ľubovoľného (Word, OpenOffice, LaTeX, …) formátu do formátu DocBook a vygenerovanie cieľového tvaru v PDF. Výsledný dokument bude mať rozsah minimálne 10 a maximálne 15 strán. Do rozsahu sa nezapočítavajú úvodné strany (obsah, zoznamy obrázkov a tabuliek), použitá literatúra a prílohy.

Požadované a kontrolné konštrukcie sú:

* Štandardné členenie textu na kapitola, podkapitola, podpodkapitola, príloha, generovaný obsah
* Zvýraznenie slov, zvýraznenie členenia textu odrážkami alebo číslovaním
* Odkazy na iné časti vlastného dokumentu, prípadne odkazy na URL
* Poznámka pod čiarou
* Zoznam použitej literatúry a zdrojov vrátane ich citácie v texte
* Vloženie obrázku a tabuliek, odkazy na ne v texte; zoznam obrázkov a tabuliek v úvode alebo závere textu
* Vytvorenie registra pojmov (indexu) s pojmami hierarchicky usporiadanými do dvoch úrovni, napríklad „cykly, while“, „cykly, for“ (najmenej ako ukážku na 10-15 pojmoch na predvedenie práce s registrom)

Súčasťou požiadaviek na zadanie je vytvorenie správy o zadaní 2, ktorá bude súčasťou vašej stránky o Webovom publikovaní na GitHube. Správa o rozsahu 150-250 slov bude obsahovať informácie o použitých elementoch a atribútoch, prípadne ukážky XML/XSLT demonštrujúce vykonané prispôsobenie DocBook šablon (nepovinné).

# Opis riešenia

Pre splnenie zadania sme sa rozhodli pretvoriť našu bakalársku prácu na FIIT STU do DocBooku.

## Použité elementy

Zoznam a popis elementov použitých v práci:
* bookinfo - zadefinovanie základných informácií o práci
* abstract - súčasť informácií o práci, anotácia, čestné prehlásenie, poďakovanie
* mypara - vytvorený vlastný typ paragrafu bez odsadenia
* para - paragraf s odsadením
* gap - element na pridanie prázdneho riadku
* chapter - kapitola
* sect1 - podkapitola
* sect2 - podpodkapitola
* sect3 - podpodpodkapitola
* orderedlist - čislovaný zoznam
* itemizedlist - zoznam
* figure - obrázok, rovnica
* table -tabuľka
* indexterm - slovo z registra
* bibliography - bibliografia
* index - register pojmov
* appendix - príloha

## Vykonané zmeny
### Zmena úvodnej stránky dokumentu
Zmena názvu práce
```
<xsl:template name="book.titlepage.before.recto">
<fo:block xmlns:fo="http://www.w3.org/1999/XSL/Format" text-align="center" font-size="18pt" border-bottom-width="0.5pt" border-bottom-style="solid" font-family="sans-serif" text-transform="uppercase">
```

Zmena vedúceho práce
```
Vedúci bakalárskej práce:
<xsl:value-of select="/book/bookinfo/othername[@role='veduci']"/>
```

Zmena v zalamovaní textu
```
<xsl:template name="book.titlepage.before.recto"><fo:block xmlns:fo="http://www.w3.org/1999/XSL/Format" text-align="center" font-size="25pt" border-bottom-width="0.5pt" border-bottom-style="solid" font-family="sans-serif" text-transform="uppercase" hyphenate="false">
      <xsl:value-of select="/book/bookinfo//affiliation/orgname"/>
    </fo:block><fo:block xmlns:fo="http://www.w3.org/1999/XSL/Format" space-before="6pt" text-align="center" font-size="14pt" font-family="sans-serif">
      <xsl:value-of select="/book/bookinfo//affiliation/orgdiv[@role='fakulta']"/>
    </fo:block><fo:block xmlns:fo="http://www.w3.org/1999/XSL/Format" space-before="3pt" text-align="center" font-size="12pt" font-family="sans-serif">
      <xsl:value-of select="/book/bookinfo//affiliation/orgdiv[@role='katedra']"/>
    </fo:block><fo:block xmlns:fo="http://www.w3.org/1999/XSL/Format" space-before="12pt" space-after="1cm" text-align="center">
      <fo:external-graphic src="url(kizi.pdf)" width="2cm" content-width="scale-to-fit"/>
    </fo:block>
</xsl:template>
```

### Kapitoly
Zmena v pomenovaní kapitol (kapitálkami)
```
<xsl:attribute-set name="chap.label.properties.common">
  <xsl:attribute name="font-size">
    <xsl:value-of select="$body.font.master * 2"/>
    <xsl:text>pt</xsl:text>
  </xsl:attribute>
  <xsl:attribute name="font-weight">bold</xsl:attribute>
  <xsl:attribute name="hyphenate">false</xsl:attribute>
  <xsl:attribute name="margin-top">2em</xsl:attribute>
  <xsl:attribute name="space-after.minimum">1.5em</xsl:attribute>
  <xsl:attribute name="space-after.optimum">1.2em</xsl:attribute>
  <xsl:attribute name="space-after.maximum">1.8em</xsl:attribute>
</xsl:attribute-set>

<xsl:attribute-set name="chap.label.properties" use-attribute-sets="chap.label.properties.common">
  <xsl:attribute name="text-transform">capitalize</xsl:attribute>
</xsl:attribute-set>

<xsl:attribute-set name="chap.title.properties" use-attribute-sets="chap.label.properties.common">
  <xsl:attribute name="margin-top">0.5em</xsl:attribute>
</xsl:attribute-set>
```

### Zmena horizontálneho odsadenia blokov
Nastavenie odsadení
```
</fo:block><fo:block xmlns:fo="http://www.w3.org/1999/XSL/Format" space-before="3pt" space-after="5cm" text-align="center" font-size="12pt" font-family="sans-serif">
      <xsl:value-of select="/book/bookinfo//affiliation/orgdiv[@role='katedra']"/>
</fo:block>
```

### Formátovanie textu
Nastavenie riadkovania
```
<xsl:attribute-set name="normal.para.spacing">
  <xsl:attribute name="line-height">1.5em</xsl:attribute>
</xsl:attribute-set>
```

Zmena typu písma
```
<xsl:param name="body.font.family" select="'Times New Roman'"/>
<xsl:param name="title.font.family" select="'Times New Roman'"/>
<xsl:param name="monospace.font.family" select="'monospace'"/>
<xsl:param name="symbol.font.family" select="'Symbol,ZapfDingbats,LucidaUnicode'"/>
```

### Odsadenie textu
Odsadenie odstavcov
```
<xsl:attribute name="text-indent">1.5em</xsl:attribute>
```

Zadefinovanie elementu medzery
```
<xsl:template match="gap">
  <fo:block space-after="1em"></fo:block>
</xsl:template>
```

### Zoznamy
Zmena odsadenia
```
<xsl:attribute-set name="list.block.spacing">
  <xsl:attribute name="margin-left">15pt</xsl:attribute>
</xsl:attribute-set>
```

### Odstavce
Nastavenie rovnakého odsadenia pre všetky odstavce
```
<xsl:attribute-set name="normal.para.spacing">
  <xsl:attribute name="text-indent">3em</xsl:attribute>
  <xsl:attribute name="space-before.optimum">0em</xsl:attribute>
  <xsl:attribute name="space-before.minimum">0em</xsl:attribute>
  <xsl:attribute name="space-before.maximum">1em</xsl:attribute>
</xsl:attribute-set>
```

### Vlastné elementy
Zadefinovanie elementu mypara (para bez odsadenia)
```
<xsl:template match="mypara">   
  <fo:block xmlns:fo="http://www.w3.org/1999/XSL/Format" font-size="12pt" line-height="1.5em">
    <xsl:apply-templates/>  
  </fo:block>
</xsl:template>
```

### Referencie
Zmena štýlu referencií (kapitola, tabuľka, obrázok, podkapitola)
```
<xsl:param name="local.l10n.xml" select="document('')"/>
<l:i18n xmlns:l="http://docbook.sourceforge.net/xmlns/l10n/1.0">
  <l:l10n language="sk">
    <l:context name="xref-number-and-title">
		<l:template name="chapter" text="%n %t"/>
		<l:template name="table" text="Tabuľka %n - %t"/>
		<l:template name="figure" text="Obrázok %n - %t"/>
		<l:template name="sect1" text="%n %t"/>
    </l:context>    
  </l:l10n>
</l:i18n>
```

## Spôsob prekladu

Pre vygenerovanie PDF je potrebný procesor formátovacích objektov XEP.

Pre preloženie súboru mojabp.xml v zložke docbook_bp zadajte nasledujúci príkaz:
```
...\docbook_bp > pdf_xep.bat mojabp.xml
```
