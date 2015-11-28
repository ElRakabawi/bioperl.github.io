---
title: "HOWTO:Local Databases"
layout: default
---

### Authors

[Brian Osborne](Brian_Osborne "wikilink"), [Peter Schattner](Peter_Schattner "wikilink").

### Abstract

This is a HOWTO that talks about using Bioperl to create local sequence databases for fast retrieval.

### Introduction

Bioperl offers many ways to retrieve sequences from online databases like NCBI and Swissprot but there may be times when you want build local databases for fast, secure retrieval. This HOWTO discusses the different Bioperl modules you might use. Also see [HOWTO:OBDA Flat databases](HOWTO:OBDA_Flat_databases "wikilink").

### Bio::Index

The following sequence data formats are supported by Bio::Index: , , , , , , , , , , and . Once the set of sequences have been indexed using Bio::Index\*, individual sequences can be accessed using syntax very similar to that used for accessing remote databases.

For example, if one wants to set up an indexed flat-file database of fasta files one could write a script like using :

```perl

\# script 1: create the index

use Bio::Index::Fasta; 
\# using fasta file format use strict; \# some users have reported 
\# that this is necessary

my $Index_File_Name = shift;

my $inx = Bio::Index::Fasta->new(
-filename => $Index_File_Name,
-write_flag => 1);

$inx-&gt;make_index(@sequence_files);

```

This script then retrieves sequences:

```perl

1.  script 2: retrieve some files

use Bio::Index::Fasta; use strict; \# some users have reported that this is necessary

my $Index_File_Name = shift;

my $inx = Bio::Index::Fasta-&gt;new($Index_File_Name);

foreach my $id (@ARGV) {

`   my $seq = $inx->fetch($id);  # Returns Bio::Seq object`
`   # do something with the sequence`

}

```

To facilitate the creation and use of more complex or flexible indexing systems, the Bioperl distribution includes two sample scripts in the scripts/index directory, bp_index.PLS and bp_fetch.PLS. These scripts can be used as templates to develop customized local data-file indexing systems.

### Bio::DB::Fasta

Bioperl also supplies as a means to index and query Fasta format files. It's similar in spirit to but offers more methods, e.g.

```perl

use Bio::DB::Fasta; use strict;

my $db = Bio::DB::Fasta-&gt;new($file); \# one file or many files my $seqstring = $db-&gt;seq($id); \# get a sequence as string my $seqobj = $db-&gt;get_Seq_by_id($id); \# get a PrimarySeq obj my $desc = $db-&gt;header($id); \# get the header, or description line

```

See for more information on this fully-featured module.

### Indexing using a specific substring

Both modules also offer the user the ability to designate a specific string within the fasta header as the desired id, such as the gi number within the string "gi|4556644|gb|X45555". Consider the following fasta-formatted sequence, in "test.fa":

```perl

&gt;gi|523232|emb|AAC12345|sp|D12567 titin fragment MHRHHRTGYSAAYGPLKJHGYVHFIMCVVVSWWASDVVTYIPLLLNNSSAGWKRWWWIIFGGE GHGHHRTYSALWWPPLKJHGSKHFILCVKVSWLAKKERTYIPKKILLMMGGWWAAWWWI

```

By default and will use the first "word" they encounter in the fasta header as the retrieval key, in this case "gi|523232|emb|AAC12345|sp|D12567". What would be more useful as a key would be a single id. The code below will index the "test.fa" file and create an index file called "test.fa.idx" where the keys are the Swissprot, or "sp", identifiers.

```perl

$ENV{BIOPERL_INDEX_TYPE} = "SDBM_File";

1.  look for the index in the current directory

$ENV{BIOPERL_INDEX} = ".";

my $file_name = "test.fa"; my $inx = Bio::Index::Fasta-&gt;new( -filename =&gt; $file_name . ".idx",

`                                 -write_flag => 1 );`

1.  pass a reference to the critical function to the Bio::Index object

$inx-&gt;id_parser(&get_id);

1.  make the index

$inx-&gt;make_index($file_name);

1.  here is where the retrieval key is specified

sub get_id {

`  my $header = shift;`
`  $header =~ /^>.*bsp|([A-Z]d{5}b)/;`
`  $1;`

}

```

Here is how you would retrieve the sequence, as a object:

```perl

my $seq = $inx-&gt;fetch("D12567"); print $seq-&gt;seq;

```

What if you wanted to retrieve a sequence using either a Swissprot id or a gi number and the fasta header was actually a concatenation of headers with multiple gi's and Swissprots?

```perl

&gt;gi|523232|emb|AAC12345|sp|D12567|gi|7744242|sp|V11223 titin fragment

```

Modify the function that's passed to the id_parser() method:

```perl

sub get_id {

`  my $header = shift;`
`  my (@sps) = $header =~ /^>.*bsp|([A-Z]d{5})b/g;`
`  my (@gis) = $header =~ /gi|(d+)b/g;`
`  return (@sps,@gis);`

}

```

The module uses the same principle, but the syntax is slightly different, for example:

```perl

my $db = Bio::DB::Fasta-&gt;new('test.fa', -makeid=&gt;&make_my_id); my $seqobj = $db-&gt;get_Seq_by_id($id);

sub make_my_id {

`  my $description_line = shift;`
`  $description_line =~ /gi|(d+)|emb|(w+)/;`
`  ($1,$2);`

}

```

### Storing sequences in a relational database

The core Bioperl package does not support accessing sequences and data stored in relational databases but this capability is available in the [Bioperl-db](Bioperl-db "wikilink") package.'

<Category:HOWTOs>
