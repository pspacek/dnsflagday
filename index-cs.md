---
title: 2019
lang: cs
redirect_from:
  - /cs/
---

Co se připravuje?
=================
Současný systém [DNS](https://cs.wikipedia.org/wiki/Domain_Name_System) je z pomalý a potýká se s technickými problémy způsobenými nedodžováním standardů. Pro celosvětový chod je nezbytné provést změny, aby bylo možné nasadit novou funkcionalitu jako je např. lepší ochrana před [DDoS útoky](https://cs.wikipedia.org/wiki/Denial_of_service#Distributed_DoS).

Z výše uvedených důvodů bude po **1. únoru 2019** ukončena podpora některých historických implementací DNS. Tato změna ovlivní jen domény používající software, který nedodržuje ani starší standard DNS z roku 1986 (RFC1034 a RFC1035) ani novější standard [EDNS]() z roku 1999 (RFC2535).

Pro více informací klikněte na skupinu, do které spadáte:

- [Jsem uživatel Internetu, nemám svou vlastní doménu](#users)
- [Jsem držitel domény](#domain-holders)
- [Jsem správce DNS](#dns-admins)
- [Zajímám se o DNS z odborného hlediska (vývoj DNS softwaru, výzkum, atd.)](#experts)

a neumožňuje nasadit novou funkcionalitu,  Pro zmírnění problému skupina [výrobců DNS software](#akci-podporují) a [významných poskytovatelů DNS infrastruktury](#akci-podporují) proto po **1. únoru 2019** ukončí podporu některých historických implementací DNS porušujících standardy.

Tato změna ovlivní jen domény používající software, který nedodržuje publikované standardy. Ovlivní i vás?

<a name="users"></a>
Jsem uživatel Internetu, nemám svou vlastní doménu
==================================================
Pokud nemáte vlastní doménu, není třeba se strachovat. Vás se změna týká pouze nepřímo a není nutné podnikat žádné další kroky.

<a name="domain-holders"></a>
Jsem držitel domény
===================
Jste-li držitel domény, zkontrolujte, zda je vaše doména připravena na změnu systému DNS pomocí následujícího formuláře. Výsledek testu vám v případě potřeby zároveň sdělí doporučený postup nápravy.

{% include checker.html lang=site.data.checker.cs %}

<a name="dns-admins"></a>
Jsem správce DNS
================

1. Správci DNS resolverů
------------------------
Skupina výrobců DNS software naplánovala na dny okolo 1. února 2019 vydání aktualizovaných verzí software, které ukončí podporu některých implementací DNS porušujících standardy. Je naplánováno vydání těchto verzí:

* BIND 9.13.3 (vývojová verze) a 9.14.0 (produkční)
* Knot Resolver ve všech současných verzích již implementuje striktní chování
* PowerDNS Recursor 4.2.0
* Unbound 1.9.0

Také [poskytovatelé veřejných DNS resolverů uvedení níže](#supporters) během krátkého období ukončí tuto podporu.

V případě, že 


2. Správci DNS serverů
----------------------
Po 1. únoru 2019 [poskytovatelé veřejných DNS resolverů uvedení níže](#supporters) během krátkého období ukončí podporu autoritativních serverů, které nedodržují starší standard DNS z roku 1986 (RFC1034 a RFC1035) ani novější standard EDNS z roku 1999 (RFC2535). Domény hostované na serverech které nedoržují standardy se tak pro významnou část uživatelů Internetu stanou nedostupnými.

Jako přípravu vám doporučujeme použít následující postup:
1. Otestujte své autoritativní servery pomocí [formuláře uvedeného výše](#domain-holders). Stačí otestovat libovolnou zónu hostovanou na vašich DNS serverech, není potřeba testovat všechny zóny jednotlivě.

2. Výsledek test může být ovlivněn dočasným problémem v síti. Pokud je detekován problém, zkuste opakujte test.

3. Pokud testy selhávají, aktualizujte váš DNS software na poslední stabilní verzi a zopakujte test. Pokud testy selhávají i po aktualizaci DNS software, doporučujeme vám zkontrolovat konfiguraci firewallu.

4. **Firewally nesmí zahazovat DNS pakety** ani když obsahují rozšíření EDNS, včetně dosud neznámých rozšíření splňující standard EDNS. Články jednotlivých výrobců naleznete zde:

* [Akamai](https://community.akamai.com/customers/s/article/CloudSecurityDNSFlagDayandAkamai20190115151216?language=en_US)
* [BlueCat](https://www.bluecatnetworks.com/blog/dns-flag-day-is-coming-and-bluecat-is-ready/)
* [F5 BIG-IP](https://support.f5.com/csp/article/K07808381?sf206085287=1)
* Juniper SRX ve výchozím nastavení zahazují EDNS pakety. Řešením je buď upgrade na polední verzi, nebo vypnutí funkce "DNS doctoring" příkazem `# set security alg dns doctoring none`.

Pokud jste provedli upgrade DNS software a problém přetrvává i po aplikaci postupů uvedených výše, prosím kontaktujte výrobce firewallu a požádejte podporu.

Technické informace
-------------------
Dále je možné otestovat vaše DNS servery pomocí nástroje [ednscomp](https://ednscomp.isc.org/ednscomp), který zobrazuje podrobnou technickou zprávu. Do pole `zone name` zadejte jméno jakékoliv zóny hostované na vašich DNS serverech a klikněte na tlačítko `Submit`.

Celkový výsledek zobrazený nástrojem [ednscomp](https://ednscomp.isc.org/ednscomp) by měla být zpráva `All Ok` (zelenou barvou).

Pro minimální konfiguraci, která ještě bude v roce 2019 fungovat, nevypisuje nástroj [ednscomp](https://ednscomp.isc.org/ednscomp) výsledek `timeout` v žádném z testů pro původní DNS ani v testech pro rozšíření EDNS verze 0. Vezměte prosím na vědomí, že takováto minimální konfigurace stále neodpovídá standardům a dříve nebo později bude způsobovat potíže. Z tohoto důvodu **doporučujeme najednou opravit vaše DNS tak, aby všechny testy skončily výsledkem `ok`**. Vyhnete se tak problémům v budoucnu.

Pokud bude zjištěn problém, nástroj ednscomp vám zobrazí vysvětlení pro každý test. Problémy jsou typicky způsobeny:
* zastaralým nebo nekvalitním DNS softwarem
* chybnou konfigurací firewallu

**Firewally nesmí zahazovat DNS pakety** které obsahují rozšíření EDNS (ani dosud neznámá rozšíření splňující standard EDNS). Moderní DNS software používá rozšíření jako např. [DNS cookies](https://tools.ietf.org/html/rfc7873) pro ochranu proti DoS útokům. Firewally které zahazují DNS pakety s rozšířeními ve skutečnosti zhoršují situaci pro všechny uživatele, včetně zhoršování DoS útoků a zpomalování DNS provozu.

<a name="experts"></a>
Zajímám se o DNS z odborného hlediska
=====================================

Vývojáři DNS software
---------------------
Hlavní změna spočívá v tom, že DNS software od výše uvedených výrobců bude nově interpretovat timeout (vypršení časového limitu požadavku) jako příznak problému na síti nebo vzdáleném serveru. Počínaje 1. únorem 2019 DNS timeout **nezpůsobí vypnutí EDNS**.

Důsledkem je, že DNS servery které **vůbec neodpovídají na EDNS dotazy** se stanou zcela nedostupnými.

Prosím otestujte svou implementaci DNS pomocí nástroje [ednscomp](https://ednscomp.isc.org/ednscomp) a ujistěte se, že správně zpracováváte rozšíření EDNS. Zdrojový kód testovacího nástroje je také [k dispozici](https://gitlab.isc.org/isc-projects/DNS-Compliance-Testing).

Upozorňujeme, že rozšíření EDNS stále není povinné. Pokud se rozhodnete nepodporovat EDNS, vše bude fungovat pokud se váš software bude řídit podle [EDNS standard section 7](https://tools.ietf.org/html/rfc6891#section-7).

Výzkumníci
----------
Další zdroje pro výzkumníky:
 * [Statistiky podpory EDNS](https://ednscomp.isc.org/) vygenerované pomocí [sady EDNS testů](https://gitlab.isc.org/isc-projects/DNS-Compliance-Testing) vytvořené sdružením ISC.
 * [EDNS skener zón](https://gitlab.labs.nic.cz/knot/edns-zone-scanner/) vytvořený sdružením CZ.NIC, který si klade za cíl vyhodnotit reálný dopad změny popsané na této stránce.

Před interpretací dat si prosím přečtěte metodologii uvedenou u konkrétního zdroje. S dotazy se neváhejte obrátit na autory pomocí odkazů do Gitlabu uvedených výše.

Prezentace
==========

 * Bude vaše doména fungovat i v roce 2019? - CSNOG1: [abstrakt](https://csnog.eu/event/1/contributions/33/), [prezentace](https://csnog.eu/event/1/contributions/33/attachments/9/31/Petr_Spacek_01.pdf), [video](https://youtu.be/KQO48HbY6o0)
 * DNS-OARC 28: [abstrakt](https://indico.dns-oarc.net/event/28/contributions/515/), [prezentace](https://indico.dns-oarc.net/event/28/contributions/515/attachments/490/799/Removing_EDNS_Workarounds.pdf), [video](https://www.youtube.com/watch?v=9YYH8JFH_bY&feature=youtu.be&t=5198)
 * LOADAYS 2018: [abstrakt](http://loadays.org/pages/dnsupdate.html), [prezentace](http://loadays.org/files/plexis-edns-workaround-removal-loadays-2018.pdf), [video](https://www.youtube.com/watch?v=OXbbH0ORmSY)
 * RIPE 76: [prezentace](https://ripe76.ripe.net/presentations/159-edns.pdf), [video](https://ripe76.ripe.net/archives/video/161)

Nástroje
========

 * [ISC EDNS Compliance tester](https://ednscomp.isc.org/), [source code](https://gitlab.isc.org/isc-projects/DNS-Compliance-Testing)
 * [DNSViz](http://dnsviz.net/)

Kontakty
========

 * Připomínky k tomuto webu prosím směřujte do [dnsflagday repozitáře](https://github.com/dns-violations/dnsflagday/issues) na Githubu
 * Komentáře k výsledkům testů z toho webu nebo přímo [nástroje ednscomp](https://ednscomp.isc.org/ednscomp) prosím hlaste do [DNS Compliance Testing](https://gitlab.isc.org/isc-projects/DNS-Compliance-Testing) projektu v Gitlabu ISC

<a name="supporters"></a>
Akci podporují
==============
{% include supporters.html %}

Literatura
==========
 * [Minimal EDNS compliance requirements](https://datatracker.ietf.org/doc/draft-spacek-edns-camel-diet/)
 * [“The DNS Camel”, or, the rise in DNS complexity](https://blog.powerdns.com/2018/03/22/the-dns-camel-or-the-rise-in-dns-complexit/)
