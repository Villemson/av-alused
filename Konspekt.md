# av-alused

## Sissejuhatus

Läbin kursuse https://www.udacity.com/course/networking-for-web-developers--ud256

Seal olevaid õpetusi katsetan https://cloud.google.com/shell

Enne kasutamist käivitan käsud

`sudo apt-get update`

`sudo apt-get install netcat-openbsd tcpdump traceroute mtr`

## Pingist HTTP-le

Ping on arvutivõrgu võrguühenduse diagnostikaprogramm, mille abil IP-võrgus hinnatakse hosti ligipääsetavust programmi käivitanud arvutist ning mõõdetakse pakettide edastamiseks kuluvat aega (RTT).

Võrguprotokoll määratleb võrguseadmete vahelise suhtlemise eeskirjad ja kokkulepped. Võrguprotokollid sisaldavad mehhanisme, mis võimaldavad seadmeid üksteisega identifitseerida ja omavahel ühendada, samuti vormingu reegleid, mis määravad, kuidas andmed pakitakse, saadetakse ja vastu võetakse. Mõned protokollid toetavad ka sõnumite kinnitamist ja andmete tihendamist, et tagada usaldusväärne ja/või suure jõudlusega võrgusuhtlus. Võrguprotokolli viga võib tekkida näiteks halva internetiühenduse tõttu.

Võrguprotokolle võib üldistatult jaotada:
- Interneti-protokollid (nt IP, TCP, UDP)
- traadita võrgu protokollid
- marsruutimisprotokollid

HTTP (Hypertext Transfer Protocol) ja SSH (Secure Shell) on rakenduskihi protokollid. HTTP on veebis andmete edastamise aluseks.

Interneti klient/server-programmid suhtlevad portide kaudu. Vastavalt protokolli tüübile jagatakse porte TCP- ja UDP-portideks.

Ühes arvutis võib korraga olla käimas mitmeid rakendusi, nii kliente kui ka servereid. Et aru saada, milline pakett millise rakenduse juurde kuulub, korraldatakse sorteerimine portide kaupa. Porte iseloomustatakse numbrite abil (täisarv vahemikus 0 kuni 65535), seejuures numbrid 0 kuni 1023 on reserveeritud kindlate teenuste jaoks (well-known ports). Serveripoolne teenus kasutab tavaliselt kindlat porti, kuid kliendipoolne pordi number määratakse dünaamiliselt, et tagada ühenduse ühene identifitseerimine. Näiteks saavad samade arvutite vahel korraga toimuda mitu ühendust; erinevate ühenduste paketid ei tohi segi minna ning selleks kasutatakse erinevaid pordinumbrite kombinatsioone.

Brauseriga veebilehe avamisel laetakse mitmeid faile korraga. Mida kiiremini need alla laetakse, seda kiiremini avaneb veebileht. Kui 1000 inimest proovib ühest uksest siseneda, siis liikumine aeglustub, kuid kahe ukse korral on läbipääs kiirem. Sarnaselt aitab paralleelsus (mitu ühendust) ressursse kiiremini tuua. Serveri poolel saab ühendusi hallata mitmel moel: luues iga ühenduse jaoks eraldi protsessi/lõime, kasutades protsessi- või thread-pool’i või asünkroonset sündmuspõhist mudelit, kus sama protsess vahetab kiiresti päringute käsitlemist nende vabanemisel.

Käsk `printf` ja `nc` (netcat): sisendi lõpetamiseks kasutatakse Ctrl+D (EOF). Väljundi saab suunata faili, nt `example.txt`.

## Nimed ja aadressid

IP-aadress (Internet Protocol address) on kasutusel selleks, et eristada Internetis erinevaid arvuteid ja seadmeid. Selle tööpõhimõte on sarnane postiaadressile: see näitab, kuhu andmed tuleb kohale toimetada.

DNS (Domain Name System) on internetiteenus, mis teisendab domeeninimed IP-aadressideks ja vastupidi.

Host on arvuti või seade, mis pakub teenuseid teistele seadmetele või tarbib teenuseid võrgus.

