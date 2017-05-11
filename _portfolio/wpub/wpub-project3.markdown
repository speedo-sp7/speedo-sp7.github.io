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

Pre splnenie zadania sme sa rozhodli vytvoriť ukážkovú prezentáciu, ktorú je možné zvalidovať prostredníctvom zadefinovanej schémy.
