##Principles

These letters are Different: **t, d, m, n, c, g, p, x, f**.

These pairs are the Same: **b v, r l, s z**. The latter is for English speakers!

Excluded: **j, w, h, e, q**

Fancy: **y, k**

Vowels: **a i o u**. We reserve **e** for shwa type sounds in between syllables, which many languages will need. 

Different letters form unique pairs. We may use either of the Same letters, but do not consider them distinct in constructing a phoneme. E.g, since we have `zod`, we may not have `sod`.

This gives us, in essence, 12 consonants. 132 pairs of which we can use, so we don't have duplication like `bub`. To avoid a couple hard collisions, we use `t` and `c` at the end. To begin, we use `y` and `k`. `c` and `k` are phonemically the same, but tackat is easier to parse than taccat. Both elide into one `k`, which is in practice fine. 

If we assign two vowels to the low end, and two to the high, we get 264 symbols for each. We remove eight that sound sweary in English. Other languages may amuse themselves accordingly. 

I then hand tuned the Same letters to sound nice in my own language. This is what I've come up with:

```
 yad yid yam yim yan yin yac yic yag yig yap yip yax yix yaf yif yav yiv yas yiz yal yir 
 dat dit     dim dan din dac dic dag dig dap dip dax dix daf dif dav div das diz dal dir 
 mat mit mad mid man min mac mic mag mig map mip max mix maf mif mav miv mas miz mar mil
 nat nit nad nid nam nim nac nic nag nig nap nip nax nix naf nif nav niv nas niz nal nir 
 kat kit kad kid kam kim kan kin kag kig kap kip kax kix kaf kif kav kiv kas kiz kal kir 
 gat git gad gid gam gim gan gin gac gic gap gip gax gix gaf gif gav giv gas giz gal gir 
 pat pit pad pid pam pim pan pin pac pic pag pig pax pix paf pif pav piv pas piz par pil 
         xad xid xam xim xan xin xac xic xag xig xap xip xaf xif xav xiv xaz xis xar xil
 fat fit fad fid fam fim fan fin     fic     fig     fip fax fix fav fiv faz fis fal fir
 vat bit vad vid bam vim van vin bac bic vag vig vap vip vax bix baf vif baz viz var vir
 zat sit zad sid zam zim san zin sac sic zag zig zap zip sax six saf zif sav ziv zar sil
 lat lit rad rid ram rim lan rin lac ric rag rig rap lip lax lix raf rif rav liv laz riz

 yod yud yom yum yon yun yoc yuc yog yug yop yup yox yux yof yuf yov yuv yos yuz yol yur 
 dot dut     dum don dun doc duc dog dug dop dup dox dux dof duf dov duv dos duz dol dur 
 mot mut mod mud mon mun moc muc mog mug mop mup mox mux mof muf mov muv mos muz mor mul
 not nut nod nud nom num noc nuc nog nug nop nup nox nux nof nuf nov nuv nos nuz nol nur 
 kot kut kod kud kom kum kon kun kog kug kop kup kox kux kof kuf kov kuv kos kuz kol kur 
 got gut god gud gom gum gon gun goc guc gop gup gox gux gof guf gov guv gos guz gol gur 
 pot put pod pud pom pum pon pun poc puc pog pug pox pux pof puf pov puv pos puz por pul 
         xod xud xom xum xon xun xoc xuc xog xug xop xup xof xuf xov xuv xoz xus xor xul
 fot fut fod fud fom fum fon fun     fuc     fug     fup fox fux fov fuv foz fus fol fur
 vot but vod vud bom vum von vun boc buc vog vug vop vup vox bux bof vuf boz vuz vor vur
 zot sut zod sud zom zum son zun soc suc zog zug zop zup sox sux sof zuf sov zuv zor sul
 lot lut rod rud rom rum lon run loc ruc rog rug rop lup lox lux rof ruf rov luv loz ruz
```

 The order is generated. To work for Urbit, or anything else really, these need to be permuted. You'll note that `zod` is hanging out in the low end as requested. 256 could be ~diznoc, for rough continuity. Or ~daznoc, I chose `das` but this is of course arbitrary.

This is only rough on English in that three letter syllables beginning with x are hen's teeth. This will encourage us to pronounce `x` "sh", which is the only preferred phonetic realization that will be counterintuitive for English speakers. "ch" and "loogie" are also ok.

For posterity, I made the pairs thus:

```javascript
var consonants = ["t", "d", "m", "n", "c", "g", "p", "x", "f", "v", "z", "r"];

var vowels = ["a", "i"]

var pairs = [];


for (var i = 0 ; i < consonants.length; i ++) {
    for (var j = 0 ; j < consonants.length; j ++) {
        for (var a = 0; a < vowels.length; a++) {
            if (j != i) {
            console.log(consonants[i] + vowels[a] + consonants[j]);  
            }
        }
    }
}
```
If that ain't dirty pool, what is?

