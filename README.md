# SQL_project_data_academy_13_09_2023
Analysis of basic foodstuffs availability

Název SQL projektu: Dostupnost základních potravin široké veřejnosti
Vypracovala: Rita Suchá
Datum odevzdání: 8. 11. 2023

Zadání: Na analytickém oddělení naší nezávislé společnosti, která se zabývá životní úrovní občanů byly definovány výzkumné otázky
adresující dostupnost základních potravin široké veřejnosti. Tiskové oddělení bude odpovědi na tyto otázky prezentovat na konferenci
a cílem tohoto projektu je příprava datových podkladů pro prezentaci.

Specifické cíle: Hlavním cílem projektu je porovnání dostupnosti potravin v ČR na základě příjmů za definované časové období.
Dále bude nezbytné porovnat údaje z ČR s údaji z dalších evropských států se zaměřením na HDP a GINI koeficient.

Výzkumné otázky
VO1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?
VO2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?
VO3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?
VO4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?
VO5. Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce,
projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?

Primární tabulky:
1. czechia_payroll – Informace o mzdách v různých odvětvích za několikaleté období. Datová sada pochází z Portálu otevřených dat ČR.
2. czechia_payroll_calculation – Číselník kalkulací v tabulce mezd.
3. czechia_payroll_industry_branch – Číselník odvětví v tabulce mezd.
4. czechia_payroll_unit – Číselník jednotek hodnot v tabulce mezd.
5. czechia_payroll_value_type – Číselník typů hodnot v tabulce mezd.
6. czechia_price – Informace o cenách vybraných potravin za několikaleté období. Datová sada pochází z Portálu otevřených dat ČR.
7. czechia_price_category – Číselník kategorií potravin, které se vyskytují v našem přehledu.

Číselníky sdílených informací o ČR:
1. czechia_region – Číselník krajů České republiky dle normy CZ-NUTS 2.
2. czechia_district – Číselník okresů České republiky dle normy LAU.

Dodatečné tabulky:
1. countries - Všemožné informace o zemích na světě, například hlavní město, měna, národní jídlo nebo průměrná výška populace.
2. economies - HDP, GINI, daňová zátěž, atd. pro daný stát a rok.

Úvaha:
Pro zodopvězení výzkumných otázek vytvořím tabulku obsahující vybrané záznamy. V dostupných tabulkách potřebuju najít:
- mzdy ve všech odvětvích za jednotlivé roky (VO1, VO2, VO5)
- ceny mléka chleba v jednotlivých letech (VO2)
- ceny všech potravin za jednotlivé roky dle kategorie (VO3, VO4, VO5)
- HDP za jednotlivé roky (VO5)

V tabulce czechia_payroll se nachází 6,880 záznamů.
Datová sada obsahuje časovou řadu počtu zaměstnanců a průměrných měsíčních mezd (fyzické i přepočtené osoby) podle odvětví v období 2000-2021.
- zajímají mně sloupce:
id (klíčové hodnoty, unikátní bez duplicit)
value (proměnná)
industry_branch_code (kategorie)
payroll_year (období)
- zajímají mě záznamy s:
value_type_code = 5,958 a unit_code = 200 (průměrná hrubá mzda na zaměstnance v Kč, celkem 3,440 záznamů)
calculation_code = 200 (příjem přepočtený na plný pracovní úvazek, předpokládám zapojení zaměstnance na více pracovištích s podobnou výší mzdy, celkem 3440 záznamů)
- nezajímají mě záznamy s:
value = NULL (celkem 3,096 záznamů)
value_type_code = 316 a unit_code = 80,403 (průměrný počet zaměstnaných osob v tis. osob, celkem 3,440 záznamů)
calculation_code = 100 (fyzický příjem dle výše pracovního úvazku)
Výběr obsahuje 4 sloupce a celkem 1720 unikátních záznamů průměrné hrubé mzdy na zaměstnance v Kč v odvětvích A-S v letech 2000-2021.

Dle ERD je tabulka czechia_payroll napojena přes sloupec 'code' na číselníky czechia_payroll_calculation,
czechia_payroll_industry_branch, czechia_payroll_unit a czechia_payroll_value_type.

