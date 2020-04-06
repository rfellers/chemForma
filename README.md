# chemForma
We would like to create a Chemical Formula Specification for Mass Spectrometry that standardizes how chemical compositions are written.

## Places this would be used
1. UniProt's Controlled vocabulary of posttranslational modifications (ptmlist.txt)
2. Unimod
3. PSI-MOD
4. ProForma

## Goals of this project
1. Create a unified specification for a chemical formula to be written as a single line of ASCII text
2. Human readable, machine parsable, and optimized for the simple and common cases (e.g. H2O)
3. Handle specific MS use cases
   * Glycan atom support
   * Isotope support
   * Negative cardinalities

## Do we need that? Isn't this already settled?
Given the goals above, it seems like something should already exist for this. But in my searching, I've found only the following:

* Hill Convention

   The well known [Hill Convention](https://en.wikipedia.org/wiki/Chemical_formula#Hill_system) introduced using element symbols, cardinality, and specific ordering to chemical formulas. However, for MS, we desire additional features.

* InChI

   [InChI](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-015-0068-4) has a required base layer for a chemical formula, but it simply uses Hill convention and doesn't go any further.

* SMILES

   [SMILES](https://en.wikipedia.org/wiki/Simplified_molecular-input_line-entry_system) is another common format, but only encodes structure and doesn't have a place for chemical formula.

## What do MS modification repositories do?

* UniProt (ptmlist.txt)

   UniProt maintains a list of modifications used in its protein entries called [ptmlist.txt](https://www.uniprot.org/docs/ptmlist). This document encodes a correction formula and appears to simply be Hill Convention with additional support for negative cardinalities and extra spaces between the elements.

   `H2 O`

   `H-7 N-1 O1`

* Unimod

   Unimod lists a database field called [Composition](http://www.unimod.org/fields.html) that is the delta between modified and unmodified. This format handles glycans and certain isotopes as named 'atoms', yet requires all cardinalities be wrapped in paranthesis.

   `H(2) O`

   `H(-7) C(-3) N(-1) S(-1)`

   `C(-2) 13C(2) N(-2) 15N(2)`

   `Ac Hex HexNAc NeuAc(2)`

* PSI-MOD (and RESID)

   After a survey of terms, [PSI-MOD](https://www.ebi.ac.uk/ols/ontologies/mod) appears use Hill Convention, while handling isotopes using a prefix wrapped in paranthesis. There was no inherant glycan support as full elemental formulas were given. Also, spaces were used extensively: they separate elements from each other as well as from cardinalities. 

   `H 2 O`

   `C 0 H 0 N 0 O 0 S -1 Se 1`

   `(12)C 8 (13)C 4 H 20 (14)N 1 (15)N 1 O 2`

## Our Proposal
Given that nothing accomplishes all the goals laid out above, we must simply accept an existing format or propose the creation of a new standard.

### Adopt Unimod's Composition structure
The Unimod composition field is the only approach that covers enough of the requirements to consider adoption. We would need to create a specification document and get buy-in from all relevant parties.

--- __OR__ ---

### Create a new standard: chemForma
Making a new standard could make adoption more difficult, but it would allow input from the community and yield a (hopefully) better result. We would start with the following rules:

#### Rule 1
A formula will be composed of pairs of atoms and their corresponding cardinality. Pairs *may* be separated by spaces, but are not required to be. Also, Hill convention for ordering is perferred, but not required.

`C12H20O2`

#### Rule 2
Atoms will consist of all known periodic elements as well as known glycan residues (likely a subset from [here](https://www.ncbi.nlm.nih.gov/glycans/snfg.html), we will defer to glycan experts). If glycan symbols conflict with themselves or element symbols in such a way that ambiguities occur, we will consider requiring spaces between 'atoms' (see Rule #1).

`Hex2 Man`

#### Rule 3
Cardinalities must be positive or negative integer values. Zero is not supported. If a cardinality is not included with an atom, it is assumed to be 1.

`HN-1O2`

#### Rule 4
Isotopes will be handled by prefixing the atom with its isotopic number in parenthesis.

`(13)C2 (12)C-2 H2 N`