Let's permute in the same cavalier fashion:

```javascript
var tags = [["yod", "yud", "yom", "yum", "yon", "yun", "yoc", "yuc", "yog", "yug", "yop", "yup", "yox", "yux", "yof", "yuf", "yov", "yuv", "yos", "yuz", "yol", "yur"],
 ["dot", "dut", "@@@", "dum", "don", "dun", "doc", "duc", "dog", "dug", "dop", "dup", "dox", "dux", "dof", "duf", "dov", "duv", "dos", "duz", "dol", "dur"],
 ["mot", "mut", "mod", "mud", "mon", "mun", "moc", "muc", "mog", "mug", "mop", "mup", "mox", "mux", "mof", "muf", "mov", "muv", "mos", "muz", "mor", "mul"],
 ["not", "nut", "nod", "nud", "nom", "num", "noc", "nuc", "nog", "nug", "nop", "nup", "nox", "nux", "nof", "nuf", "nov", "nuv", "nos", "nuz", "nol", "nur"],
 ["kot", "kut", "kod", "kud", "kom", "kum", "kon", "kun", "kog", "kug", "kop", "kup", "kox", "kux", "kof", "kuf", "kov", "kuv", "kos", "kuz", "kol", "kur"],
 ["got", "gut", "god", "gud", "gom", "gum", "gon", "gun", "goc", "guc", "gop", "gup", "gox", "gux", "gof", "guf", "gov", "guv", "gos", "guz", "gol", "gur"],
 ["pot", "put", "pod", "pud", "pom", "pum", "pon", "pun", "poc", "puc", "pog", "pug", "pox", "pux", "pof", "puf", "pov", "puv", "pos", "puz", "por", "pul"],
 ["@@@", "@@@", "xod", "xud", "xom", "xum", "xon", "xun", "xoc", "xuc", "xog", "xug", "xop", "xup", "xof", "xuf", "xov", "xuv", "xoz", "xus", "xor", "xul"],
 ["fot", "fut", "fod", "fud", "fom", "fum", "fon", "fun", "@@@", "fuc", "@@@", "fug", "@@@", "fup", "fox", "fux", "fov", "fuv", "foz", "fus", "fol", "fur"],
 ["vot", "but", "vod", "vud", "bom", "vum", "von", "vun", "boc", "buc", "vog", "vug", "vop", "vup", "vox", "bux", "bof", "vuf", "boz", "vuz", "vor", "vur"],
 ["zot", "sut", "zod", "sud", "zom", "zum", "son", "zun", "soc", "suc", "zog", "zug", "zop", "zup", "sox", "sux", "sof", "zuf", "sov", "zuv", "zor", "sul"],
 ["lot", "lut", "rod", "rud", "rom", "rum", "lon", "run", "loc", "ruc", "rog", "rug", "rop", "lup", "lox", "lux", "rof", "ruf", "rov", "luv", "loz", "ruz"]];
 
 var permutes = [["yod", "yud", "yom", "yum", "yon", "yun", "yoc", "yuc", "yog", "yug", "yop", "yup", "yox", "yux", "yof", "yuf", "yov", "yuv", "yos", "yuz", "yol", "yur"],
 ["dot", "dut", "@@@", "dum", "don", "dun", "doc", "duc", "dog", "dug", "dop", "dup", "dox", "dux", "dof", "duf", "dov", "duv", "dos", "duz", "dol", "dur"],
 ["mot", "mut", "mod", "mud", "mon", "mun", "moc", "muc", "mog", "mug", "mop", "mup", "mox", "mux", "mof", "muf", "mov", "muv", "mos", "muz", "mor", "mul"],
 ["not", "nut", "nod", "nud", "nom", "num", "noc", "nuc", "nog", "nug", "nop", "nup", "nox", "nux", "nof", "nuf", "nov", "nuv", "nos", "nuz", "nol", "nur"],
 ["kot", "kut", "kod", "kud", "kom", "kum", "kon", "kun", "kog", "kug", "kop", "kup", "kox", "kux", "kof", "kuf", "kov", "kuv", "kos", "kuz", "kol", "kur"],
 ["got", "gut", "god", "gud", "gom", "gum", "gon", "gun", "goc", "guc", "gop", "gup", "gox", "gux", "gof", "guf", "gov", "guv", "gos", "guz", "gol", "gur"],
 ["pot", "put", "pod", "pud", "pom", "pum", "pon", "pun", "poc", "puc", "pog", "pug", "pox", "pux", "pof", "puf", "pov", "puv", "pos", "puz", "por", "pul"],
 ["@@@", "@@@", "xod", "xud", "xom", "xum", "xon", "xun", "xoc", "xuc", "xog", "xug", "xop", "xup", "xof", "xuf", "xov", "xuv", "xoz", "xus", "xor", "xul"],
 ["fot", "fut", "fod", "fud", "fom", "fum", "fon", "fun", "@@@", "fuc", "@@@", "fug", "@@@", "fup", "fox", "fux", "fov", "fuv", "foz", "fus", "fol", "fur"],
 ["vot", "but", "vod", "vud", "bom", "vum", "von", "vun", "boc", "buc", "vog", "vug", "vop", "vup", "vox", "bux", "bof", "vuf", "boz", "vuz", "vor", "vur"],
 ["zot", "sut", "zod", "sud", "zom", "zum", "son", "zun", "soc", "suc", "zog", "zug", "zop", "zup", "sox", "sux", "sof", "zuf", "sov", "zuv", "zor", "sul"],
 ["lot", "lut", "rod", "rud", "rom", "rum", "lon", "run", "loc", "ruc", "rog", "rug", "rop", "lup", "lox", "lux", "rof", "ruf", "rov", "luv", "loz", "ruz"]];
 
 var i, j = 0 ;
 
for (i = 0 ; i < tags.length; i++) {
     for ( j = 0; j < tags[i].length; j++) {
         right = ((i+j)* 13) % tags[i].length;
         tags[i][j] = permutes[i][right];
     }
}
 
  
for ( j = 0; j < tags[0].length; j++) {
     for (i = 0 ; i < tags.length; i++) {       
         left = ((i+j) * 7) % tags.length;
         permutes[i][j] = tags[left][j];
         }
}
  
//one more munge

for (i = 0 ; i < tags.length; i++) {
     for ( j = 0; j < tags[i].length; j++) {
        if (j%2 === 0) { 
            tags[i][j]= permutes[(i+3) % tags.length][j];
        } else {
            tags[i][j]= permutes[i][j];
        }
     }
}

    
for (i = 0; i < tags.length; i++) {
    console.log(tags[i].join())
}
     
     
```

