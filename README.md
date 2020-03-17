# chemForma
Chemical Formula Specification for Mass Spectrometry 

## Goals of this project
1. Create a unified specification for a chemical formula to be written as a single line of ASCII text
2. Human readable, machine parsable, and optimized for the simple and common cases (e.g. H2O)
3. Handle specific MS use cases
   * Glycan atom support
   * Isotope support
   * Negative cardinalities

## Isn't this already settled?
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

* Unimod

   Unimod lists a database field called [Composition](http://www.unimod.org/fields.html) that is the delta between modified and unmodified. This format handles glycans and certain isotopes as named 'atoms', yet requires all cardinalities be wrapped in paranthesis.

* PSI-MOD (and RESID)

   After a survey of terms, [PSI-MOD](https://www.ebi.ac.uk/ols/ontologies/mod) appears use Hill Convention, while handling isotopes using a prefix wrapped in paranthesis. There was no inherant glycan support as full elemental formulas were given. Also, spaces were used extensively: they separate elements from each other as well as from cardinalities. 

## Our Proposal
Given that nothing else seems to fit the bill, we propose the creation of a new standard with the following rules:

### Rule 1
A formula will be composed of pairs of atoms and their corresponding cardinality. Pairs *may* be separated by spaces, but are not required to be.

### Rule 2
Atoms will consist of all known periodic elements as well as known glycan residues (__TBD__, find standard list)

### Rule 3
Cardinalities must be positive or negative integer values. Zero is not supported. If a cardinality is not included with an atom, it is assumed to be 1.

### Rule 4
Isotopes will be handled by prefixing the atom with its isotopic number.
