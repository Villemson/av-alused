# av-alused

##Sissejuhatus

Läbin kursuse https://www.udacity.com/course/networking-for-web-developers--ud256

Seal olevaid õpetusi katsetan https://cloud.google.com/shell

Enne kasutamist käivitan käsud 

`sudo apt-get update `

`sudo apt-get install netcat-openbsd tcpdump traceroute mtr`

## Pingist HTTP-le

Ping on arvutivõrgu võrguühenduse diagnostikaprogramm, mille abil IP-võrgus hinnatakse hosti ligipääsetavust programmi käivitanud arvutist, samuti pakettide edastamiseks kuluvat aega.

Võrguprotokoll määratleb võrguseadmete vahelise suhtlemise eeskirjad ja kokkulepped. Võrguprotokollid sisaldavad mehhanisme, mis võimaldavad seadmeid üksteisega identifitseerida ja omavahel ühendada, samuti vormingu reegleid, mis määravad, kuidas andmed pakitud saata ja vastu võtta. Mõned protokollid toetavad ka sõnumite kinnitamist ja andmete tihendamist, mis on loodud usaldusväärseks ja / või suure jõudlusega võrguühenduseks. Võrguprotokolli viga tekib siis, kui Interneti-ühendus on halb.
Võrguprotokolle on kolme tüüpi: Interneti-protokoll, Traadita võrgu protokollid ja marsruutimisprotokollid.

HTTP Hypertext Transfer Protocol SSH Secure Shell. HTTP on andmete edastamise aluseks veebis.

Interneti klient/server programmid suhtlevad nn. portide kaudu.
Vastavalt protokolli tüübile jagatakse porte TCP- ja UDP-portideks.

Ühes arvutis võib korraga olla käimas mitmeid rakendusi, nii kliente kui ka servereid. Et aru saada, milline pakett millise rakenduse juurde kuulub, tuleb korraldada nende sorteerimine nn. portide kaupa. Porte iseloomustatakse numbrite abil (täisarv vahemikus 0 kuni 65535), seejuures numbrid 0 kuni 1023 on reserveeritud kindlate rakenduste jaoks. Teenusega serveri poolt on seotud  konkreetne pordi number, kuid kliendipoolne pordi number määratakse dünaamiliselt, et tagada ühenduse ühene identifitseerimine. Näiteks saab samade arvutite vahel korraga saata mitut kirja. Selleks, et erinevatesse kirjadesse kuuluvad paketid segi ei läheks, peab iga kirja jaoks olema eraldi ühendus, s.t. erinev komplekt pordinumbreid.

Brauseriga veebilehe avamisel laetakse mitmeid faile korraga. Mida kiiremini need alla laetakse, seda kiiremini avaneb veebileht. Kui 1000 inimest proovib läbi ukse siseneda, siis tekib seal tunduvalt aeglasem liikumine. Aga kui oleks avatud 2 ust, siis on sisenemine palju kiirem. Uue ühenduse tekkimisel server jagab selle kaheks. Järgmine võimalus on, et serverid tekitavad protsessidest või lõimedest ühise kogumi, millest igaüks saab hallata üht ühendust korraga. See on kiirem võrreldes uue ühenduse loomisega, kuid peale teatud arvu päringute haldamist hakkab aeglustuma. Veel on variant, et sama protsessi käigus päringute haldamine kiiresti vahetub nende vabanemisel.

käsk `printf`ja nc(netcat) control-d või ^d failiks `example.txt: `

## Nimed ja aadressid

IP-aadress (lühend Internet Protocol address-ist) on kasutusel selleks, et eristada Internetis erinevaid arvuteid ja seadmeid. Tema tööpõhimõte on sarnane ümbrikul oleva kirja saatja aadressi tööpõhimõttega.
DNS(Domain Name System) internetiteenus, mis teisendab domeeninimed IP-aadressideks ja vastupidi.

Host on arvuti, mis jagab teistele arvutitele või muudele seadmetele infot.

