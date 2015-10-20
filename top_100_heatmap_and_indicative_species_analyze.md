# top_100_heatmap_and_indicative_species_analyze
Jingzhe Jiang  
2015年10月19日  
# top 100 otus
combine top 100 otus of each oyster samples in cohort 2

## read data

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
origin_data <- read.csv("D:/bioinfo/UBC_data/otu_summaries/otu_table_os_top960_NormAndRanked.csv", header = TRUE, row.names = 1) # single number giving the column of the table which contains the row names
str(origin_data)
```

```
## 'data.frame':	960 obs. of  12 variables:
##  $ os1 : int  65781 6220 3572 1863 239 632 2239 2038 3537 9216 ...
##  $ os2 : int  166746 12708 762 174 456 899 1571 962 1218 669 ...
##  $ os3 : int  133838 3240 1622 799 1095 2425 1246 4931 1098 216 ...
##  $ os4 : int  114072 7716 138 1465 832 321 2280 243 7172 110 ...
##  $ os5 : int  116996 25435 1546 80 271 2153 1378 1093 286 2709 ...
##  $ os6 : int  53608 20694 2523 410 1598 3401 1585 5390 2444 705 ...
##  $ os7 : int  103203 2675 2239 83 4953 3386 2691 1232 4012 129 ...
##  $ os8 : int  32723 9560 11451 19998 15650 11040 7173 2677 2223 2059 ...
##  $ os9 : int  56342 7871 7209 17405 1020 4394 2064 3516 1243 65 ...
##  $ os10: int  112139 13210 770 155 10638 1705 5373 34 101 245 ...
##  $ os11: int  36434 3173 14906 1187 4509 9532 6013 2188 1448 9157 ...
##  $ os12: int  64483 7371 7489 2369 4602 2998 1427 3894 2628 352 ...
```

```r
row.names(origin_data)
```

```
##   [1] "denovo662732"  "denovo1425070" "denovo814559"  "denovo1760022"
##   [5] "denovo1559849" "denovo1327185" "denovo768018"  "denovo1035276"
##   [9] "denovo1477023" "denovo1543122" "denovo915108"  "denovo860670" 
##  [13] "denovo1156384" "denovo1142809" "denovo170534"  "denovo693023" 
##  [17] "denovo1093725" "denovo856104"  "denovo561174"  "denovo1683946"
##  [21] "denovo1455744" "denovo557286"  "denovo434128"  "denovo638866" 
##  [25] "denovo317806"  "denovo662736"  "denovo1579715" "denovo433458" 
##  [29] "denovo1136525" "denovo808406"  "denovo620303"  "denovo912297" 
##  [33] "denovo52350"   "denovo1218837" "denovo1103057" "denovo1194195"
##  [37] "denovo1625815" "denovo955760"  "denovo462999"  "denovo654592" 
##  [41] "denovo538192"  "denovo65672"   "denovo920937"  "denovo1584768"
##  [45] "denovo118506"  "denovo786048"  "denovo1395961" "denovo279525" 
##  [49] "denovo1324431" "denovo203775"  "denovo1720098" "denovo399954" 
##  [53] "denovo984724"  "denovo1284655" "denovo158042"  "denovo1022006"
##  [57] "denovo130148"  "denovo1251103" "denovo655412"  "denovo1377533"
##  [61] "denovo1145770" "denovo1533659" "denovo1686283" "denovo1121171"
##  [65] "denovo1639036" "denovo1017076" "denovo1725396" "denovo1220249"
##  [69] "denovo1162359" "denovo172066"  "denovo1682125" "denovo1187056"
##  [73] "denovo198633"  "denovo1663026" "denovo1210067" "denovo543764" 
##  [77] "denovo1521762" "denovo1433382" "denovo189293"  "denovo432539" 
##  [81] "denovo391969"  "denovo233345"  "denovo1730710" "denovo1502042"
##  [85] "denovo1031142" "denovo1037767" "denovo775637"  "denovo345302" 
##  [89] "denovo1500588" "denovo1293995" "denovo299535"  "denovo581502" 
##  [93] "denovo266662"  "denovo51527"   "denovo203661"  "denovo1583249"
##  [97] "denovo489250"  "denovo965254"  "denovo912979"  "denovo956839" 
## [101] "denovo921956"  "denovo1460484" "denovo1596085" "denovo638829" 
## [105] "denovo1335926" "denovo92200"   "denovo1439298" "denovo1136955"
## [109] "denovo362320"  "denovo821128"  "denovo340592"  "denovo132060" 
## [113] "denovo1676026" "denovo1552894" "denovo367396"  "denovo705407" 
## [117] "denovo613215"  "denovo980138"  "denovo596415"  "denovo431938" 
## [121] "denovo1155118" "denovo1051221" "denovo495293"  "denovo1327678"
## [125] "denovo275492"  "denovo3743"    "denovo57741"   "denovo640768" 
## [129] "denovo357473"  "denovo1367007" "denovo953277"  "denovo583661" 
## [133] "denovo1050490" "denovo287142"  "denovo1552954" "denovo1258897"
## [137] "denovo313457"  "denovo112009"  "denovo972185"  "denovo987767" 
## [141] "denovo789478"  "denovo1148983" "denovo578894"  "denovo1559283"
## [145] "denovo1460014" "denovo1074506" "denovo37306"   "denovo871329" 
## [149] "denovo107193"  "denovo731156"  "denovo700434"  "denovo54572"  
## [153] "denovo1059460" "denovo1233649" "denovo1500688" "denovo1731240"
## [157] "denovo1754679" "denovo1092785" "denovo8306"    "denovo1347779"
## [161] "denovo1071845" "denovo1079196" "denovo547070"  "denovo1290952"
## [165] "denovo1529768" "denovo976096"  "denovo710744"  "denovo1461141"
## [169] "denovo1139397" "denovo1302275" "denovo1535638" "denovo1774296"
## [173] "denovo1461144" "denovo961112"  "denovo1252064" "denovo112921" 
## [177] "denovo1096821" "denovo617238"  "denovo179042"  "denovo995949" 
## [181] "denovo223078"  "denovo713681"  "denovo586582"  "denovo859771" 
## [185] "denovo1585536" "denovo577174"  "denovo1743922" "denovo1220278"
## [189] "denovo1232694" "denovo313835"  "denovo939613"  "denovo628944" 
## [193] "denovo519765"  "denovo1732409" "denovo890427"  "denovo208223" 
## [197] "denovo204305"  "denovo399503"  "denovo1195151" "denovo210630" 
## [201] "denovo238836"  "denovo1683673" "denovo311908"  "denovo1670264"
## [205] "denovo1652942" "denovo948950"  "denovo214246"  "denovo752875" 
## [209] "denovo729359"  "denovo240207"  "denovo19500"   "denovo12834"  
## [213] "denovo1423212" "denovo30040"   "denovo918504"  "denovo242761" 
## [217] "denovo413039"  "denovo244825"  "denovo944764"  "denovo339048" 
## [221] "denovo1054936" "denovo1232973" "denovo83894"   "denovo1703852"
## [225] "denovo167137"  "denovo460993"  "denovo927915"  "denovo640126" 
## [229] "denovo1022436" "denovo973136"  "denovo64259"   "denovo108791" 
## [233] "denovo1333307" "denovo1699786" "denovo232859"  "denovo1405641"
## [237] "denovo462870"  "denovo381677"  "denovo1065681" "denovo406816" 
## [241] "denovo570630"  "denovo1765786" "denovo1461216" "denovo622191" 
## [245] "denovo1505621" "denovo862239"  "denovo1497950" "denovo600105" 
## [249] "denovo1178554" "denovo606764"  "denovo785776"  "denovo1177908"
## [253] "denovo303383"  "denovo548548"  "denovo1683444" "denovo911239" 
## [257] "denovo1768855" "denovo376664"  "denovo1538816" "denovo1087758"
## [261] "denovo47776"   "denovo823941"  "denovo847190"  "denovo1529151"
## [265] "denovo1320467" "denovo318553"  "denovo1695008" "denovo1343764"
## [269] "denovo1349216" "denovo1237631" "denovo998467"  "denovo823489" 
## [273] "denovo106727"  "denovo1272072" "denovo653391"  "denovo354012" 
## [277] "denovo1178830" "denovo1507662" "denovo1453849" "denovo942340" 
## [281] "denovo962257"  "denovo216907"  "denovo1293918" "denovo1153492"
## [285] "denovo1685089" "denovo215704"  "denovo1528055" "denovo1176276"
## [289] "denovo115454"  "denovo497520"  "denovo359549"  "denovo1542464"
## [293] "denovo99211"   "denovo1716540" "denovo643908"  "denovo677744" 
## [297] "denovo72082"   "denovo558840"  "denovo679037"  "denovo632179" 
## [301] "denovo719291"  "denovo941447"  "denovo1015897" "denovo1467138"
## [305] "denovo1250134" "denovo397529"  "denovo687790"  "denovo1039858"
## [309] "denovo729674"  "denovo111172"  "denovo872506"  "denovo1583745"
## [313] "denovo1033522" "denovo146302"  "denovo466416"  "denovo509244" 
## [317] "denovo222342"  "denovo1110758" "denovo408510"  "denovo1096322"
## [321] "denovo1774843" "denovo1286340" "denovo741397"  "denovo218779" 
## [325] "denovo1678605" "denovo913777"  "denovo644756"  "denovo687380" 
## [329] "denovo699027"  "denovo530703"  "denovo1645364" "denovo1569122"
## [333] "denovo1080387" "denovo1475368" "denovo459882"  "denovo745164" 
## [337] "denovo1492122" "denovo995988"  "denovo101614"  "denovo1512634"
## [341] "denovo890215"  "denovo996083"  "denovo739082"  "denovo1576950"
## [345] "denovo145444"  "denovo382952"  "denovo779594"  "denovo1612226"
## [349] "denovo1284749" "denovo705886"  "denovo1181752" "denovo1475075"
## [353] "denovo1054295" "denovo1067515" "denovo638268"  "denovo844470" 
## [357] "denovo738152"  "denovo306685"  "denovo6317"    "denovo8451"   
## [361] "denovo1295886" "denovo69297"   "denovo1696629" "denovo1717952"
## [365] "denovo347057"  "denovo976547"  "denovo271827"  "denovo1175688"
## [369] "denovo933360"  "denovo733843"  "denovo1136265" "denovo1165463"
## [373] "denovo138381"  "denovo196566"  "denovo397425"  "denovo1313586"
## [377] "denovo451179"  "denovo1175979" "denovo1056352" "denovo265127" 
## [381] "denovo408643"  "denovo188365"  "denovo809663"  "denovo1163184"
## [385] "denovo332144"  "denovo247315"  "denovo575062"  "denovo144556" 
## [389] "denovo477685"  "denovo165695"  "denovo785264"  "denovo1262347"
## [393] "denovo685205"  "denovo1552925" "denovo707290"  "denovo1298770"
## [397] "denovo1018459" "denovo138059"  "denovo253976"  "denovo1313102"
## [401] "denovo891026"  "denovo137490"  "denovo1371028" "denovo774726" 
## [405] "denovo375754"  "denovo460667"  "denovo1058719" "denovo45065"  
## [409] "denovo1613402" "denovo1086824" "denovo1313859" "denovo1746456"
## [413] "denovo89864"   "denovo229728"  "denovo97117"   "denovo1174421"
## [417] "denovo986055"  "denovo217215"  "denovo482943"  "denovo835034" 
## [421] "denovo1615999" "denovo1150659" "denovo245577"  "denovo152547" 
## [425] "denovo1523982" "denovo1534924" "denovo405473"  "denovo615882" 
## [429] "denovo1652040" "denovo584445"  "denovo699555"  "denovo1109803"
## [433] "denovo446277"  "denovo936348"  "denovo1335061" "denovo1517706"
## [437] "denovo1411383" "denovo920092"  "denovo1023951" "denovo79715"  
## [441] "denovo567309"  "denovo1187970" "denovo727204"  "denovo734068" 
## [445] "denovo512166"  "denovo359249"  "denovo299014"  "denovo346201" 
## [449] "denovo1766501" "denovo374726"  "denovo1503354" "denovo1412898"
## [453] "denovo1205230" "denovo830054"  "denovo238601"  "denovo563079" 
## [457] "denovo179676"  "denovo457114"  "denovo873379"  "denovo693009" 
## [461] "denovo1021067" "denovo969193"  "denovo596265"  "denovo1209920"
## [465] "denovo234357"  "denovo1097540" "denovo1136432" "denovo1116310"
## [469] "denovo1698512" "denovo892158"  "denovo1104325" "denovo218871" 
## [473] "denovo615410"  "denovo234644"  "denovo664517"  "denovo673139" 
## [477] "denovo573201"  "denovo758150"  "denovo450350"  "denovo531235" 
## [481] "denovo1561556" "denovo106416"  "denovo823403"  "denovo1686600"
## [485] "denovo1043235" "denovo679041"  "denovo1752632" "denovo972717" 
## [489] "denovo935772"  "denovo174506"  "denovo1176308" "denovo436604" 
## [493] "denovo923953"  "denovo1705190" "denovo1139261" "denovo405907" 
## [497] "denovo917147"  "denovo216673"  "denovo56629"   "denovo1490008"
## [501] "denovo1415833" "denovo822087"  "denovo1503755" "denovo573723" 
## [505] "denovo1771794" "denovo1347389" "denovo478577"  "denovo1105298"
## [509] "denovo1596201" "denovo1728999" "denovo261480"  "denovo713248" 
## [513] "denovo841352"  "denovo108286"  "denovo943678"  "denovo474854" 
## [517] "denovo1751598" "denovo1267424" "denovo254412"  "denovo865960" 
## [521] "denovo1288696" "denovo82772"   "denovo619116"  "denovo654221" 
## [525] "denovo910204"  "denovo1423671" "denovo138014"  "denovo97271"  
## [529] "denovo675769"  "denovo481967"  "denovo1163950" "denovo1528590"
## [533] "denovo648570"  "denovo903920"  "denovo1771"    "denovo1743137"
## [537] "denovo105247"  "denovo56387"   "denovo527272"  "denovo109433" 
## [541] "denovo1243482" "denovo959665"  "denovo10251"   "denovo1072049"
## [545] "denovo729303"  "denovo1317798" "denovo1166873" "denovo1280072"
## [549] "denovo1271874" "denovo825766"  "denovo420540"  "denovo327345" 
## [553] "denovo796529"  "denovo481865"  "denovo67547"   "denovo314604" 
## [557] "denovo1738705" "denovo167976"  "denovo1729652" "denovo431287" 
## [561] "denovo1276143" "denovo1035554" "denovo239149"  "denovo1650918"
## [565] "denovo1315276" "denovo1226541" "denovo1740963" "denovo1398738"
## [569] "denovo540550"  "denovo1335313" "denovo715755"  "denovo1368433"
## [573] "denovo967862"  "denovo1578213" "denovo538587"  "denovo450396" 
## [577] "denovo1770579" "denovo641807"  "denovo687378"  "denovo1636353"
## [581] "denovo328770"  "denovo494178"  "denovo16671"   "denovo1161608"
## [585] "denovo1017121" "denovo1318688" "denovo411887"  "denovo78338"  
## [589] "denovo1009229" "denovo1431269" "denovo1335774" "denovo260258" 
## [593] "denovo639769"  "denovo1150016" "denovo1168620" "denovo176047" 
## [597] "denovo765982"  "denovo218736"  "denovo1456562" "denovo314706" 
## [601] "denovo247345"  "denovo1545916" "denovo53072"   "denovo1749264"
## [605] "denovo1125940" "denovo41211"   "denovo1611098" "denovo676750" 
## [609] "denovo634545"  "denovo487203"  "denovo263359"  "denovo146677" 
## [613] "denovo1341013" "denovo1092487" "denovo654595"  "denovo342552" 
## [617] "denovo1693865" "denovo386175"  "denovo883892"  "denovo1124197"
## [621] "denovo1685813" "denovo390151"  "denovo1184876" "denovo1038783"
## [625] "denovo1432559" "denovo1314698" "denovo27866"   "denovo1123220"
## [629] "denovo820521"  "denovo1542305" "denovo927705"  "denovo1313525"
## [633] "denovo1449522" "denovo123619"  "denovo302768"  "denovo483793" 
## [637] "denovo1610657" "denovo705580"  "denovo1253982" "denovo429101" 
## [641] "denovo552478"  "denovo1107328" "denovo193923"  "denovo376347" 
## [645] "denovo50186"   "denovo306787"  "denovo1005478" "denovo1020187"
## [649] "denovo15018"   "denovo39714"   "denovo1745263" "denovo227591" 
## [653] "denovo1624753" "denovo644627"  "denovo1453872" "denovo1773782"
## [657] "denovo81259"   "denovo1004776" "denovo820680"  "denovo291310" 
## [661] "denovo1609857" "denovo48690"   "denovo194663"  "denovo1125712"
## [665] "denovo1042395" "denovo649906"  "denovo787261"  "denovo77358"  
## [669] "denovo1113012" "denovo572733"  "denovo788648"  "denovo1034960"
## [673] "denovo1052578" "denovo212898"  "denovo641235"  "denovo1279814"
## [677] "denovo126615"  "denovo506636"  "denovo1662736" "denovo1190320"
## [681] "denovo876045"  "denovo634541"  "denovo289706"  "denovo1645785"
## [685] "denovo1699672" "denovo1445616" "denovo803922"  "denovo965749" 
## [689] "denovo360530"  "denovo221595"  "denovo542158"  "denovo782845" 
## [693] "denovo433548"  "denovo1627677" "denovo1501661" "denovo677593" 
## [697] "denovo800085"  "denovo924512"  "denovo1639701" "denovo674246" 
## [701] "denovo1201117" "denovo1342525" "denovo116245"  "denovo1344540"
## [705] "denovo473165"  "denovo230078"  "denovo236264"  "denovo1554041"
## [709] "denovo452452"  "denovo1394722" "denovo1274267" "denovo921920" 
## [713] "denovo1132099" "denovo1055844" "denovo893785"  "denovo117036" 
## [717] "denovo515629"  "denovo555188"  "denovo537882"  "denovo715649" 
## [721] "denovo440246"  "denovo149499"  "denovo776067"  "denovo833995" 
## [725] "denovo130763"  "denovo441204"  "denovo671657"  "denovo538939" 
## [729] "denovo123063"  "denovo1722052" "denovo1244569" "denovo709050" 
## [733] "denovo908069"  "denovo788219"  "denovo148938"  "denovo266029" 
## [737] "denovo641162"  "denovo8353"    "denovo166169"  "denovo992379" 
## [741] "denovo1403026" "denovo1505078" "denovo522499"  "denovo255400" 
## [745] "denovo1839"    "denovo446373"  "denovo211774"  "denovo187499" 
## [749] "denovo1699894" "denovo1164326" "denovo1169421" "denovo1303800"
## [753] "denovo900707"  "denovo1573922" "denovo901751"  "denovo836801" 
## [757] "denovo1263363" "denovo420584"  "denovo273696"  "denovo1774145"
## [761] "denovo947304"  "denovo110250"  "denovo941686"  "denovo1512655"
## [765] "denovo342782"  "denovo1394529" "denovo156167"  "denovo1700013"
## [769] "denovo593555"  "denovo389888"  "denovo1492031" "denovo1256782"
## [773] "denovo1046430" "denovo1435953" "denovo1412797" "denovo1497249"
## [777] "denovo144840"  "denovo1329751" "denovo1569256" "denovo998375" 
## [781] "denovo610818"  "denovo957233"  "denovo1102125" "denovo1214863"
## [785] "denovo263817"  "denovo343785"  "denovo1467860" "denovo198683" 
## [789] "denovo12064"   "denovo546403"  "denovo1405429" "denovo967870" 
## [793] "denovo823624"  "denovo622692"  "denovo1452027" "denovo424018" 
## [797] "denovo443738"  "denovo425906"  "denovo118524"  "denovo980632" 
## [801] "denovo1489074" "denovo827476"  "denovo964183"  "denovo361813" 
## [805] "denovo1029448" "denovo166077"  "denovo1149285" "denovo628447" 
## [809] "denovo731739"  "denovo1621002" "denovo1061371" "denovo872374" 
## [813] "denovo1597310" "denovo1035858" "denovo1610647" "denovo1503795"
## [817] "denovo212882"  "denovo1129749" "denovo1527235" "denovo107725" 
## [821] "denovo1642066" "denovo1670578" "denovo1085993" "denovo1300741"
## [825] "denovo1595544" "denovo162850"  "denovo1045187" "denovo360099" 
## [829] "denovo473501"  "denovo511471"  "denovo1426472" "denovo333338" 
## [833] "denovo1314927" "denovo1181818" "denovo78625"   "denovo126656" 
## [837] "denovo810436"  "denovo67708"   "denovo1436140" "denovo658397" 
## [841] "denovo783532"  "denovo1141822" "denovo612914"  "denovo1637122"
## [845] "denovo1639443" "denovo658540"  "denovo1088265" "denovo1022919"
## [849] "denovo49469"   "denovo879658"  "denovo1481256" "denovo389651" 
## [853] "denovo1017354" "denovo761386"  "denovo1343535" "denovo1541972"
## [857] "denovo841594"  "denovo1728702" "denovo1016452" "denovo1758699"
## [861] "denovo1285128" "denovo1326239" "denovo964876"  "denovo1196857"
## [865] "denovo618296"  "denovo230815"  "denovo197307"  "denovo1743661"
## [869] "denovo534830"  "denovo1449451" "denovo544210"  "denovo578138" 
## [873] "denovo704636"  "denovo1174816" "denovo37686"   "denovo851887" 
## [877] "denovo852855"  "denovo1226852" "denovo799324"  "denovo1243679"
## [881] "denovo722999"  "denovo1344311" "denovo1584725" "denovo1248852"
## [885] "denovo819829"  "denovo455805"  "denovo1402334" "denovo1376127"
## [889] "denovo795220"  "denovo622150"  "denovo1661952" "denovo349426" 
## [893] "denovo1749422" "denovo1200917" "denovo1256096" "denovo917537" 
## [897] "denovo201249"  "denovo1033116" "denovo70915"   "denovo1624810"
## [901] "denovo656814"  "denovo1202391" "denovo503508"  "denovo1460498"
## [905] "denovo274616"  "denovo1443040" "denovo596008"  "denovo1696985"
## [909] "denovo1613140" "denovo1529033" "denovo1070540" "denovo1023971"
## [913] "denovo24194"   "denovo121064"  "denovo1422247" "denovo702032" 
## [917] "denovo1331591" "denovo397352"  "denovo1004473" "denovo233019" 
## [921] "denovo609657"  "denovo351245"  "denovo719603"  "denovo314244" 
## [925] "denovo1624290" "denovo1699261" "denovo1519708" "denovo664806" 
## [929] "denovo1227029" "denovo1010966" "denovo1533748" "denovo234158" 
## [933] "denovo1603338" "denovo1426597" "denovo1312012" "denovo1490071"
## [937] "denovo1517725" "denovo1212946" "denovo1180700" "denovo449785" 
## [941] "denovo168495"  "denovo1756865" "denovo523962"  "denovo922097" 
## [945] "denovo1366123" "denovo1164416" "denovo1200504" "denovo291238" 
## [949] "denovo194835"  "denovo410821"  "denovo276664"  "denovo267040" 
## [953] "denovo768092"  "denovo1011969" "denovo709006"  "denovo1651470"
## [957] "denovo1393676" "denovo947416"  "denovo1117218" "denovo1183566"
```

```r
colnames(origin_data)
```

```
##  [1] "os1"  "os2"  "os3"  "os4"  "os5"  "os6"  "os7"  "os8"  "os9"  "os10"
## [11] "os11" "os12"
```

```r
gtbl <- tbl_df(origin_data)
gtbl
```

```
## Source: local data frame [960 x 12]
## 
##      os1    os2    os3    os4    os5   os6    os7   os8   os9   os10  os11
##    (int)  (int)  (int)  (int)  (int) (int)  (int) (int) (int)  (int) (int)
## 1  65781 166746 133838 114072 116996 53608 103203 32723 56342 112139 36434
## 2   6220  12708   3240   7716  25435 20694   2675  9560  7871  13210  3173
## 3   3572    762   1622    138   1546  2523   2239 11451  7209    770 14906
## 4   1863    174    799   1465     80   410     83 19998 17405    155  1187
## 5    239    456   1095    832    271  1598   4953 15650  1020  10638  4509
## 6    632    899   2425    321   2153  3401   3386 11040  4394   1705  9532
## 7   2239   1571   1246   2280   1378  1585   2691  7173  2064   5373  6013
## 8   2038    962   4931    243   1093  5390   1232  2677  3516     34  2188
## 9   3537   1218   1098   7172    286  2444   4012  2223  1243    101  1448
## 10  9216    669    216    110   2709   705    129  2059    65    245  9157
## ..   ...    ...    ...    ...    ...   ...    ...   ...   ...    ...   ...
## Variables not shown: os12 (int)
```
## normalized columns
based on the total reads of each sample

```r
# clip total reads from excel, than paste into R
refs <- read.delim("clipboard", header = TRUE)
```

```
## Warning in read.table(file = file, header = header, sep = sep, quote =
## quote, : incomplete final line found by readTableHeader on 'clipboard'
```

```r
refs %>% glimpse
```

```
## Observations: 0
## Variables: 1
## $ clipboard (lgl)
```

clipboard as below:

SeqName	SmpName	TtlReads
os1	h0618	494604
os2	h0625	311167
os3	h0702	493566
os4	s0618	470232
os5	s0625	547637
os6	s0702	490970
os7	h0730	501020
os8	h0806	507057
os9	h0813	478260
os10	s0806	545820
os11	s0813	496962
os12	s0820	554586

