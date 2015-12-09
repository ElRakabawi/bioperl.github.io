---
title: FASTA sequence format
layout: default
---

Description
-----------

One of the oldest and simplest sequence formats. A single header line followed by 1 or more sequence lines.

Examples
--------

A sequence database with two protein sequences in FASTA format. The description line after the `>` is totally free-form, although applications often assume the first string after the `>` symbol is a sequence identifier of some sort. Traditionally the sequence lines are limited to a width of 60 characters, but this is not guaranteed.

```
>CATH_RAT
MWTALPLLCAGAWLLSAGATAELTVNAIEKFHFTSWMKQHQKTYSSREYSHRLQVFANNWRKIQAHNQRN
HTFKMGLNQFSDMSFAEIKHKYLWSEPQNCSATKSNYLRGTGPYPSSMDWRKKGNVVSPVKNQGACGSCW
TFSTTGALESAVAIASGKMMTLAEQQLVDCAQNFNNHGCQGGLPSQAFEYILYNKGIMGEDSYPYIGKNG
QCKFNPEKAVAFVKNVVNITLNDEAAMVEAVALYNPVSFAFEVTEDFMMYKSGVYSSNSCHKTPDKVNHA
VLAVGYGEQNGLLYWIVKNSWGSNWGNNGYFLIERGKNMCGLAACASYPIPQV
>CATL_HUMAN
MNPTLILAAFCLGIASATLTFDHSLEAQWTKWKAMHNRLYGMNEEGWRRAVWEKNMKMIELHNQEYREGK
HSFTMAMNAFGDMTSEEFRQVMNGFQNRKPRKGKVFQEPLFYEAPRSVDWREKGYVTPVKNQGQCGSCWA
FSATGALEGQMFRKTGRLISLSEQNLVDCSGPQGNEGCNGGLMDYAFQYVQDNGGLDSEESYPYEATEES
CKYNPKYSVANDTGFVDIPKQEKALMKAVATVGPISVAIDAGHESFLFYKEGIYFEPDCSSEDMDHGVLV
VGYGFESTESDNNKYWLVKNSWGEEWGMGGYVKMAKDRRNHCGIASAASYPTV

```

An NCBI formatted sequence header which includes *g*enBank-*i*dentifier number `142864`, accession number `M10040.1`, and Locus name `BACDNAE`. This sequence was first submitted to the GenBank database as described by the `gb` prefixing the accession number. Other abbreviaions include `emb` for EMBL Database or `pdb` for PDB Database.

```
>gi|142864|gb|M10040.1|BACDNAE B.subtilis dnaE gene encoding DNA primase, complete cds
GTACGACGGAGTGTTATAAGATGGGAAATCGGATACCAGATGAAATTGTGGATCAGGTGCAAAAGTCGGC
AGATATCGTTGAAGTCATAGGTGATTATGTTCAATTAAAGAAGCAAGGCCGAAACTACTTTGGACTCTGT
CCTTTTCATGGAGAAAGCACACCTTCGTTTTCCGTATCGCCCGACAAACAGATTTTTCATTGCTTTGGCT
GCGGAGCGGGCGGCAATGTTTTCTCTTTTTTAAGGCAGATGGAAGGCTATTCTTTTGCCGAGTCGGTTTC
TCACCTTGCTGACAAATACCAAATTGATTTTCCAGATGATATAACAGTCCATTCCGGAGCCCGGCCAGAG
TCTTCTGGAGAACAAAAAATGGCTGAGGCACATGAGCTCCTGAAGAAATTTTACCATCATTTGTTAATAA
ATACAAAAGAAGGTCAAGAGGCACTGGATTATCTGCTTTCTAGGGGCTTTACGAAAGAGCTGATTAATGA
ATTTCAGATTGGCTATGCTCTTGATTCTTGGGACTTTATCACGAAATTCCTTGTAAAGAGGGGATTTAGT
GAGGCGCAAATGGAAAAAGCGGGTCTCCTGATCAGACGCGAAGACGGAAGCGGATATTTCGACCGCTTCA
GAAACCGTGTCATGTTTCCGATCCATGATCATCACGGGGCTGTTGTTGCTTTCTCAGGCAGGGCTCTTGG

```

Different version of Fasta
----

Note that there is no formal definition for the header line, so `>CATL_HUMAN` and `>gi|7733636|ref|NP_887744 Gadget protein` are both valid. NCBI has one format, Swissprot another, and so on. Therefore BioPerl has no guaranteed way of knowing where names, accessions, and particular identifiers are in the header line. 

File extensions
---------------

There are no standard file extensions for FASTA formatted files. However, common ones are `.fa` and `.fsa`. [NCBI](http://www.ncbi.nlm.nih.gov/) distributes their genomic data in FASTA format using four different extensions: `.fna` for whole genomic DNA sequences, `.faa` for protein coding sequences (CDS), `.ffn` for the untranslated nucleotide sequences for each CDS, and `.frn` for nucleotide sequences of RNA related features.


