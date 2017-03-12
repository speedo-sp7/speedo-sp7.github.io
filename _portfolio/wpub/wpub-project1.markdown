---
layout: project
title: Projekt 1 - Stránka v Yekyll
category: "Webové publikovanie"
tag: wpub

project_url: https://github.com/speedo-sp7/speedo-sp7.github.io

show_related_projects: yes
---

# Zadanie projektu

### Osobná webová prezentácia na GitHub pages

Vytvorte webovú prezentáciu (webové sídlo) o sebe. Zamerajte sa jednak na vaše profesné záujmy (napr. projekty, ktoré riešite/riešili ste, čo vás v informatike najviac baví, fascinuje = váš developerský profil) a jednak vaše osobné záujmy, hobby.

V rámci developerského profilu vytvorte sekciu Webové publikovanie, kde budete publikovať všetky tri vaše vypracované zadania z predmetu.

Využite pritom technológie Git + GitHub Pages + Jekyll + Markdown. Využite potenciál statického generátora Jekyll a jeho templatovacích možností.

Podrobné požiadavky na vypracovanie a odovzdanie zadania (priemerná úroveň kvality):

* Sídlo musí obsahovať aspoň 5 podstránok, pri využití aspoň 3 rôznych rozložení (layout-ov)
* V rámci šablon musí byť použité:
  * aspoň 5 premenných  
  * kolekcie alebo dátové súbory
  * aspoň 5 filtrov alebo tagov
  * aspoň 1 plugin (okrem pagination)

### Hodnotenie:

+ Štrukturálna úroveň sídla (využitie potenciálu nástroja Jekyll) – max 4b
+ Obsahová stránka sídla (zmysluplný obsah) – max 1b
+ Vizuálna úroveň (ako sídlo vyzerá, celková konzistencia) – max 0.5b
+ Stav projektu na GitHub-e – max 0.5b

_Pozor na plagiarizmus. Webové sídlo musí byť vaša pôvodná práca, nemôže byť príliš podobné inému existujúcemu sídlu či sídlu vytvorenému pomocou automatického generátora._

Poznámka: Keď budete vaše zadanie nasadzovať na GitHub Pages, plugin tam pravdepodobne nebude funkčný. Z pohľadu zadania je to v poriadku. Lokálna verzia však musí byť funkčná aj s pluginom.


# Opis riešenia

Pri tvorbe zadaného projektu webovej stránky sme využili jednotlivé technológie, ako vyplýva z požiadaviek zadania. Našu stránku sme uložili na Git, vytvorená bola pomocou Jekyll a jej obsah je tvorený aj za pomoci jazyka Markdown.

# Spustenie stránky na lokálnom serveri

Pre spustenie stránky na lokálnom počítači je nutné mať nainštalovaný jekyll (všetky potrebné informácie nájdete na oficiálnej stránke [Jekyll](https://jekyllrb.com/)).

Pre nainštalovanie nástroja jekyll v systéme osX spusťte nasledujúci príkaz:

```
[sudo] gem install jekyll

[sudo] gem install bundler
```

Následne je možné si projekt stiahnuť zo [zdrojového repozitára](https://github.com/speedo-sp7/speedo-sp7.github.io).


### Potrebné pluginy

Všetky gemy, ktoré sú potrebné pre správne fungovanie stránky nainštalujete spustením príkazu nudle install v adresári projektu.

```
cd speedo-sp7.github.io

bundle install
```

Pre spustenie jekyll servera je potrebné ešte spustiť nasledujúci príkaz

```
jekyll serve
```

Stránku je teraz možné zobraziť vo vašom webovom prehliadači na webovej adrese [localhost:4000](localhost:4000).


# Opis splnenia požiadaviek

V rámci splnenia požiadaviek projektu sme v implementácii našej webovej stránky zahrnuli:
* Hlavnú stránku spolu so 6 typmi podstránok (O mne, Blog, Blogový príspevok, Portfólio, Projekt a Kontakt)
  * Jednotlivé stránky pritom využívaju 7 základných rozložení: default, homepage, page, post, project, contact
* V rámci šablon sme použili:
  * premenné
    * **site.profile_image** - načítanie profilovej fotky uvedenej vo front matter hlavnej stránky
    * **page.title** - pre nastavenie nadpisu každej stránky okrem homepage (layout page)
    * **page.description** - pre uvedenie popisu stránku (uvedeného vo front matter každej stránky okrem homepage)(layout page)
    * **socials.href**, **social.title** - pre načítanie zadaných linkov v paneli socials (include)
    * **banner-image** - premenná uchovávajúca adresu bannera webovej stránky
    * a mnohé ďalšie (definované v súbore saas/variables), v ktorých nastavujeme základné parametre webovej stránky a je tak možné meniť štýly písma aleb farebnú schému stránky z jedného miesta
  * kolekcie a dátové súbory
    * kolekcia **Projekty** - táto kolekcia slúži na evidenciu projektov, pričom každý z projektov má definovanú kategóriu, a tak je možné ich usporiadať do spoločných celkov (portfolio)
    * **navigation.yml** - v tomto dátovom súbore sme zadefinovali základnú štruktúru našej webovej stránky (využité v includes/navigation)
    * **socials.yml** - tu sú uvedené názvy a linky na jendotlivé sociálne siete, na ktor=ych máme vedený účet (využité v includes/socials)
    * **portfolio_categories** - v tomto dátovom sóbore definujeme rozšírené názvy tagov pre kategórie portfólia (využité v portfolio)
    * **contacts** - zadefinovanie základných kontaktných údajov, ktoré si želáme zobraziť na stránke kontakty
  * filtrov a tagy
    * **date: "%-d. %b %Y"** - pre zobrazenie vhodného formátu dátumu pridania príspevku (v blog) a aktuálneho dátumu na hlavnej stránke
    * **upcase** - pre zobrazenie písmen tlačidiel menu veľkými písmenami (v include/navigation)
    * **for, if, else, endif, endfor** - základné konštrukcie pre zobrazovanie zoznamov a pod.
    * **include ...** - pre zahrnutie komponentu do iného komponentu (tvorba layoutov, obsahu stránok)
    * **number_of_words** - pre zobrazenie celkového počtu slov blogového príspevku (layout post)
  * pluginy
    * **jemoji** - plugin pre zobrazenie icon emoji namiesto použitých značiek (:wolf:, :smile:)
    * **jekyll-sitemap** - plugin pre vygenerovanie mapy stránky (pre zjednodušenie indexovania stránky prehľadávacími nástrojmi)
    * **image-optim** - plugin na optimalizáciu obrázkov (uložených v zložke images) (zdroj: [Github](https://github.com/chrisanthropic/image_optim-jekyll-plugin))


Pre jednoduchšie formátovanie obsahu webových stránok sme v projekte použili [Bootsrap](https://github.com/plusjade/jekyll-bootstrap). Niektoré parametre boli upravované za behu vytvárania vzhľadu stránky (odsadenia, štýly, veľkosti písma a podobne).

Použité ikony sociálnych sietí nájdete v sass/icomoon/base (zdroj [Icomoon.io](https://icomoon.io)).  
