# SeedSigner Crosscheck – Last Word (v1.1.4)

Een offline HTML-tool om het **laatste woord** (woord 12 of 24) van je seed phrase onafhankelijk te berekenen en te vergelijken met wat je SeedSigner toont. Hiermee kun je de SeedSigner-software op een tweede apparaat crosschecken.

Zie ook de volledige crosscheck-tool voor single-sig/multisig, fingerprints en xpubs: [wout2121/seedsigner-crosscheck](https://github.com/wout2121/seedsigner-crosscheck).

## Waarom deze tool?

Ik heb de code van @M21 geforkt en aangepast zodat je de SeedSigner-software **gemakkelijker kunt crosschecken**.

> **Cruciaal:** download de SeedSigner-software op een **ander apparaat** dan het apparaat dat je voor deze crosscheck gebruikt.

**"Waarom is dit nog nodig als de GPG-keys al geverifieerd zijn?"**
Een geldige GPG-handtekening bewijst alleen dat het bestand dat je downloadde echt van de ontwikkelaars komt. Ze beschermt je niet tegen het apparaat waarop je dat doet. Staat er malware op je computer, dan kan die nog steeds heel wat manipuleren, zoals:

- de **12 (of 24) woorden** van je seed phrase;
- de **private keys**;
- de **xpubs**;
- de **ontvangstadressen**.

Door de SeedSigner-software op het ene apparaat te downloaden en de resultaten op een **tweede, onafhankelijk apparaat** met deze tool te controleren, moet een aanvaller beide apparaten tegelijk gecompromitteerd hebben om je te misleiden.

## Wat doet de laatste-woord-calculator?

Het laatste woord van een BIP39-seed bevat deels willekeurige bits en deels een **checksum**. Deze tool zet de vrije bits vast op **0**:

| Seedlengte | Vrije bits in laatste woord | Checksum-bits | Effectieve entropie |
|---|---|---|---|
| 12 woorden | 7 → vastgezet op 0 | 4 | 121 bits |
| 24 woorden | 3 → vastgezet op 0 | 8 | 253 bits |

Je typt de eerste 11 (of 23) woorden en de tool geeft **precies één** geldig laatste woord terug, met het nummer uit de BIP39-woordenlijst. Omdat de vrije bits op 0 staan, is dat woord volledig bepaald door de woorden die je al typte. Zo kun je exact controleren of je SeedSigner hetzelfde laatste woord berekent (optie met nullen bij het berekenen van het laatste woord).

Daarnaast bevat de tool ook de hex-/dobbelsteen-entropie zoals SeedSigner (50 tekens → 12 woorden, 99 tekens → 24 woorden, via SHA-256).

## Wat zit er in deze repository?

| Bestand | Uitleg |
|---|---|
| `seed-hex-dice-v1.1.4-last-word-0-bits.html` | De tool zelf: één zelfstandig HTML-bestand, zonder netwerkverkeer en zonder opslag (strikte Content-Security-Policy). |
| `seed-hex-dice-v1.1.4-last-word-0-bits.html.sig` | De GPG-handtekening van het HTML-bestand. Beide bestanden horen samen. |

## Downloaden en verifiëren

1. Download **beide** bestanden (`.html` en `.html.sig`) via de groene knop **Code → Download ZIP**, of klik op elk bestand en kies **Download raw file**.
2. Controleer de GPG-handtekening:

   ```bash
   gpg --verify seed-hex-dice-v1.1.4-last-word-0-bits.html.sig seed-hex-dice-v1.1.4-last-word-0-bits.html
   ```

   De handtekening is gemaakt met de sleutel met fingerprint:

   ```
   CE1F B111 C2B5 FECC 91C3  6F11 C464 F440 9ED9 FEF2
   ```

   Je moet deze publieke sleutel eerst importeren (`gpg --import`) en de fingerprint via een onafhankelijk kanaal controleren.

3. Optioneel: controleer de SHA-256-hash:

   ```bash
   sha256sum seed-hex-dice-v1.1.4-last-word-0-bits.html
   # 96c553689b0291b79b5516d4507a1b1d99e2c024f345c104b6a0a43d6c583aec
   ```

## Veilig gebruiken

1. Download de SeedSigner-software op **apparaat A**.
2. Download en verifieer deze tool op **apparaat B** (een ander apparaat).
3. Koppel apparaat B **los van het internet** voordat je het HTML-bestand opent.
4. Typ de eerste 11 (of 23) woorden en vergelijk het berekende laatste woord met wat je SeedSigner toont.
5. Sluit het tabblad na gebruik en gebruik voor echte fondsen bij voorkeur een apparaat dat nooit meer online komt (of een live-USB).

## Disclaimer

Gebruik op eigen risico. Deze tool is bedoeld als extra controle, niet als vervanging van goede operationele beveiliging. Omdat de vrije bits op 0 staan, heeft je seed iets minder entropie (121 i.p.v. 128 bits, of 253 i.p.v. 256 bits); dat is nog steeds ruim voldoende, maar alleen zolang de eerste woorden echt willekeurig gekozen zijn. Test altijd eerst met een testseed voordat je er echte fondsen aan toevertrouwt.
