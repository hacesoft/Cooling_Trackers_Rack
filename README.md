# Cooling_Trackers_Rack
**Inteligentní chlazení MPPT trackerů a měničů v racku**

Plugin pro rozšíření projektu **[Linea](https://github.com/hacesoft/Linea)** - automatické řízení ventilátorů na základě dat z Victron Energy systému.

<img width="1687" height="595" alt="image" src="https://github.com/user-attachments/assets/f2125a2d-28aa-4d88-beaa-d31cb1ac17ea" />

---

## 📋 O projektu

Tento Node-RED flow automaticky řídí chlazení MPPT trackerů a měničů Victron Energy podle aktuálního výkonu a směru toku energie. Ventilátory jsou ovládány přes WiFi zásuvky **Shelly Plug**.

⚠️ **Tento plugin není samostatně funkční!** Vyžaduje nainstalovaný a nakonfigurovaný základní projekt **[Linea](https://github.com/hacesoft/Linea)**, který zajišťuje globální funkce a komunikaci s Victron systémem.

---

## ✨ Funkce

### Chlazení měničů (CHANGER)
- ✅ Zapne se při vybíjení baterie nad -600W
- ✅ Zapne se když MPPT produkuje ale nejde do baterie (dodávka do gridu)
- ✅ Vypne se když MPPT nabíjí baterii (žádná zátěž měničů)

### Chlazení MPPT trackerů
- ✅ Zapne se při produkci nad 900W
- ✅ Vypne se při nízké produkci

### Ochrana a stabilita
- ✅ **Hystereze** - 3x požadavek na zapnutí, 20x na vypnutí (ochrana proti fluktuaci)
- ✅ **Vizuální stavy** - barevné indikátory pod každým nodem (🟢 zapnuto / ⚪ vypnuto)
- ✅ **Monitorování** - kontrola fyzického stavu ventilátorů
- ✅ **General STOP** - nouzové vypnutí všech ventilátorů
- ✅ **Debug režim** - podrobné výpisy pro diagnostiku

---

## 📦 Požadavky

### Základní projekt
- **[Linea](https://github.com/hacesoft/Linea)** - hlavní řídící systém FVE (povinné!)

### Hardware
- **Victron Energy systém:**
  - MPPT solární regulátor s Modbus TCP/IP
  - Bateriový systém s monitorováním výkonu
- **2x Shelly Plug** (nebo kompatibilní WiFi zásuvky s HTTP API)
- **2x Ventilátor** (pro měniče a MPPT trackery)

### Software
- Node-RED (verze 3.0+)
- `node-red-contrib-modbus` (pro komunikaci s Victron)

---

## 🚀 Instalace

### 1. Nainstalujte základní projekt Linea
Nejprve musíte mít funkční projekt Linea:
```
https://github.com/hacesoft/Linea
```

### 2. Import pluginu do Node-RED
1. Otevřete Node-RED editor
2. Menu → Import → Clipboard
3. Vložte obsah souboru `flow.json`
4. Klikněte na "Import"

### 3. Konfigurace

**Nastavte IP adresy Shelly Plug zásuvek:**
```javascript
// V kódu CHANGER
const sIpAddress = "192.168.11.9";  // Ventilátor pro měniče

// V kódu MPPT
const sIpAddress = "192.168.11.7";  // Ventilátor pro MPPT trackery
```

**Upravte triggery (volitelně):**
```javascript
// Chlazení měničů
let n_trigger_Changer = 600;  // Trigger pro vybíjení baterie [W]
let n_trigger_MPPT = 600;     // Trigger pro produkci MPPT [W]

// Chlazení MPPT trackerů
let n_trigger_MPPT = 900;     // Trigger pro zapnutí chlazení [W]
```

---

## 🧠 Logika řízení

### Chlazení měničů (CHANGER) - kdy se ZAPNE:
1. **Baterie se vybíjí** > 600W → Měnič aktivní, potřebuje chlazení
2. **MPPT produkuje** > 600W **A baterie se nenabíjí dostatečně** → Energie jde do gridu přes měnič

### Chlazení měničů - kdy se VYPNE:
- MPPT nabíjí baterii (energie jde přímo do baterie, měnič neaktivní)
- Nízká produkce nebo malé vybíjení (pod triggerem)

### Chlazení MPPT trackerů:
- **Zapne se:** MPPT výkon ≥ 900W
- **Vypne se:** MPPT výkon < 900W

### Hystereze (ochrana proti fluktuaci):
- **Zapnutí:** Musí přijít 3x po sobě požadavek na zapnutí
- **Vypnutí:** Musí přijít 20x po sobě požadavek na vypnutí
- Pokud mezitím přijde opačný požadavek, čítač se vynuluje

---

## 📊 Monitorování

Pod každým nodem v Node-RED uvidíte aktuální stav:

**Příklady stavů:**
```
🟢 ZAPNUTO - Baterie vybíjí: -750W
🟢 ZAPNUTO - MPPT: 1500W, Bat: 200W
⚪ VYPNUTO - MPPT nabíjí bat (M:1500W, B:+800W)
⚪ VYPNUTO - Nízká produkce (M:200W, B:100W)
⚪ Čeká na zapnutí (2/3) - vypnuto
🟢 MPTT | 🟢 CHANGER  (fyzický stav ventilátorů)
```

**Debug režim:**
Zapněte debug výpisy v kódu:
```javascript
const debug = true;  // true = zapnuto, false = vypnuto
```

---

## 🔧 Řešení problémů

### Ventilátory se nezapínají
1. Zkontrolujte IP adresy Shelly Plug zásuvek
2. Ověřte síťovou konektivitu (ping na IP)
3. Zkontrolujte, zda je nainstalován projekt Linea
4. Zapněte debug režim a sledujte konzoli

### Ventilátory se přepínají příliš často
Zvyšte zpoždění hystereze:
```javascript
const nWaitStartLoop = 5;  // Počet cyklů před zapnutím (výchozí 3)
const nWaitLoop = 30;      // Počet cyklů před vypnutím (výchozí 20)
```

### Červený status "Chyba: Globální funkce nenalezeny"
- Projekt Linea není nainstalován nebo není správně nakonfigurován
- Řešení: Nainstalujte https://github.com/hacesoft/Linea

---

## 📝 Závislosti na projektu Linea

Tento plugin vyžaduje z projektu Linea:

**Globální funkce:**
- `fSendUrl(ipAddress, relayNumber, turn)` - generování URL pro Shelly Plug
- `nConvertSignetUnsignet()` - konverze signed/unsigned hodnot z Modbus

**Globální proměnné:**
- `pvPower` - aktuální výkon MPPT [W]
- `nBattery_Power` - aktuální výkon baterie [W] (záporné = vybíjení, kladné = nabíjení)

---

## 📅 Changelog

**Verze 1.9** (14.12.2025)
- ✅ Přidána vizualizace stavů pod všemi nody
- ✅ Implementována hystereze pro stabilní chod
- ✅ Vylepšený debug režim
- ✅ Přidána kontrola fyzického stavu ventilátorů
- ✅ Kompletní dokumentace

---

## 👨‍💻 Autor

**hacesoft** - http://www.hacesoft.cz

---

## 📄 Licence

This code is distributed under the GNU Public License.
Všechny informace jsou zahrnuty pod GPL licenci, pokud není explicitně uveden jiný typ licence.

---

## ⚠️ Zřeknutí se odpovědnosti

Autor tohoto projektu neposkytuje žádné záruky, výslovné ani implicitní, ohledně správnosti, spolehlivosti, funkčnosti nebo vhodnosti k jakémukoli účelu. Veškeré použití tohoto softwaru, kódu, schémat, návodů, technických řešení, produktů a jakýchkoli dalších poskytnutých materiálů je na vlastní odpovědnost uživatele.

Autor nenese žádnou odpovědnost za jakékoli škody, ztráty, finanční náklady, přímé či nepřímé škody vzniklé v důsledku použití těchto materiálů, a to včetně, ale nejen, ztráty dat, poškození zařízení, výpadků systému, poruchy elektrických či jiných instalací, požárů, ztrát příjmů nebo jiných nepředvídatelných následků.

Uživatel bere na vědomí, že jakékoli úpravy, sestavování, instalace, zapojení či implementace na základě poskytnutých informací provádí výhradně na vlastní riziko. Autor neposkytuje žádné garance funkčnosti, bezpečnosti ani souladu s platnými právními normami a předpisy.

Uživatel se zavazuje, že nevyužije žádné právní kroky vůči autorovi v souvislosti s jakýmikoli škodami nebo jinými nároky vyplývajícími z používání tohoto softwaru, produktů, schémat nebo návodů. Jakékoli právní nároky vůči autorovi jsou tímto výslovně vyloučeny a nevymahatelné, a to i soudní cestou.

**Použitím těchto materiálů uživatel potvrzuje svůj souhlas s výše uvedenými podmínkami.** Pokud s nimi nesouhlasíte, nepoužívejte tento software, schémata, návody ani jiné poskytnuté materiály.

---

## 🔗 Související projekty

- **[Linea](https://github.com/hacesoft/Linea)** - Hlavní řídící systém FVE Victron (povinný!)