That'll do it! We end up with these:

```
zod,noc,lux,vod,dur,rog,nuc,dov,guf,nod,@@@,gop,buc,xoz,ruf,bom,dut,rop,nug,dos,guv,nom
kog,muv,pov,kud,fod,pug,zog,fus,yov,zum,mod,yup,kop,muz,pos,kum,fom,pux,zop,fur,yos,zun
rug,vor,duv,lon,nud,dox,gup,nol,xus,gon,vum,xof,lup,vot,duz,loc,num,dof,gux,not,xul,goc
pox,kur,fol,pun,son,fux,yox,sut,mor,yuc,kon,muf,pof,kut,fot,puc,soc,fuv,yof,sud,mot,yug
dux,rod,nur,dog,gun,nov,xuf,god,but,xog,ruc,boz,duf,rom,nut,dop,guc,nos,xuv,gom,vud,xop
fov,pud,vun,fug,yog,zuv,mov,yum,kod,mup,pog,kuz,foz,pum,zom,fup,yop,sul,mos,yun,kom,mux
nuv,don,gud,nox,xug,gol,vuz,xon,rum,vox,dup,lot,nuz,doc,gum,nof,xup,got,vur,xoc,run,bof
zor,fun,yon,sux,mox,yur,kol,muc,pon,kuf,fox,put,zot,@@@,yoc,zuf,mof,yud,kot,mug,poc,kuv
gur,nog,xun,gov,bux,xod,lut,vog,duc,rov,nuf,@@@,gut,nop,xuc,gos,vuf,xom,rud,vop,dug,loz
yod,zug,mog,yuv,kov,mud,pod,kup,@@@,puz,sov,fum,yom,zup,mop,yuz,kos,mun,pom,kux,@@@,pul
xud,gox,vug,xor,luv,von,@@@,lox,nup,dol,guz,xov,xum,gof,vup,@@@,ruz,boc,dun,rof,nux,dot
mon,yux,kox,mul,por,kun,fon,puf,sox,fut,yol,suc,moc,yuf,kof,mut,pot,kug,@@@,puv,sof,fud

daz,xav,lix,vad,dir,rag,nic,dav,gif,nad,@@@,gap,bic,xaz,rif,bam,dit,rap,nig,vin,giv,nam
kag,miv,pav,kid,fad,pig,zag,fis,yav,zim,mad,yip,kap,miz,pas,kim,fam,pix,zap,fir,yas,zin
rig,var,div,lan,nid,dax,gip,nal,xis,gan,vim,xaf,lip,vat,dis,lac,nim,daf,gix,nat,xil,gac
pax,kir,fal,pin,san,fix,yax,sit,mar,yic,kan,mif,paf,kit,fat,pic,sac,fiv,yaf,sid,mat,yig
dix,rad,nir,dag,gin,nav,xif,gad,bit,xag,ric,baz,dif,ram,nit,dap,gic,nas,xiv,gam,vid,xap
fav,pid,zad,fig,yag,ziv,mav,yim,kad,mip,pag,kiz,faz,pim,zam,fip,yap,sil,mas,yin,kam,mix
niv,dan,gid,nax,xig,gal,viz,xan,rim,vax,dip,lat,niz,dac,gim,naf,xip,gat,vir,xac,rin,baf
zar,fin,yan,six,max,yir,kal,mic,pan,kif,fax,pit,zat,fic,yac,zif,maf,yid,kat,mig,pac,kiv
gir,nag,xin,gav,bix,xad,lit,vag,@@@,rav,nif,@@@,git,nap,xic,gas,vif,xam,rid,vap,dig,laz
yad,zig,mag,yiv,kav,mid,pad,kip,@@@,piz,sav,fim,yam,zip,map,yiz,kas,min,pam,kix,@@@,pil
xid,gax,vig,xar,liv,van,dim,lax,nip,dal,giz,nac,xim,gaf,vip,@@@,riz,bac,din,raf,nix,dat
man,yix,kax,mil,par,kin,fan,pif,sax,fit,yal,sic,mac,@@@,kaf,mit,pat,kig,@@@,piv,saf,fid

```