Käsk `host eesti.ee` on lihtne tööriist DNS-kirjete otsimiseks. See käsk esitab päringu operatsioonisüsteemi resolver’ile, mis reeglina saadab päringu seadistatud DNS-serverile.

`dig` on sarnane tööriist nagu `host`, kuid tehnilisem ja detailsem. `dig` võimaldab kasutada erinevaid lippe ja parameetreid, mis teeb päringud paindlikuks ning vastused detailsemaks.

DNS kirjeid on mitmeid:
- CNAME – aliasnimi, seab nimele vastavusse teise nime
- AAAA – aadress, seab nimele vastavusse IPv6 aadressi
- NS – tsooni või alamtsooni nimeserver; NS-kirje määrab, milline nimeserver teenindab tsooni. NS-kirje võib olla authority record (kui asub samanimelises tsoonis) või delegation record (kui kirjeldab tütardomeeni delegeerimist).
- A – aadress, seab nimele vastavusse IPv4 aadressi

Domeeninimi internetis peab olema kordumatu. Peamine eesmärk on luua IP-aadressi numbrilise kuju kõrvale inimloetav esitus. Domeeninimi võib koosneda tähtedest, numbritest ja sidekriipsudest.

Veebiserver saab töödelda päringuid erinevatelt veebisaitidelt. HTTP-päringutel on `Host` header, mis näitab, millise domeeni sisu klient soovib.

Host header on vajalik HTTP/1.1 päringute jaoks. Veebiküpsised salvestatakse kindlale domeeninimele.

Kasutusel on kaks IP-aadressi versiooni:
- IPv4 (IP versioon 4), kus aadress koosneb neljast punktiga eraldatud täisarvust vahemikus 0 kuni 255, nt `213.184.53.178` (32-bit)
- IPv6 (IP versioon 6), kus aadress koosneb kaheksast kooloniga eraldatud kuueteistkümnendarvude rühmast, nt `2001:0db8:85a3:0042:1000:8a2e:0370:7334` (128-bit)

## Adresseerimine ja võrgud

Kõiki 32-bitiseid väärtuseid ei kasutata päris aadressidena. Osa on reserveeritud spetsiaalsete protokollide jaoks, sisemisteks privaatvõrkude jaoks, testimiseks või dokumenteerimiseks. Seetõttu ei saa kõiki IPv4 aadresse avalikuks kasutuseks jagada ning aadressiruum on piiratud.

Samas võrguplokis olevad arvutid saavad sarnase võrguosa IP-st ning omavaheliseks suhtluseks ei vaja eraldi ruuterit (suhtlus toimub lokaalses võrgus). Võrgud on erineva suurusega. Lühem võrgu eesliide võimaldab rohkem aadresse hosti jaoks. Võrgu eesliite pikkus valitakse võrku üles seades.

CIDR-notatsioonis märgitakse võrk kujul `/N`. Näiteks `/22` tähendab, et 22 bitti kuulub võrguosale ja ülejäänud 10 bitti hostiosale (32 - 22 = 10).

Võrgu suurust saab väljendada ka alamvõrgumaskiga (subnet mask), mida sageli kasutatakse võrgu seadistamisel. Nagu IPv4 aadressid, on ka alamvõrgumaskid 32-bitised väärtused.

Arvutil on näiteks wifi-, ethernet- ja loopback-liides. Loopback-liidesel on reeglina IP-aadressiks `127.0.0.1` ja nimeks `localhost`. See laseb programmidel kasutada võrguprotokolli virna, et suhelda sama hosti teiste programmidega.

Linuxis saab liidesed leida käsuga `ip addr show`.

Koduruuteril on kohalikud liidesed (ethernet, wifi) ja üks liides, mis on ühendatud internetiteenuse pakkujaga. Välisvõrk (WAN) on liides, mis ühendub ülejäänud internetiga. Kohalik võrk (LAN) ühendab koduarvuti ja muud seadmed ruuteriga.

## Protokollikihid

Võrguprotokollid koosnevad mitmest kihist. Käsk `tcpdump` võimaldab näha võrguliiklust hosti ja võrgu vahel.

Näide käsitsi HTTP-päringu saatmisest:

