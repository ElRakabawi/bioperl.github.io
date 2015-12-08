---
title: NEXUS multiple alignment format
layout: default
---

NEXUS format
------------

The *NEXUS format* is used by software such as MrBayes and PAUP. It is not just a simple multiple alignment format, it can also contain a wealth of meta data regarding the alignment as well as commands for a program reading the NEXUS file to execute.

The NEXUS file format is comprised a "blocks" such as the taxa block, data block, sets block, trees block, PAUP block and MrBayes block, to name a few. Each block starts with `begin <block name>;` and ends with `end;`.

Software reading the NEXUS format should be able to process the file and skip any blocks that it does not understand. In writing a NEXUS file, data should be written to a "block" which is most appropriate and follows the convention set down by Madison before writing the data to a new type of block. For example, MrBayes currently requires TAXSETs to be defined in the MrBayes block. However, the appropriate place for these definitions in the SETS block so they are available to other NEXUS file readers.

Software commands can be included in a block which is to be read only by the intended software. This is usually done so as to allow batch processing of files by executing many commands in succession without the need to sit at the computer and enter them manually. For example the PAUP block and MrBayes block contain commands pertinent to their respective software.

Two main parts of the NEXUS file, which are describe in more detail below, are the Alignment and the Tree sections.

### Alignment

```
#NEXUS
[TITLE: NoName]
begin data;
dimensions ntax=3 nchar=384;
format interleave datatype=protein   gap=- symbols="FSTNKEYVQMCLAWPHDRIG";
matrix
CYS1_DICDI          -----MKVIL LFVLAVFTVF VSS------- --------RG IPPEEQ---- 
ALEU_HORVU          MAHARVLLLA LAVLATAAVA VASSSSFADS NPIRPVTDRA ASTLESAVLG 
CATH_HUMAN          ------MWAT LPLLCAGAWL LGV------- -PVCGAAELS VNSLEK---- 
CYS1_DICDI          --------SQ FLEFQDKFNK KY-SHEEYLE RFEIFKSNLG KIEELNLIAI 
ALEU_HORVU          ALGRTRHALR FARFAVRYGK SYESAAEVRR RFRIFSESLE EVRSTN---- 
CATH_HUMAN          --------FH FKSWMSKHRK TY-STEEYHH RLQTFASNWR KINAHN---- 
CYS1_DICDI          NHKADTKFGV NKFADLSSDE FKNYYLNNKE AIFTDDLPVA DYLDDEFINS 
ALEU_HORVU          RKGLPYRLGI NRFSDMSWEE FQATRL-GAA QTCSATLAGN HLMRDA--AA 
CATH_HUMAN          NGNHTFKMAL NQFSDMSFAE IKHKYLWSEP QNCSAT--KS NYLRGT--GP 
 
CYS1_DICDI          IPTAFDWRTR G-AVTPVKNQ GQCGSCWSFS TTGNVEGQHF ISQNKLVSLS 
ALEU_HORVU          LPETKDWRED G-IVSPVKNQ AHCGSCWTFS TTGALEAAYT QATGKNISLS 
CATH_HUMAN          YPPSVDWRKK GNFVSPVKNQ GACGSCWTFS TTGALESAIA IATGKMLSLA 
CYS1_DICDI          EQNLVDCDHE CMEYEGEEAC DEGCNGGLQP NAYNYIIKNG GIQTESSYPY 
ALEU_HORVU          EQQLVDCAGG FNNF------ --GCNGGLPS QAFEYIKYNG GIDTEESYPY 
CATH_HUMAN          EQQLVDCAQD FNNY------ --GCQGGLPS QAFEYILYNK GIMGEDTYPY 
CYS1_DICDI          TAETGTQCNF NSANIGAKIS NFTMIP-KNE TVMAGYIVST GPLAIAADAV 
ALEU_HORVU          KGVNGV-CHY KAENAAVQVL DSVNITLNAE DELKNAVGLV RPVSVAFQVI 
CATH_HUMAN          QGKDGY-CKF QPGKAIGFVK DVANITIYDE EAMVEAVALY NPVSFAFEVT 
CYS1_DICDI          E-WQFYIGGV F-DIPCN--P NSLDHGILIV GYSAKNTIFR KNMPYWIVKN 
ALEU_HORVU          DGFRQYKSGV YTSDHCGTTP DDVNHAVLAV GYGVENGV-- ---PYWLIKN 
CATH_HUMAN          QDFMMYRTGI YSSTSCHKTP DKVNHAVLAV GYGEKNGI-- ---PYWIVKN 
CYS1_DICDI          SWGADWGEQG YIYLRRGKNT CGVSNFVSTS II-- 
ALEU_HORVU          SWGADWGDNG YFKMEMGKNM CAIATCASYP VVAA 
CATH_HUMAN          SWGPQWGMNG YFLIERGKNM CGLAACASYP IPLV 

```