With a little hand-tuning so that ~zod is still zero and ~daznoc is now ~doznec or 256. A touch of the familiar to carry us through.

Which we can format for Hoon compatibility:

```
++  po
  ~/  %po
  =+  :-  ^=  sis
          'dazxavlixvaddirragnicdavgifnadgapbicxazrifbamdit\
          /rapnigvingivnamkagmivpavkidfadpigzagfisyavzimmad\
          /yipkapmizpaskimfampixzapfiryaszinrigvardivlannid\
          /daxgipnalxisganvimxaflipvatdislacnimdafgixnatxil\
          /gacpaxkirfalpinsanfixyaxsitmaryickanmifpafkitfat\
          /picsacfivyafsidmatyigdixradnirdagginnavxifgadbit\
          /xagricbazdiframnitdapgicnasxivgamvidxapfavpidzad\
          /figyagzivmavyimkadmippagkizfazpimzamfipyapsilmas\
          /yinkammixnivdangidnaxxiggalvizxanrimvaxdiplatniz\
          /dacgimnafxipgatvirxacrinbafzarfinyansixmaxyirkal\
          /micpankiffaxpitzatficyaczifmafyidkatmigpackivgir\
          /nagxingavbixxadlitvagravnifgitnapxicgasvifxamrid\
          /vapdiglazyadzigmagyivkavmidpadkippizsavfimyamzip\
          /mapyizkasminpamkixpilxidgaxvigxarlivvandimlaxnip\
          /dalgiznacximgafviprizbacdinrafnixdatmanyixkaxmil\
          /parkinfanpifsaxfityalsicmackafmitpatkigpivsaffid'
      ^=  dex
      'zodnocluxvoddurrognucdovgufnodgopbucxozrufbomdut\
      /ropnugdosguvnomkogmuvpovkudfodpugzogfusyovzummod\
      /yupkopmuzposkumfompuxzopfuryoszunrugvorduvlonnud\
      /doxgupnolxusgonvumxoflupvotduzlocnumdofguxnotxul\
      /gocpoxkurfolpunsonfuxyoxsutmoryuckonmufpofkutfot\
      /pucsocfuvyofsudmotyugduxrodnurdoggunnovxufgodbut\
      /xogrucbozdufromnutdopgucnosxuvgomvudxopfovpudvun\
      /fugyogzuvmovyumkodmuppogkuzfozpumzomfupyopsulmos\
      /yunkommuxnuvdongudnoxxuggolvuzxonrumvoxduplotnuz\
      /docgumnofxupgotvurxocrunbofzorfunyonsuxmoxyurkol\
      /mucponkuffoxputzotyoczufmofyudkotmugpockuvgurnog\
      /xungovbuxxodlutvogducrovnufgutnopxucgosvufxomrud\
      /vopduglozyodzugmogyuvkovmudpodkuppuzsovfumyomzup\
      /mopyuzkosmunpomkuxpulxudgoxvugxorluvvonloxnupdol\
      /guzxovxumgofvupruzbocdunrofnuxdotmonyuxkoxmulpor\
      /kunfonpufsoxfutyolsucmocyufkofmutpotkugpuvsoffud'
  |%
```

Indeed.

