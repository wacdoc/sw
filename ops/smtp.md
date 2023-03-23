# Unda seva yako ya kutuma barua ya SMTP

## utangulizi

SMTP inaweza kununua moja kwa moja huduma kutoka kwa wachuuzi wa wingu, kama vile:

* [Amazon SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali cloud email push](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Unaweza pia kuunda seva yako ya barua - utumaji usio na kikomo, gharama ya chini kwa jumla.

Hapo chini, tunaonyesha hatua kwa hatua jinsi ya kuunda seva yetu ya barua.

## Uchaguzi wa seva

Seva ya SMTP inayojipangisha yenyewe inahitaji IP ya umma iliyo na milango 25, 456 na 587 wazi.

Mawingu ya umma yanayotumiwa kawaida yamezuia bandari hizi kwa chaguo-msingi, na inawezekana kuzifungua kwa kutoa amri ya kazi, lakini ni shida sana baada ya yote.

Ninapendekeza kununua kutoka kwa seva pangishi ambayo bandari hizi zimefunguliwa na inasaidia kusanidi majina ya vikoa kinyume.

Hapa, ninapendekeza [Contabo](https://contabo.com) .

Contabo ni mtoa huduma mwenyeji aliye mjini Munich, Ujerumani, iliyoanzishwa mwaka wa 2003 kwa bei za ushindani sana.

Ukichagua Euro kama sarafu ya ununuzi, bei itakuwa nafuu zaidi (seva yenye kumbukumbu ya 8GB na CPU 4 hugharimu takriban yuan 529 kwa mwaka, na ada ya awali ya usakinishaji ni bure kwa mwaka mmoja).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

Wakati wa kuagiza, remark `prefer AMD` , na seva iliyo na AMD CPU itakuwa na utendakazi bora.

Katika zifuatazo, nitachukua VPS ya Contabo kama mfano ili kuonyesha jinsi ya kuunda seva yako ya barua.

## Usanidi wa mfumo wa Ubuntu

Mfumo wa uendeshaji hapa ni Ubuntu 22.04

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

Ikiwa seva kwenye ssh itaonyesha `Welcome to TinyCore 13!` (kama inavyoonyeshwa kwenye takwimu hapa chini), inamaanisha kuwa mfumo bado haujasakinishwa. Tafadhali tenganisha ssh na usubiri kwa dakika chache ili kuingia tena.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

Wakati `Welcome to Ubuntu 22.04.1 LTS` inaonekana, uanzishaji umekamilika, na unaweza kuendelea na hatua zifuatazo.

### [Si lazima] Anzisha mazingira ya maendeleo

Hatua hii ni ya hiari.

Kwa urahisi, niliweka usakinishaji na usanidi wa mfumo wa programu ya ubuntu ndani [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) .

Endesha amri ifuatayo ili kusakinisha kwa mbofyo mmoja.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Watumiaji wa Kichina, tafadhali tumia amri ifuatayo badala yake, na lugha, saa za eneo, n.k. zitawekwa kiotomatiki.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Contabo huwasha IPV6

Washa IPV6 ili SMTP iweze kutuma barua pepe zilizo na anwani za IPV6.

hariri `/etc/sysctl.conf`

Rekebisha au ongeza mistari ifuatayo

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

Fuata [mafunzo ya mawasiliano: Kuongeza muunganisho wa IPv6 kwenye seva yako](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

Hariri `/etc/netplan/01-netcfg.yaml` , ongeza mistari michache kama inavyoonyeshwa kwenye takwimu hapa chini (Faili ya usanidi chaguo-msingi ya Contabo VPS tayari ina mistari hii, iondoe tu).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Kisha `netplan apply` ili kufanya usanidi uliorekebishwa uanze kutumika.

Baada ya usanidi kufanikiwa, unaweza kutumia `curl 6.ipw.cn` kutazama anwani ya ipv6 ya mtandao wako wa nje.

## Weka mipangilio ya hifadhi ya usanidi

```
git clone https://github.com/wactax/ops.soft.git
```

## Tengeneza cheti cha bure cha SSL kwa jina la kikoa chako

Kutuma barua kunahitaji cheti cha SSL kwa usimbaji fiche na kutia sahihi.

Tunatumia [acme.sh](https://github.com/acmesh-official/acme.sh) kutengeneza vyeti.

acme.sh ni zana huria ya kusaini cheti kiotomatiki cha chanzo,

Ingiza ghala la usanidi ops.soft, endesha `./ssl.sh` , na folda ya `conf` itaundwa kwenye **saraka ya juu** .

Tafuta mtoa huduma wako wa DNS kutoka [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) , hariri `conf/conf.sh` .

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Kisha endesha `./ssl.sh 123.com` ili kuzalisha `123.com` na vyeti vya `*.123.com` kwa jina la kikoa chako.

Uendeshaji wa kwanza utasakinisha kiotomatiki [acme.sh](https://github.com/acmesh-official/acme.sh) na kuongeza kazi iliyoratibiwa kwa usasishaji kiotomatiki. Unaweza kuona `crontab -l` , kuna mstari kama ifuatavyo.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Njia ya cheti kilichotolewa ni kitu kama `/mnt/www/.acme.sh/123.com_ecc。`

Upyaji wa cheti utaita hati ya `conf/reload/123.com.sh` , hariri hati hii, unaweza kuongeza amri kama vile `nginx -s reload` ili kuonyesha upya akiba ya cheti cha programu zinazohusiana.

## Jenga seva ya SMTP na chasquid

[chasquid](https://github.com/albertito/chasquid) ni seva ya chanzo huria ya SMTP iliyoandikwa kwa lugha ya Go.

Kama mbadala wa programu za seva za barua za zamani kama vile Postfix na Sendmail, chasquid ni rahisi na rahisi kutumia, na pia ni rahisi kwa maendeleo ya pili.

Endesha `./chasquid/init.sh 123.com` itasakinishwa kiotomatiki kwa mbofyo mmoja (badilisha 123.com na jina la kikoa chako cha kutuma).

## Sanidi Sahihi ya Barua Pepe DKIM

DKIM hutumiwa kutuma saini za barua pepe ili kuzuia barua zisichukuliwe kama barua taka.

Baada ya amri kufanya kazi kwa mafanikio, utaulizwa kuweka rekodi ya DKIM (kama inavyoonyeshwa hapa chini).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

Ongeza tu rekodi ya TXT kwenye DNS yako (kama inavyoonyeshwa hapa chini).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## Tazama hali ya huduma na kumbukumbu

 `systemctl status chasquid` Tazama hali ya huduma.

Hali ya operesheni ya kawaida ni kama inavyoonyeshwa kwenye takwimu hapa chini

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` au `journalctl -xeu chasquid` inaweza kutazama logi ya makosa.

## Badilisha usanidi wa jina la kikoa

Jina la kikoa la nyuma ni kuruhusu anwani ya IP kutatuliwa kwa jina la kikoa linalolingana.

Kuweka jina la kikoa kinyume kunaweza kuzuia barua pepe kutambuliwa kama barua taka.

Barua inapopokelewa, seva inayopokea itafanya uchambuzi wa jina la kikoa kinyume kwenye anwani ya IP ya seva inayotuma ili kuthibitisha kama seva inayotuma ina jina la kikoa la kinyume.

Ikiwa seva inayotuma haina jina la kikoa cha kurudi nyuma au ikiwa jina la kikoa la kinyume halilingani na anwani ya IP ya seva inayotuma, seva inayopokea inaweza kutambua barua pepe kama barua taka au kuikataa.

Tembelea [https://my.contabo.com/rdns](https://my.contabo.com/rdns) na usanidi kama inavyoonyeshwa hapa chini

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Baada ya kuweka jina la kikoa cha nyuma, kumbuka kusanidi azimio la mbele la jina la kikoa ipv4 na ipv6 kwa seva.

## Hariri jina la mpangishi wa chasquid.conf

Rekebisha `conf/chasquid/chasquid.conf` hadi thamani ya jina la kikoa la kinyume.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Kisha endesha `systemctl restart chasquid` ili kuanza tena huduma.

## Hifadhi nakala ya conf kwenye hazina ya git

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Kwa mfano, ninahifadhi nakala ya folda ya conf kwa mchakato wangu mwenyewe wa github kama ifuatavyo

Unda ghala la kibinafsi kwanza

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Ingiza saraka ya conf na uwasilishe kwenye ghala

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Ongeza mtumaji

kukimbia

```
chasquid-util user-add i@wac.tax
```

Inaweza kuongeza mtumaji

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Thibitisha kuwa nenosiri limewekwa kwa usahihi

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Baada ya kuongeza mtumiaji, `chasquid/domains/wac.tax/users` itasasishwa, kumbuka kuiwasilisha kwenye ghala.

## DNS ongeza rekodi ya SPF

SPF ( Mfumo wa Sera ya Mtumaji) ni teknolojia ya uthibitishaji wa barua pepe inayotumiwa kuzuia ulaghai wa barua pepe.

Inathibitisha utambulisho wa mtumaji barua kwa kuangalia kama anwani ya IP ya mtumaji inalingana na rekodi za DNS za jina la kikoa analodai kuwa, hivyo kuzuia walaghai kutuma barua pepe za uwongo.

Kuongeza rekodi za SPF kunaweza kuzuia barua pepe kutambuliwa kama barua taka kadiri inavyowezekana.

Ikiwa seva yako ya jina la kikoa haiauni aina ya SPF, ongeza rekodi ya aina ya TXT.

Kwa mfano, SPF ya `wac.tax` ni kama ifuatavyo

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

SPF kwa `_spf.wac.tax`

`v=spf1 a:smtp.wac.tax ~all`

Kumbuka kuwa `include:_spf.google.com` hapa, hii ni kwa sababu nitasanidi `i@wac.tax` kama anwani ya kutuma katika kisanduku cha barua cha Google baadaye.

## Usanidi wa DNS DMARC

DMARC ni ufupisho wa (Uthibitishaji wa Ujumbe wa Kikoa, Kuripoti & Upatanifu).

Inatumika kunasa midundo ya SPF (labda imesababishwa na hitilafu za usanidi, au mtu mwingine anajifanya kuwa wewe ili kutuma barua taka).

Ongeza rekodi ya TXT `_dmarc` ,

Maudhui ni kama ifuatavyo

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Maana ya kila parameta ni kama ifuatavyo

### p (Sera)

Inaonyesha jinsi ya kushughulikia barua pepe ambazo hazijathibitishwa na SPF (Mfumo wa Sera ya Mtumaji) au DKIM (Barua Iliyotambulishwa ya Kikoa). Kigezo cha p kinaweza kuwekwa kwa moja ya maadili matatu:

* hakuna: Hakuna hatua inayochukuliwa, ni matokeo ya uthibitishaji pekee yanayorejeshwa kwa mtumaji kupitia utaratibu wa kuripoti barua pepe.
* Karantini: Weka barua pepe ambayo haijapitisha uthibitishaji kwenye folda ya barua taka, lakini haitakataa barua pepe moja kwa moja.
* kataa: Kataa moja kwa moja barua pepe ambazo hazijathibitishwa.

### fo (Chaguo za Kushindwa)

Hubainisha kiasi cha maelezo yanayorejeshwa na utaratibu wa kuripoti. Inaweza kuwekwa kwa mojawapo ya maadili yafuatayo:

* 0: Ripoti matokeo ya uthibitishaji kwa ujumbe wote
* 1: Ripoti ujumbe ambao haujathibitishwa pekee
* d: Ripoti mapungufu ya uthibitishaji wa jina la kikoa pekee
* s: ripoti tu mapungufu ya uthibitishaji wa SPF
* l: Ripoti hitilafu za uthibitishaji wa DKIM pekee

### ruf na ruf

* rua (Kuripoti URI kwa ripoti za Jumla): Anwani ya barua pepe ya kupokea ripoti zilizojumlishwa
* ruf (Kuripoti URI kwa ripoti za Uchunguzi wa Uchunguzi): anwani ya barua pepe ili kupokea ripoti za kina

## Ongeza rekodi za MX ili kusambaza barua pepe kwa Google Mail

Kwa sababu sikuweza kupata kisanduku cha barua cha biashara kisicholipishwa ambacho kinaweza kutumia anwani za ulimwengu wote (Catch-All, inaweza kupokea barua pepe zozote zinazotumwa kwa jina la kikoa hiki, bila vizuizi vya viambishi awali), nilitumia chasquid kusambaza barua pepe zote kwenye kisanduku changu cha barua cha Gmail.

**Ikiwa una kisanduku chako cha barua cha biashara kilicholipiwa, tafadhali usibadilishe MX na uruke hatua hii.**

Hariri `conf/chasquid/domains/wac.tax/aliases` , weka kisanduku cha barua cha kusambaza

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` inaonyesha barua pepe zote, `i` ni kiambishi awali cha anwani ya barua pepe ya mtumiaji anayetuma iliyoundwa hapo juu. Ili kusambaza barua pepe, kila mtumiaji anahitaji kuongeza laini.

Kisha ongeza rekodi ya MX (Ninaelekeza moja kwa moja kwa anwani ya jina la kikoa cha nyuma hapa, kama inavyoonyeshwa kwenye mstari wa kwanza kwenye takwimu hapa chini).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Baada ya usanidi kukamilika, unaweza kutumia barua pepe nyingine kutuma barua pepe kwa `i@wac.tax` na `any123@wac.tax` ili kuona kama unaweza kupokea barua pepe katika Gmail.

Ikiwa sivyo, angalia logi ya chasquid ( `grep chasquid /var/log/syslog` ).

## Tuma barua pepe kwa i@wac.tax na Google Mail

Baada ya Google Mail kupokea barua hiyo, nilitarajia kujibu kwa `i@wac.tax` badala ya i.wac.tax@gmail.com.

Tembelea [https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) na ubofye "Ongeza anwani nyingine ya barua pepe".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Kisha, ingiza msimbo wa uthibitishaji uliopokelewa na barua pepe ambayo ilitumwa.

Hatimaye, inaweza kuwekwa kama anwani chaguo-msingi ya mtumaji (pamoja na chaguo la kujibu kwa kutumia anwani sawa).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Kwa njia hii, tumekamilisha uanzishaji wa seva ya barua ya SMTP na wakati huo huo kutumia Google Mail kutuma na kupokea barua pepe.

## Tuma barua pepe ya jaribio ili kuangalia kama usanidi umefaulu

Ingiza `ops/chasquid`

Run `direnv allow` kusakinisha utegemezi (direnv imewekwa katika mchakato wa uanzishaji wa ufunguo mmoja uliopita na ndoano imeongezwa kwenye ganda)

kisha kukimbia

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Maana ya vigezo ni kama ifuatavyo

* mtumiaji: jina la mtumiaji la SMTP
* kupita: Nenosiri la SMTP
* kwa: mpokeaji

Unaweza kutuma barua pepe ya majaribio.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

Inapendekezwa kutumia Gmail kupokea barua pepe za majaribio ili kuangalia kama usanidi umefaulu.

### Usimbaji fiche wa kawaida wa TLS

Kama inavyoonyeshwa kwenye mchoro hapa chini, kuna kufuli hii ndogo, ambayo inamaanisha kuwa cheti cha SSL kimewezeshwa kwa mafanikio.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Kisha ubofye "Onyesha Barua pepe Halisi"

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

Kama inavyoonyeshwa kwenye mchoro hapa chini, ukurasa wa barua wa asili wa Gmail unaonyesha DKIM, ambayo inamaanisha kuwa usanidi wa DKIM umefaulu.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Angalia Imepokewa katika kichwa cha barua pepe asili, na unaweza kuona kwamba anwani ya mtumaji ni IPV6, ambayo ina maana kwamba IPV6 pia imesanidiwa kwa ufanisi.