`printf 'HEAD / HTTP/1.1\r\nHost: example.net\r\n\r\n' | nc example.net 80`

See võimaldab näha pakette, mis liiguvad arvuti ja veebiserveri vahel. `tcpdump` kirje lõpus olev `length` näitab, kui palju rakenduse andmeid paketis on (payloadi maht).

Serverile info saatmiseks või vastuvõtmiseks kulub alati aega. Kui saata kaks järjestikust päringut, võib server vastata esimesele päringule ootamata ära teist (sõltub ühendusest ja serveri käsitlemisloogikast).

Kui TCP ühendus avaneb, peavad klient ja server ühenduse kinnitama (three-way handshake). Klient teab serveri IP-aadressi ja porti, kuid server ei tea kliendi ühenduse parameetreid enne, kui klient saadab ühenduse algatava paki. Klient saadab serverile paki koos kliendi IP/porti infoga ja sooviga ühendus luua.

TCP jälgib pakettide järjestust, kasutades järjekorranumbreid (sequence numbers). Vastaspool kinnitab kättesaamist ACK-idega. Kui pakk läheb kaduma, näeb saatja, et kinnitust ei tulnud, ning saadab paki uuesti. Paljudel kontrollpakkidel (nt osa ACK-idest) võib `length` olla 0, sest need ei kanna rakenduse andmeid.

Järjekorranumbrite järgi saab näha, kuidas andmed jõuavad kohale ja kinnitatakse. Võrk võib toimetada pakke ka korrapäratult; operatsioonisüsteem hoiab saabuvad andmed mälus ning paneb need järjekorranumbrite alusel õigesse järjekorda.

`tcpdump` paki kirjel on osa `Flags`, millele järgnevad kandilistes sulgudes tähed või punktid. Flag on bitina talletatud tõeväärtus: 1 tähendab set, 0 tähendab unset/cleared.

Esimesel paketil TCP ühenduse loomiseks on SYN lipp (Flags [S]). Sellele vastab server paketiga, kus on SYN ja ACK (Flags [S.]). Ühenduse lõpetamisel saadetakse FIN; teine pool saadab ACK, et näidata FIN kättesaamist.

TCP alustab andmete saatmist aeglaselt (slow start). Kui teine pool kinnitab, et kõik on korras, suurendab TCP kiirust. Kui tekib kaotus või ummik, vähendab TCP kiirust ja seejärel proovib taas suurendada.

Kui serverilt vastuse saamine ületab määratud aja (timeout), siis TCP loobub ühendamisest ja annab veateate rakendusele. TCP-s on selleks mitu erinevat taimerit (näiteks retransmission timeout).

## Suured võrgud

Interneti toimimiseks on vaja ruutereid, mis on omavahel ühenduses. Iga edastus ühest seadmest teise on hop.

`traceroute` tööriistaga saab näha teekonda hop’ide kaupa, nt:

`traceroute google.com`

Kõigil IP-pakettidel on väli TTL (Time To Live), mille väärtust vähendatakse ühe võrra igal edastamisel allikast sihtpunktini. Kui TTL jõuab nullini, saadetakse allikale ICMP sõnum “time exceeded”. Sarnane probleem võib tekkida ka killustamisel, kui mõni fragment ei jõua kohale.

Latentsusaeg on aeg, mis kulub andmepaketi saatmiseks ühest võrgupunktist teise. Latentsust mõjutavad vahemaa, edastusmeedium, ruuterite ja lüüside viited ning sihtpunkti seadme koormus.

Ülekandemaht (bandwidth) on edastuskanali läbilaskevõime ehk suurim võimalik andmeedastuskiirus kanalis. Mõõtühik on bit/s või B/s.

Tulemüür (firewall) on tarkvara või seade, mille põhiülesanne on reguleerida liiklust erineva turvatasemega võrkude vahel, eraldades näiteks kodu- või kontorivõrgu ülejäänud internetist. Arvutivõrkudes on tulemüüri ülesandeks takistada pahatahtliku liikluse ja rünnete levikut välisvõrgust sisevõrku või konkreetsesse arvutisse.
