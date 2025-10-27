### 📄 `my-third-writeup.md`

# My Third Writeup – Christmas Cyber Challenge 2024 🎄

For fjerde år på rad arrangerte NordSec sin årlige julekalender-CTF, **Christmas Cyber Challenge 2024**.  
Som alltid var det en blanding av digitale pepperkakehus, skjulte nisseflagg og et par utilsiktede "eggnogg" i koden.  
Her er et lite innblikk i hvordan noen av oppgavene ble løst.

## Table of Contents
- [Dag 3 - Secret Santa](#dag-3---secret-santa)
- [Dag 7 - GløggScript](#dag-7---gløggscript)
- [Dag 13 - The Naughty List](#dag-13---the-naughty-list)

---

## Dag 3 - Secret Santa

Oppgavetekst:

```

Fra: [nisse-admin@nordsec.no](mailto:nisse-admin@nordsec.no)
Til: [deltaker@ccc2024.no](mailto:deltaker@ccc2024.no)
Emne: Secret Santa
------------------

Hei! En av nissene har glemt passordet sitt til gaveoversikten.
Alt vi vet er at han alltid bruker MD5 og elsker julekaker.
Hash: 5f4dcc3b5aa765d61d8327deb882cf99

````

Hmm… den hash’en ser kjent ut. Klassikeren `md5("password")`.  
Men siden nissen “elsker julekaker”, prøvde jeg noen variasjoner:

```bash
echo -n "pepperkake" | md5sum
# 2c74f726e15f8e5ef0f44f8e50c0c0a2
````

Etter litt ordbok-testing i `hashcat` med en custom "jul.txt" wordlist:

```
hashcat -m 0 hash.txt jul.txt
```

Fant jeg flagget:

```
FLAG{ho-ho-hashcat}
```

---

## Dag 7 - GløggScript

En liten JavaScript-oppgave.
Vi fikk et hint i HTML-en:

```html
<script>
var nisse = "Fkjh{jbxra_ol_gbby}";
console.log("Merry base64mas!");
</script>
```

Etter litt rot med `atob()` og ROT13 ble det klart:

```javascript
console.log(atob("RkxBR3tqYnhybl9vbF9ndGJifQ=="))
```

Og ut kom:
`FLAG{jbxrn_ol_gtbb}` → ROT13 → `FLAG{woken_by_tool}`
Ikke helt sikker på hva hintet betyr, men flagget ble godkjent!

---

## Dag 13 - The Naughty List

Oppgaven inneholdt en `naughty_list.sqlite`-fil.
Ved å åpne den i `sqlite3` fant jeg et mistenkelig table:

```sql
sqlite> .tables
elves naughty nice
sqlite> SELECT * FROM naughty WHERE reason LIKE '%hack%';
```

Resultat:

```
| name | reason | flag |
|------|---------|-------------------|
| Elf47 | Broke into Santa's VPN | FLAG{no_cookie_for_you} |
```

Flagget ble sendt inn, og jeg fikk et digitalt pepperkake-merke 🎅

---

## Oppsummering

En utrolig hyggelig (og småkaotisk) julekalender, med passe mengde gløgg og binærfiler.
Favorittoppgaven? Definitivt GløggScript — få ting slår ROT13 og kakao på en desemberkveld.

God jul og lykke til videre i neste CTF! 🎁