### Tree

```
BEGIN TREES; 
       TRANSLATE
               1       Struthio_camelus,
               2       Rhea_americana,
               3       Pterocnemia_pennata,
               4       Casuarius_casuarius,
               5       Dromaius_novaehollandiae,
               6       Nothoprocta_cinerascens,
               7       Eudromia_elegans,
               8       Pygoscelis_adeliae_f,
               9       Pygoscelis_adeliae_y,
               10      Spheniscus_humboldti,
               11      Phalacrocorax_sulcirostris,
               12      Anhinga_novaehollandeae,
               13      Nycticorax_nycticorax,
               14      Chauna_chavaria,
               15      Anseranas_semipalmata,
               16      Dendrocygna_arcuata,
               17      Dendrocygna_autumnalis,
               18      Dendrocygna_eytoni_d,
               19      Dendrocygna_eytoni_e,
               20      Dendrocygna_viduata,
               21      Coscoroba_coscoroba,
               22      Cygnus_atratus,
               23      Goose,
               24      Anser_indicus,
               25      Branta_canadensis,
               26      Cereopsis_novaehollandiae,
               27      Chloephaga_picta,
               28      Duck,
               29      Anas_platyrhynchos,
               30      Megapodius_freycinet,
               31      Leipoa_ocellata,
               32      Ortalis_vetula,
               33      Penelope_jacquacu,
               34      Penelope_superciliaris,
               35      Bonasa_umbellus,
               36      Tympanuchus_cupido,
               37      Oreortyx_pictus,
               38      Callipepla_squamata_n,
               39      Callipepla_squamata_s,
               40      Lophortyx_californicus,
               41      Colinus_virginianus,
               42      Cyrtonyx_montezumae_l,
               43      Cyrtonyx_montezumae_s,
               44      Alectoris_chukar,
               45      Alectoris_rufa,
               46      Francolinus_afer,
               47      Francolinus_erckelii,
               48      Francolinus_coqui_v,
               49      Francolinus_coqui_a,
               50      Francolinus_francolinus_a,
               51      Francolinus_francolinus_v,
               52      Francolinus_pondicerianus,
               53      Perdix_perdix,
               54      Coturnix_delegorguei,
               55      Coturnix_coturnix_japonica_1,
               56      Coturnix_coturnix_japonica_2,
               57      Arborophilia_torqueola,
               58      Bambusicola_thoracica,
               59      Tragopan_satyra,
               60      Tragopan_temmincki,
               61      Lophophorus_impejanus,
               62      Crossoptilon_auritum,
               63      Lophura_edwardsi,
               64      Lophura_ignita,
               65      Gallus_gallus,
               66      Grey_jungle_fowl,
               67      Phasianus_colchicus,
               68      Syrmaticus_ellioti,
               69      Syrmaticus_reevesii,
               70      Chrysolophus_amherstiae,
               71      Polyplectron_bicalcaratum,
               72      Argusianus_argus_argus,
               73      Pavo_cristatus,
               74      Afropavo_congensis,
               75      Numida_meleagris,
               76      Acryllium_vulturinum,
               77      Meleagris_gallopavo,
               78      Grus_carunculatus,
               79      Anthropoides_virgo,
               80      Grus_vipio,
               81      Fulica_atra,
               82      Vanellus_spinosus,
               83      Larus_rudibundus,
               84      Turnix_sylvatica,
               85      Gallirallus_australis,
               86      Geococcyx_californianus,
               87      Dacelo_novaeguineae,
               88      Carpococcyx_renauldi,
               89      Podargus_strigoides
       ;
       TREE  * PAUP_1 =  [&R] (1,(((2,3),(4,5)),((((((((((6,7),((30,31),(((32,33),34),(((((((35,57),((((53,67),70),(62,(63,64),(68,69))),
(((54,(55,56)),84),74))),(((46,(48,49)),47),71)),((36,59),60),(61,(75,76))),(72,73),77),((44,45),((((50,51),58),52),(65,66)))),
(((37,((38,39),40)),41),(42,43)))))),14),15),((((16,20),(18,19)),17),((((21,26),(27,(28,29))),22),((23,24),25)))),(78,79,80)),87),
((81,85),(82,83))),(8,9,(10,((11,(86,88)),13),12))),89)));
END;

```

Reference
---------

http://www.ncbi.nlm.nih.gov/pubmed/11975335
