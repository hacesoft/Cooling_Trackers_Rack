# Cooling_Trackers_Rack
Cooling of Trackers in the Rack

Tento FLOW není samostatně funkční, chybí mu globální funkce a nastavení, které jsou součástí: https://github.com/hacesoft/Linea/tree/main?tab=readme-ov-file#linea

![image](https://github.com/user-attachments/assets/0e920405-32e4-4dee-8a06-68bc3f3edf43)

**Popis funkce:**

Funkce je implementována v prostředí Node-RED a slouží k ovládání ventilátorů připojených přes Shelly Plug. Na základě údajů z MPPT trackeru Victron se vyčítá aktuální výkon, který se sečte. Podle nastavených triggerů se pak ovládají Shelly Plug zásuvky, které spínají ventilační jednotky. Jedna ventilační jednotka je namířena na měniče Victron a druhá na MPPT tracker.

---

## Pracujte pečlivě! Dvakrát měř, jednou řež...

---

## Zřeknutí se odpovědnosti

Autor tohoto projektu neposkytuje žádné záruky, výslovné ani implicitní, ohledně správnosti, spolehlivosti, funkčnosti nebo vhodnosti k jakémukoli účelu. Veškeré použití tohoto softwaru, kódu, schémat, návodů, technických řešení, produktů a jakýchkoli dalších poskytnutých materiálů je na vlastní odpovědnost uživatele.

Autor nenese žádnou odpovědnost za jakékoli škody, ztráty, finanční náklady, přímé či nepřímé škody vzniklé v důsledku použití těchto materiálů, a to včetně, ale nejen, ztráty dat, poškození zařízení, výpadků systému, poruchy elektrických či jiných instalací, požárů, ztrát příjmů nebo jiných nepředvídatelných následků.

Uživatel bere na vědomí, že jakékoli úpravy, sestavování, instalace, zapojení či implementace na základě poskytnutých informací provádí výhradně na vlastní riziko. Autor neposkytuje žádné garance funkčnosti, bezpečnosti ani souladu s platnými právními normami a předpisy.

Uživatel se zavazuje, že nevyužije žádné právní kroky vůči autorovi v souvislosti s jakýmikoli škodami nebo jinými nároky vyplývajícími z používání tohoto softwaru, produktů, schémat nebo návodů. Jakékoli právní nároky vůči autorovi jsou tímto výslovně vyloučeny a nevymahatelné, a to i soudní cestou.

Použitím těchto materiálů uživatel potvrzuje svůj souhlas s výše uvedenými podmínkami. Pokud s nimi nesouhlasíte, nepoužívejte tento software, schémata, návody ani jiné poskytnuté materiály.