Hosti korraldus host eesti.ee on baastööriist, et otsida DNS-st kirjeid. See käsk esitab päringu operatsioonisüsteemi võrgule, mis reeglina lõpeb sellega, et saadetakse päring saatja arvutile seadistatud DNS-i serverile.
Dig on samalaadne tööriist nagu host, kuid tehnilisem. Päringutes saab kasutada erinevaid lippe ja parameetreid, mis teeb kasutamise paindlikuks ja päringute vastused detailsemaks.



DNS kirjeid on mitmeid. 

CNAME - aliasnimi, seab nimele vastavusse teise nime
AAAA - aadress, seab nimele vastavusse IPv6 aadressi
NS - tsooni või alamtsooni nimeserver NS-kirje määrab, milline nimeserver teenindab antud tsooni. Iga NS kirje on kas delegeerimiskirje või pädevuskirje (authority record). Kui NS kirje asub samanimelises tsoonis, siis ta on pädevuskirje. Kui NS kirje nimi on tütardomeeni (descendant domain) nimi, siis ta on delegeerimiskirje.
A - aadress, seab nimele vastavusse IP aadressi.

Domeeninimi internetis peab olema kordumatu. Peamine eesmärk on IP-aadresside numbrilisele kujule vastava paralleelse esitusviisi loomine. Domeeninimi võib koosneda tähtedest, numbritest ja sidekriipsudest. 
Veebiserver saab töödelda päringuid erinevatelt veebisaitidelt. HTTP-päringutel on hostipäis (host header).
Hostipäis on vajalik HTTP-päringute jaoks. Veebiküpsised salvestatakse kindlale domeeninimele.

Kasutusel on kaks IP-aadressi versiooni:
a) IPv4 (IP versioon 4), kus aadress koosneb neljast punktiga eraldatud täisarvust vahemikus
0 kuni 255. Näiteks 213.184.53.178; (32bit)
b) IPv6 (IP versioon 6), kus aadress koosneb kaheksast kooloniga eraldatud kuueteistkümnendarvude3
 rühmast, näiteks 2001:0db8:85a3:0042:1000:8a2e:0370:7334. (128bit)


## Adresseerimine ja võrgud

Kõiki  32-bitiseid väärtuseid ei kasutata päris aadressidena. Osad on reserveeritud spetsiaalsete protokollide jaoks, sisemisteks privaatvõrkude jaoks, testimiseks või dokumenteerimiseks. Juhul kui saaks kasutada kõiki 32-bitilisi aadresse, siis neid kõigile ei jaguks.

Samas võrguplokis olevad arvutid saavad sarnase IP ning omavaheliseks ühemduseks ei vaja eraldi ruuterit.
Võrgud on erineva suurusega. Lühem võrgu eesliide võimaldab rohkem aadresse hosti jaoks. Võrgu eesliite pikkus tuleb valida võrku üles seades. Kui on 22-bitise eesliitega võrk, siis 8 biti jääb üle aadressi hosti osa jaoks. Seda märgitakse /22.

Veel on võimalus märkida võrgu suurust on alamvõrgumask (subnet mask), mida sageli kasutatakse võrgu seadistamisel. Nagu IPv4 aadressid on ka alamvõrgumaskid 32-bitiste väärtustega.

Arvutil on näiteks wifi,  etherneti ja loopback liides. Loopback liidesel on reeglina IP-aadressiks 127.0.0.1 ja nimeks localhost - see laseb programmidel kasutada protokollivirnasid, et suhelda sama hosti teiste programmidega. Linuxis saab liidesed leida käsuga `ip addr show`.

Koduruuteril on kohalikud liidesed (etherneti, wifi) ja üks liides, mis on ühendatud internetiteenuse pakkujaga. Laivõrk (WAN) on liides, mis ühendub ülejäänud internetiga. Kohalikus võrgus (LAN) on koduarvuti ja seadmed.

## Protokollikihid


Võrguprotokollid koosnevad mitmest kihist. Käsk `tcpdump `võimaldab näha võrguliiklust võrgu ja hosti vahel. 

`printf 'HEAD / HTTP/1.1\r\nHost: example.net\r\n\r\n' | nc example.net 80` näitab pakke, mis liiguvad arvuti ja veebilehe vahel. Kirje lõpus olev length näitab saadetud pakkide andmete hulka.

