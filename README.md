# Itility Data Science Assessment
## Roman Nekrasov
Welkom in deze repo waar ik de data science assessment heb uitgewerkt. De vragen worden onderstaand schriftelijk beantwoord. In de Jupyter notebook Itility.ipynb zijn de technische aspecten en stappen te vinden die de antwoorden aanvullen. Deze kan in de browser via GitHub bekeken worden.

### Advies downsizen VMs
Er zijn bruikbare meetgegevens aangeleverd van 4259 unieke VMs over een periode van ongeveer 2 maanden. De CPU workload van deze VMs zijn geanalyseerd en hieruit is gebleken dat 1228 VMs oversized zijn en gedownscaled kunnen worden. De lijst met VMs kan gevonden worden in de map **Data**. De volgende stappen zijn genomen om tot deze conclusie te komen:

- De ondergrens van de CPU workload is gedefinieerd op 20%. Als een VM gedurende de hele meetperiode onder 20% vermogen draait dan wordt deze gelabeld als oversized. Deze defenitie is variabel en kan worden aangepast naar wens. Niet alleen in ondergrens, maar ook pieken kunnen worden toegelaten in bepaalde mate waar de ondergrens gepasseerd wordt.
- Voor elk meetmoment van elke VM wordt een label gemaakt of de ondergrens wordt overschreden of niet.
- Voor elke VM wordt gekeken of gedurende de meetperiode de ondergrens gepasseerd is. Zo niet, dan krijgt deze het label oversized.
- De lijst van VMs met label: 1, voor oversized en label: 0, anders, is geëxporteerd naar een CSV bestand. Deze is te vinden in de map **Data**.

### Vreemde zaken
Een aantal opvallende dingen zijn naar voorgekomen tijdens het analyseren van de data.

- De numerieke variablen **MemoryMB**,	**CpuMHz** en **NumCpu** bevatten velden waar dubbele waardes in stonden, bijvoorbeeld: "5600 8200". Dit waren in totaal enkele honderden rijen. Zonder context is het lastig vast te stellen waarom dit zo was. By design, of een fout in de export? Geziend de grote van de data set zijn deze rijen verwijderd voor de verdere analyse.
- Voor het **MetricID: MemActive** is niet duidelijk welke eenheid gebruikt is om het memory gebruik te meten. Vrijwel alle meetwaardes overstijgen de corresponderende **MemoryMB**. 
- Uit de analyse is ook gebleken dat een aantal VMs constant tegen de 100% CPU workload zit. Dit kan duiden op een runaway proces en moet mogelijk worden onderzocht.
- Er zijn een aantal outliers te vinden waar het gemeten CPU verbruik een stuk hoger ligt dan de grote van de grootste CPU. Dit zou mogelijk kunnen komen door pieken waar de VM van op hol slaat of een fout in de export.

### Machine Learning 🤖
Wat is er verder nodig om deze analyse automatisch te laten verrichten door middel van Machine Learning?
Door de uitgevoerde analyse is er data set ontstaan die voor elke VM aangeeft of deze oversized is of niet.
We hebben nu dus een gelabelde data set en kunnen supervised learning toepassen. Het label kan 1: oversized, of 0: niet oversized, zijn. 
De data kan gesplitst worden in train en test data en er kunnen verschillende modellen getrained en geëvalueerd worden die binaire classificatie verichten.
Als hier een goed presterend model uitkomt kan hier een api van gemaakt worden en worden toegevoegd aan het export script. Zo wordt er automatisch een advies bijgezet.

