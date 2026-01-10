
# Volitve iz domačega kavča 
## Kako Estonija dokazuje, da je to varno (in kaj se lahko nauči Slovenija)
![](https://www2.arnes.si/~egrmad/gibanje.eu/slovenija/e-estonia.png )
**Avtor:** [edvard g.]

**Čas branja:** 4 min

**Oznake:** #Tehnologija #Družba #Estonija #Volitve #Blockchain

----------

Ste vedeli, da v Estoniji že 20 let volijo kar od doma? Leta 2023 je prvič v zgodovini več kot 50 % vseh estonskih volivcev svoj glas oddalo preko spleta.

Medtem ko v večini držav (vključno s Slovenijo) na volilno nedeljo še vedno romamo na volišča in obkrožamo papirnate lističe, se Estonci sprašujejo: _"Zakaj bi izgubljal čas, če lahko to uredim v pižami?"_

A glavno vprašanje, ki se poraja nam, skeptikom, je: **Je to varno?** Kako vemo, da hekerji ne priredijo rezultatov? Kako preprečiti, da bi nekdo glasoval v mojem imenu?

Poglejmo v "motor" estonskega sistema, ki velja za zlati standard digitalne demokracije.

### 1. Temelj vsega: Digitalna identiteta

Brez trdne identifikacije ni e-volitev. Estonci ne uporabljajo uporabniških imen in gesel, ki jih je lahko ukrasti. Za dostop do volitev uporabljajo svojo **osebno izkaznico s čipom** ali varen sistem na mobilnem telefonu (Mobile-ID).

Sistem mora 100-odstotno vedeti, da za računalnikom sedite res vi. To omogoča tehnologija **digitalnega podpisa***, ki ima enako pravno veljavo kot lastnoročni podpis na upravni enoti.

### 2. Sistem "Dvojne kuverte" (Kako zagotoviti tajnost?)

Kako zagotoviti, da sistem ve, da ste volili (da ne morete voliti dvakrat), hkrati pa ne ve, _koga_ ste volili (tajnost glasovanja)?

Estonci so preslikali sistem glasovanja po pošti v digitalni svet. Uporabljajo sistem dveh ovojnic:

-   **Notranja ovojnica (Digitalni glas):** Ko izberete kandidata, se vaš glas šifrira (zaklene). Nihče – niti vi, niti sistemski administrator – ga v tistem trenutku ne more prebrati.
    
-   **Zunanja ovojnica (Digitalni podpis):** Šifriranemu glasu se doda vaš digitalni podpis. Ta služi kot dokaz: _"Jaz, Janez Novak, sem oddal to kuverto."_
    

### 3. Digitalni mešalec (Anonimizacija)

Ko se volitve zaključijo, se zgodi ključni trenutek ločitve. Sistem najprej preveri zunanje ovojnice (podpise). Če ste upravičenec, vaš glas sprejme.

Nato se zgodi **ločitev**: Sistem "odtrga" zunanjo kuverto z vašim imenom in jo uniči. Ostane le šifrirana notranja kuverta (glas), ki nima več nobene povezave z vami. Šele ko so vsi glasovi anonimni, se odklenejo in preštejejo.

### 4. Kaj pa prisila? (Scenarij "Pištola na glavi")

Največji ugovor proti e-volitvam je možnost prisile. Kaj, če vam nasilni partner, šef ali podkupovalec stoji za hrbtom in gleda, koga boste kliknili?

Estonija ima genialno preprosto rešitev: **Glasuješ lahko večkrat.**

V času predčasnega elektronskega glasovanja lahko svoj glas spremenite tolikokrat, kot želite. Sistem upošteva samo **zadnji oddani glas**.

-   _Primer:_ Če vas nekdo prisili, da glasujete za Stranko A, to storite. Ko ste kasneje sami in varni, se ponovno prijavite in glasujete za Stranko B.
    
-   Še večja varovalka: Če greste na volilno nedeljo fizično na volišče in oddate papirnati listič, ta **izbriše vse vaše prejšnje elektronske glasove**. Papir ima vedno prednost.
    

### 5. Zaupaj, a preveri (Skeniraj kodo)

Kaj pa, če imate na računalniku virus, ki na skrivaj spremeni vaš glas?

Po oddaji glasu se na ekranu prikaže **QR koda***. Volivec vzame svoj pametni telefon (drugo, neodvisno napravo), skenira kodo in preveri zapis. Telefon mu pove: _"Vaš glas je bil varno prejet za kandidata X."_ Če se podatek ne ujema s tistim, kar ste kliknili, lahko takoj sprožite alarm.

### 6. Vloga Blockchaina: Nepodkupljivi notar

Veliko se govori o **blockchainu*** ali verigi blokov. Ali so glasovi shranjeni tam? Ne, saj so javni blockchaini (kot Bitcoin) transparentni, volitve pa morajo biti tajne.

Estonija uporablja blockchain tehnologijo (KSI) kot **revizijsko sled**. Vsak dogodek v sistemu – prijava administratorja, prenos datoteke, začetek štetja – se zapiše v verigo kot **log*** (dnevniški zapis).

> **Bistvo:** To preprečuje, da bi "glavni računalnikar" ali heker neopazno izbrisal sledi svojega vdora ali dodal 10.000 lažnih glasov. Če bi spremenil en sam bit podatkov, se matematika verige blokov ne bi izšla in sistem bi zaznal napako.

----------

### Kje je Slovenija?

Tehnično gledano ima Slovenija že vse potrebne gradnike. Imamo nove biometrične osebne izkaznice, sistem SI-PASS in kvalificirane digitalne podpise. Estonski primer nas uči, da glavna ovira ni tehnologija, temveč **zaupanje javnosti** in politična volja za spremembo zakonodaje.

Bi vi zaupali takemu sistemu, ali pa je občutek svinčnika na papirju tisti pravi branik demokracije?

----------

### 📚 Mali slovarček uporabljenih izrazov

Za lažje razumevanje tehnologije v ozadju:

-   **Digitalni podpis:** Elektronski nadomestek lastnoročnega podpisa. S pomočjo zapletene matematike (kriptografije) jamči, da ste dokument ali glas oddali res vi in da ga nihče med potjo do strežnika ni spremenil.
    
-   **QR koda:** Dvodimenzionalna črtna koda (kvadratek s pikami), ki jo kamera pametnega telefona hitro prebere in vas usmeri na spletno stran ali prikaže skrite podatke.
    
-   **Blockchain (Veriga blokov):** Posebna vrsta baze podatkov, ki deluje kot digitalna glavna knjiga. Ko je podatek enkrat zapisan v blok, ga je praktično nemogoče spremeniti ali izbrisati za nazaj, saj je povezan z vsemi prejšnjimi zapisi.
    
-   **Logi (Sistemski dnevniki):** Avtomatski zapisi o vsem, kar se dogaja v računalniškem sistemu (kdo se je prijavil, kdaj, kaj je kliknil). V varnih sistemih so ti zapisi ključni za iskanje napak ali zlorab.

