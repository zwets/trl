# trl

_Translate nucleotide sequences to amino acids and back_


## Introduction

Trl is a tiny command-line tool that translates nucleotide sequences
to amino acid sequences, and vice versa.

`trl` needs no installation and has no requirements apart from Awk,
which is present on any respectable operating system.


## Usage

#### Examples

Pass the sequence to translate on the command-line

    $ trl agaatcgtagagcgt
    RIVER

Or talk with trl on stdin

    $ trl
    catgagttacctatggag
    HELPME
    tggcacgccacc
    WHAT

IUPAC codes for degenerate bases are allowed (with option `-d`)

    $ trl -d aaygarcgngay
    NERD

Input type is autodetected

    $ trl AMNESIC
    trl: input are amino acids; use option -3 to convert to TLA, or -r to reverse translate

Translate aa to 3-letter code

    $ trl -3 AMNESIC
    AlaMetAsnGluSerIleCys

Or reverse translate.  But, but, ... the central dogma?

    $ trl -r TAKE*THIS*CRICK
    ac.gc.aa[ag]ga[ag]ac.ca[ct]at[act](agc|agt|tc.)tg[ct](aga|agg|cg.)at[act]tg[ct]aa[ag]    

Reverse translation produces a regex(7) that matches every nt sequence
that (forward) codes for the aa sequence.  Not much practical use, but
but it made writing this _ArgGluAlaAspMetGlu_ surprisingly easy.


#### Manual

Use `trl --help` to get full usage instructions.

```
Usage: trl [OPTIONS] [SEQUENCE | FILE ...]

  Translate each line in each FILE from DNA/RNA to amino acids (AA), or
  reverse translate from AA to DNA.  Write the result to stdout.  If no FILE
  is specified, or FILE is '-', read from stdin.  If SEQUENCE is not a file
  name, then it is tried as a literal input sequence (as with -i).

  OPTIONS
   -i, --this=SEQ     Translate literal sequence SEQ, instead of from FILE
   -f, --frame=FRAME  Translate FRAME (-1, -2, -3, 1, 2, 3)
   -r, --reverse      Reverse translate AA to DNA
   -3, --tla          Write amino acids as three-letter abbrevations
   -a, --amino        Force interpret input as amino acids (when ambiguous)
   -d, --degenerate   Allow degenerate DNA alphabet (r, y, n, etc)
   -x, --use-X        Output X rather than abort on untranslatable codon

  Input can be mixed DNA and RNA sequences in upper or lowercase, or amino
  acid sequences in uppercase letters or as TLAs.  No spaces are allowed.
  Input type is auto-detected, or can be forced with -a when ambiguous.

  Each input line is translated on its own.  FASTA headers are allowed but
  file format must then be unfasta (https://github.com/zwets/unfasta), for
  the logical reason that each input line is translated on its own.

  Reverse translation produces, for a given AA sequence, a regular expression
  that matches every DNA sequence that (forward) translates to the amino acid
  sequence, or, with option -d, a sequence with degenerate bases (except for
  L, R, S, and the stop codon, for which this is impossible).
```