V tabulce czechia_payroll_calculation se nachází dva záznamy použité jako hodnoty v tabulce czechia_payroll ve sloupci calculation_code.
Rozlišuje číslem 100 nebo 200 (code - klíčová hodnota), zda se jedná o příjem fyzický nebo přepočtený podle výše pracovního úvazku.

V tabulce czechia_payroll_industry_branch jsou vypsaná všechna odvětví A-S (code - klíčová hodnota) použitá v tabulce czechia_payroll
ve sloupci industry_branch_code.

V tabulce czechia_payroll_unit se nachází dva záznamy použité jako hodnoty v tabulce czechia_payroll ve sloupci unit_code.
Rozlišuje číslem 200 nebo 80,403 (code - klíčová hodnota), zda se jedná o Kč (odkazuje na průměrnou hrubou mzdu na zaměstnance)
nebo tis. osob (odkazuje na průměrný počet zaměstnaných osob).

V tabulce czechia_payroll_value_type se nachází dva záznamy použité jako hodnoty v tabulce czechia_payroll ve sloupci value_type_code.
Rozlišuje číslem 316 nebo 5,958 (code - klíčová hodnota), zda se jedná o průměrnou hrubou mzdu na zaměstnance nebo průměrný počet zaměstnaných osob.

V tabulce czechia_price se nachází 108,249 záznamů.
Datová sada obsahuje časovou řadu statistických údajů o průměrných spotřebitelských cenách vybraného potravinářského zboží za Českou republiku a kraje zjišťované v období 2006-2018.
- zajímají mě sloupce
id (klíčové hodnoty, unikátní bez duplicit)
value (proměnná)
category_code (kategorie)
date_from (období)
- zajímají mě záznamy:
pro všechny kategorie
za celé období
- nezajímají mě záznamy s:
value = NULL (0 záznamů)
Výběr obsahuje 4 sloupce a celkem 108,249 unikátních záznamů cen potravin ve 27 kategoriích sledovaných v letech 2006-2018.

Dle ERD je tabulka czechia_price napojena přes sloupec 'code' na číselníky czechia_price_category a czechia_region.

V tabulce czechia_price_category jsou vypsané všechny kategorie potrevin (code - klíčová hodnota) použité v tabulce czechia_price
ve sloupci category_code, společně se sloupci price_value a price_unit.

Číselníky czechia_region a czechia_district pro zodpovězení výzkumných otázek nepotřebuji.

Dodatečné tabulky countries a economies jsou potřebné pro sekundární tabulku k porovnání údajů z ČR s údaji z dalších evropských států se zaměřením na HDP a GINI koeficient.

V tabulce countries jsou informace o 244 zemích na světě.
- zajímají mě sloupce:
country
contitent
- zajímají mě záznamy:
continent = Evropa

V tabulce economies jsou ekonomické ukazatele pro 266 zemí světa a vybraných celků.
- zajímají mě sloupce:
country
year
GDP
gini
populaton
- zajímají mě záznamy pro:
evropské státy


Postup:

Primární tabulka vyžaduje spojení vybraných dat z tabulek czechia_price, czechia_price_category, czechia_payroll a czechia_payroll_industry_branch.

Z tabulky czechia_price obsahující data pro 27 cenových kategorií v období 2006-2018 vyberu záznamy pro 3 sloupce:
průměr z value
category_code
rok z date_from

K této tabulce připojím jméno kategorie z tabulky czechia_price_category.

Z tabulky czechia_payroll obsahující průměrné měsíční mzdy v období 2000-2021 pro 21 odvětví vyberu záznamy pro 3 sloupce:
průměr z value
industry_branch_code
payroll_year
Vyfitruji záznamy s:
value_type_code = 5,958
unit_code = 200
calculation_code = 200
value bez NULL hodnot

K této tabulce připojím jméno odvětví z tabulky czechia_payroll_industry_branch bez záznamů s NULL hodnotami pro branch.