Serverile info saatmiseks või vastuvõtmiseks kulub alati aega. Kui nt saata kaks järjestikust päringut, siis server vastab esimesele ootamata ära teist päringut.

Kui TCP ühendus avaneb, peavad klient ja server selle kinnitama.Klient saab teada serveri IP-aadressi ja pordinumbri, aga server kliendist ei näe. Klient peab saatma serverile info, mis sisaldab kliendi IP-aadressi ja pordinumbrit ning korraldust ühendamiseks. TCP jälgib järjestust selliselt, et paneb järjekorranumbri iga pakile. Lõpus saadab kinnituse, kui on kätte saanud kindla järjekorranumbri ja kui pakk läheb kaduma, siis algsaatja märkab, et kinnitus puudub ning saadab paki uuesti. Kõigi selliste pakkide length on 0.

Järjekorranumbrite järgi saab näha pakkide toimetamist kohale ja kättesaamist. Võrk võib toimetada pakke korrapäratult. Operatsioonisüsteemis lõpp-punktis on piisavalt mälumahtu saabuvate andemete hoiustamiseks. Mälus kasutab järjekorranumbreid andmete asetusena.

tcpdump paki kirjel on osa Flags, millele järgneb kandilistes sulgudes tähed või punktid. Flag on tõeväärtus (õige või vale), mida hoiustatakse mälus ühe bitina. Kui Flag on 1, siis see on set, kui 0, siis cleared või unset.

Esimesel pakil, mille edastame TCP ühenduse loomiseks, omab alati SYN lipuke (Flags [S]). Anutd pakile tuleb uus suvaline järjekorranumber. Kui server loob ühenduse, siis saadetakse pakk tagasi. Pakil on SYN ja ACK lipukesed (Flags [S.]). 

Lõpp-punkti andmete ära saatmisel saab FIN paki, mis viitab lõpetamisele. Teine lõpp-punkt saadab ACK, et näidata FIN kättesaamsist.

TCP alustab andmete saatmist aeglaselt. Kui saab teiselt poolt kinnituse, et kõik ok, siis tõstab kiirust  Kui pakid on saadetud, siis TCP aeglustab ja seejärel suurendab taas kiirust. 

Serverilt vastuse saamine ületab määratud aja, siis TCP loobub ühendamisest ja annab veateate rakendusele. TCP-l on sisse ehitatud erinevad taimerid.

## Suured võrgud

Interneti toimimiseks on vaja ruutereid, mis omavahel on ühenduses.Iga edastus, mis läheb ühest masinast teise, on hop. traceroute tööriista kasutades saab näha enda võrgust teistesse serveritesse minevaid hop'e:  `traceroute google.com. `

Kõigil IP-pakettidel on väli TTL, mille väärtust vähendatakse ühe võrra paketi igal edastamise sammul allikast sihtpunktini. Juhul kui TTL-väärtus jõuab nullini või killustatud andmepaketi üks osa ei saabu sihtpunkti, saadetakse allikale sõnum "aeg on ületatud".

Latentsusaeg on aeg, mis kulub andmepaketi saatmiseks ühest võrgupunktist teise.
Latentsusaeg mõjutavad: aeg, keskkond, marsuuterites ja lüüsides tekkiv viive, sihtpunkti arvutis tekkiv viide.

Ülekandemaht (bandwidth) on edastuskanali läbilaskevõime ehk suurim võimalik või vajalik andmeedastuskiirus kanalis, mõõtühik bitt sekundis (bit/s) või bait sekundis (B/s).

Tulemüür kujutab endast kas tarkvararakendust või spetsiaalset seadet, mille  põhiülesandeks on reguleerida liiklust erineva turvatasemega arvutivõrkude vahel, eraldades näiteks väiksema kodu- või kontorivõrgu ülejäänud Internetist. Kui ehitiste puhul peab tulemüür takistama tule levikut ühest hoone osalt teise, siis arvutivõrkudes on tulemüüri ülesandeks takistada pahatahtlike rünnete levikut väheturvalisest välisvõrgust kaitstumasse sisevõrku või konkreetsesse arvutisse.
