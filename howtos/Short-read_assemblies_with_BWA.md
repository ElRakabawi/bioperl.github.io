---
title: "HOWTO:Short Read assemblies using bwa"
layout: default
---

Abstract
--------

Using BioPerl to create and manipulate short-read assemblies with the [`bwa`](http://bio-bwa.sourceforge.net/) and [`samtools`](http://www.htslib.org/) suites.

[Quicklink](#synopsis) to synopsis.

Author
------

Mark A. Jensen

[Fortinbras Research](http://fortinbras.us)

`maj -at- fortinbras -dot- us`

Introduction
------------

[`bwa`](http://bio-bwa.sourceforge.net/) is a suite of C programs that perform efficient alignments (based in part on the [Burrows-Wheeler transform](http://en.wikipedia.org/wiki/Burrows-Wheeler_transform)) of short (20-100bp) sequence reads, guided by a set of reference sequences provided in FASTA format. `bwa` input and output complies with the Sequence/Alignment Map ([SAM](http://www.htslib.org/doc/sam.html)) binary (`.bam`) and test (`.sam`) formats. Alignment files in SAM formats can be converted, indexed, sliced and diced by the [`samtools`](http://www.htslib.org/) suite.

These tools are comprehensive and allow the user to tweak many different parameters, and outputs can be directed to inputs to create highly application-specific workflows. The BioPerl run wrappers [Bio::Tools::Run::BWA](https://metacpan.org/pod/Bio::Tools::Run::BWA) and [Bio::Tools::Run::Samtools](https://metacpan.org/pod/Bio::Tools::Run::Samtools)
are designed to help automate and manage such workflows, and help reduce the number of cryptic command-line options and file order inconsistencies the user must remember. [Bio::Tools::Run::BWA](https://metacpan.org/pod/Bio::Tools::Run::BWA)
can also act as a canned assembler, accepting input data and returning a [Bio::Assembly::IO](https://metacpan.org/pod/Bio::Assembly::IO) object in a single command. A new assembly IO object has been created for this purpose, [Bio::Assembly::IO::sam](https://metacpan.org/pod/Bio::Assembly::IO::sam).

Dependencies and Installation
-----------------------------

[Bio::Tools::Run::BWA](https://metacpan.org/pod/Bio::Tools::Run::BWA) and additional dependencies in the Bio::Tools::Run namespace can be found in the [bioperl-run](https://github.com/bioperl/bioperl-run) repository. 

Like all run wrappers, these modules need the underlying programs to work; visit the links above to acquire them.

Also, the [Bio::DB::Sam](https://metacpan.org/pod/Bio::DB::Sam)
modules must be installed on your system. These are _not_ BioPerl modules. (They were written by a core developer, though, so they're all right.) You can get them on [CPAN](http://metacpan.org).

Creating an Assembly
--------------------

The `run()` method of `Bio::Tools::Run::BWA` creates an assembly in sorted `.bam` format, using reads in `FASTQ` format and a reference sequence database in `FASTA`. Create a `BWA` factory, then call `run` from it with your files as arguments:

```perl
$bwa = Bio::Tools::Run::BWA->new()
$bwa->out_type('asm.sam'); # specify output file
# for single-end reads:
$bwa->run( 'read1.fastq', 'refdb.fas' )
# for paired-end reads:
$bwa->run( 'read1.fastq', 'refdb.fas', 'read2.fastq');
```

If you have [Bio::Tools::Run::Samtools](https://metacpan.org/pod/Bio::Tools::Run::Samtools) installed, the output file (`asm.sam`) will already be sorted and converted to BAM.

## Getting an assembly object

The `BWA::run()` method will also create a [Bio::Assembly::Scaffold](https://metacpan.org/pod/Bio::Assembly::Scaffold) object, containing [Bio::Assembly::Contig](https://metacpan.org/pod/Bio::Assembly::Contig) alignments with associated consensus sequence objects, quality data, and sequence features.

To do this, leave `out_type` unset above, and capture the return value of `run()`:

```perl
$bwa = Bio::Tools::Run::BWA->new()
# for single-end reads:
$aio = $bwa->run( 'read1.fastq', 'refdb.fas' )
# for paired-end reads:
$aio = $bwa->run( 'read1.fastq', 'refdb.fas', 'read2.fastq');
```

## The assembly pipeline

The `Bio::Tools::Run::BWA::run()` method performs the following steps:

| Action                              | Program  | Commands      |
|-------------------------------------|----------|---------------|
| create a bwa index for ref seq      | bwa      | `index`       |
| map sequence reads to reference seq | bwa      | `aln`         |
| assemble                            | bwa      | `samse/sampe` |
| sort on coordinates                 | samtools | `sort`        |
| create bam index                    | samtools | `index`       |

Command-line options can be directed to the `aln` and `samse/sampe` steps using factory arguments. See [Specifying Options](#specifying-options).

Running separate bwa components
-------------------------------

A second mode for [Bio::Tools::Run::BWA](https://metacpan.org/pod/Bio::Tools::Run::BWA) allows direct access to `bwa` commands. To run a command, construct a run factory, specifying the desired command using the `-command` argument in the factory constructor, along with options specific to that command (see [Specifying Options](#specifying-options)):

```perl
$bwa = Bio::Tools::Run::BWA->new( -command => 'view' );
```

To execute, use the `run_bwa()` method off the factory. Input and output files are specified in the arguments of `run_maq` (see [Specifying Files](#specifying-files)):

```perl
$bwa->run_bwa( -bam=>'mysam.bam' -out=>'mysam.sam' );
```

## Specifying options

`bwa` is complex, with many subprograms (commands) and command-line options and file specs for each. The wrapper module attempts to provide commands and options comprehensively. You can browse the choices like so:

```perl
$bwa = Bio::Tools::Run::BWA->new( -command => 'aln' );
# all bwa commands
@all_commands = $bwa->available_parameters('commands'); 
@all_commands = $bwa->available_commands; # alias
# just for aln
@assemble_params = $bwa->available_parameters('params');
@assemble_switches = $bwa->available_parameters('switches');
@assemble_all_options = $bwa->available_parameters();
```

Reasonably mnemonic names have been assigned to the single-letter command line options. These are the names returned by `available_parameters`, and can be used in the factory constructor like typical BioPerl named parameters.

Options can be directed to the `aln` and `samse/sampe` components of the assembly pipeline implemented by the `run()` method. Identify the desired options, for example

```perl
@map_params = Bio::Tools::Run::Maq->new(-command => 'aln')->available_parameters();
 
# returns:
#  'max_edit_dist'
#  'max_gap_opens'
#  'max_gap_extns'
#  'deln_protect_3p'
#  'deln_protect_ends'
#  'subseq_seed'
#  'max_edit_dist_seed'
#  'n_threads'
#  'mm_penalty'
#  'gap_open_penalty'
#  'gap_extn_penalty'
#  'subopt_hit_threshold'
#  'trim_parameter'
#  'reverse_no_comp'
#  'no_iter_search'
```

then in the factory construction, specify the desired parameters prefixed by `aln_`, `sms_`, or `smp_`, as appropriate (note, *no* `-command` parameter is specified in the `run()` method):

```perl
$bwa = Bio::Tools::Run::BWA->new( -aln_n_threads => 3 );
$assy = $bwa->run( "read1.fastq", "refseq.fas" );
```

See the `bwa` [manpage](http://bio-bwa.sourceforge.net/bwa.shtml) for many gory details.

## Specifying files

When a command requires filenames, these are provided to the `run_bwa` method, not the constructor (`new()`). To see the set of files required by a command, use `available_parameters(\'filespec\')` or the alias `filespec()`.

```perl
$bwa = Bio::Tools::Run::BWA->new( -command => 'aln' );
@filespec = $bwa->filespec;
```

This example returns the following array:

```
fas
faq
>sai
```

This indicates that the FASTA database (fas) and the FASTQ reads (faq) MUST be specified, and the STDOUT of this program (SA coordinates) will be slurped into a file specified in the `run_bwa` argument list:

```perl
$bwa->run_bwa( -fas => 'my.db.fas', -faq => 'reads.faq',
               -sai => 'out.sai' );
```

If capture files are not specified per the filespec, text sent to STDOUT and STDERR is saved and is accessible with `$bwa->stdout()` and `$bwa->stderr()`.

Running samtools components
---------------------------

By an odd coincidence, the [Bio::Tools::Run::Samtools](https://metacpan.org/pod/Bio::Tools::Run::Samtools) wrapper accesses the `samtools` commands in a similar way. Create a `Samtools` factory, specifying the command line options in the constructor argument:

```perl
$converter = Bio::Tools::Run::Samtools->new(
  -command => 'view',
  -sam_input => 1,
  -bam_output => 1
);
```

and run using the `run()`, specifying necessary files in its arguments:

```perl
$converter->run( -bam => 'mysam.sam', -out => 'mysam.bam' );
```

Parameters and filespecs can be browsed as described in [Specifying options](#specifying-options) and [Specifying files](#specifying-files).

Synopsis
--------

```perl
# create a single-read assembly object
$bwa = Bio::Tools::Run::BWA->new();
$assy = $bwa->run( 'read1.fastq', 'refseqs.fas' );
# create a paired-read assembly object
$assy_prd = $bwa->run( 'read1.fastq', 'refseqs.fas', 'read2.fastq' );
 
# create just the sorted, binary SAM file
$bwa->out_type( 'assy_prd.bam' );
$bwa->run( 'read1.fastq', 'refseqs.fas', 'read2.fastq' );
 
# extract regions from the assy
$start1 = 150000;
$end1   = 200000;
$start2 = 250000;
$end2   = 275000;
$samt = Bio::Tools::Run::Samtools->new( 
  -command => 'view',
  -bam_output => 1
  );
$samt->run( -bam => 'assy_prd.bam', 
            -rgn => [ "my_seqid:$start1\-$end1",
                      "my_seqid:$start2\-$end2" ],
            -out => 'assy_rgns.bam'
            );
 
# convert a text SAM to binary format
$samt = Bio::Tools::Run::Samtools->new(
  -command => 'view',
  -sam_input => 1,
  -bam_output => 1
  );
$samt->run( -bam => 'mysam.sam', -out => 'mysam.bam' );
 
# sort it
$samt = Bio::Tools::Run::Samtools->new(
  -command => 'sort'
  );
# creates 'mysam.srt.bam':
$samt->run( -bam => 'mysam.bam', -pfx => 'mysam.srt' );
```

SEE ALSO
--------

[Short-read assemblies with `maq`](./Short-read_assemblies_with_maq.html)