Upravenou tabulku czechia_price doplněnou o data z czechia_price_category spojím s upravenou tabulkou czechia_payroll doplněnou o data z czechia_payroll_industry_branch.
Výsledná tabulka obsahuje 6,498 záznamů pro 6 sloupců.


Sekundární tabulka k porovnání údajů z ČR s údaji z dalších evropských států vyžaduje spojení vybraných dat z tabulek countries a economies.

Z tabulky countries vyberu sloupec 'country' a záznamy zfiltruji pro kontinent Evropa:

Z tabulky economies vyberu 4 sloupce:
country
GDP
gini
population
year

K upravené tabulce countries připojím upravenou tabulku economies.
Výsledná tabulka obsahuje 2,748 záznamů pro 5 sloupců.


Tabulky:

1. t_rita_sucha_project_SQL_primary_final - data mezd a cen potravin za ČR sjednocená na společné roky
- tabulka eviduje 6,498 záznamů pro 6 sloupců:
price_year = rok extrahovaný ze slouce date_from z tabulky czechia_price
category = jméno kategorie z tabulky czechia_price_category
price_average = průměr cen z tabulky czechia_price
payroll_year = rok z tabulky czechia_payroll
branch = jméno odvětví z tabulky czechia_payroll_industry_branch
payroll_average = průměr průměrných hrubých mezd z tabulky czechia_payroll
- data z tabulky czechia jsou extrahovaná z kompletních záznamů, u kterých je známé odvětví
- data z tabulky czechia_payroll jsou filtrovaná na přepočtenou průměrnou hrubou mzdu na zaměstnance v Kč

2. t_rita_sucha_project_SQL_secondary_final - dodatečná data o dalších evropských státech
- tabulka eviduje 2,748 záznamů pro 5 sloupců:
country = země z tabulky countries
population = počet obyvatel z tabulky economies
GDP = HDP z tabulky economies
gini = gini koeficient z tabulky economies
year = rok z tabulky economies
- vybraná data z tabulek countries a economies jsou filtrovaná na evropské země


SQL dotazy:

1. t_rita_sucha_project_SQL_question_1 - trend vývoje mezd ve všech odvětvích

2. q2_rita_sucha_project_SQL_question_2 - dostupnost mléka a chleba za porovnatelné období

3. q3_rita_sucha_project_SQL_question_3 - meziroční růst cen potravin dle kategorie

4. q4_rita_sucha_project_SQL_question_4 - meziroční růst cen potravin v závislosti na růstu mezd

5. q5_rita_sucha_project_SQL_question_5 - vliv HDP na vývoj mezd a cen potravin


Odpovědi na výzkumné otázky:

1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?
Vzestupný trend vývoje mezd v ČR lze až na nepatrné výkyvy pozorovat ve všech odvětvích.

2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?
V prvním srovnatelném období (r. 2006) bylo možné si koupit z tehdejší mzdy celkem 1,312 kilogramů chleba, resp. 1,465 litrů mléka.
V posledním srovnatelné období (r. 2018) bylo možné si koupit z tehdejší mzdy celkem 1,365 kilogramů chleba, resp. 1,669 litrů mléka.

3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?
V průměru zdražovali nejpomaleji banány, které od r. 2016 dokonce zlevňují.
V průměru docházelo ke zlevnění u cukru a rajčat, jejichž cena ale velmi kolísá mezi sledovanými roky.

4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?
K výraznému růstu mezd a cen potravin došlo v letech 2007, 2008 a 2018. Tento růst byl v letech 2007 a 2018 doprovázen růstem cen potravin.
V roce 2007 byl růst cen výrazně vyšší (13.87 %) než růst mezd (9.24 %).

5. Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách
potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?
Při výrazném růstu HDP nad 5 % nedochází k výraznému nárůstu cen ve stejném ani následujícím roce, ale dochází k výraznému nárůstu mezd.
Souvislost mezi výrazným nárůstem mezd a HDP by bylo možné určit z dat, která nejsou součástí tohoto projektu.
Dodatečná data by mohla ukázat, zda dochází k nárůstu HDP vlivem nárůstu mezd nebo naopak.
