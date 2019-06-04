Untitled
================

The PDB database
----------------

The [PDB](http://www.rcsb.org/) is the main repository for biomolecular structure data.

Here we examine the contents of the PDB:

> Q1 Download a CSV file from the PDB site (accessible from “Analyze” -&gt; “PDB Statistics” &gt; “by Experimental Method and Molecular Type”. Move this CSV file into your RStudio project and determine the percentage of structures solved by X-Ray and Electron Microscopy. From the website what proportion of structures are protein? Aim to have a rendered GitHub document with working code that yields your answers.

``` r
db <- read.csv("Data Export Summary.csv", row.names=1)
head(db)
```

    ##                     Proteins Nucleic.Acids Protein.NA.Complex Other  Total
    ## X-Ray                 126880          2012               6547     8 135447
    ## NMR                    11062          1279                259     8  12608
    ## Electron Microscopy     2277            31                800     0   3108
    ## Other                    256             4                  6    13    279
    ## Multi Method             129             5                  2     1    137

What percentage are X-Ray?

``` r
db$Total[1]/(sum(db$Total)) * 100
```

    ## [1] 89.35736

What percentage are Electron Microscopy?

``` r
db$Total[3]/(sum(db$Total)) * 100
```

    ## [1] 2.050416

What percentage are Protein?

``` r
(sum(db$Proteins))/(sum(db$Total)) * 100
```

    ## [1] 92.75955

Using the datapasta package to copy paste any table into R without needing a .csv file

``` r
library(datapasta)

# copy table from site, and go to add-ins and paste as data frame
# can paste any data table from any website, even if it doesn't have a .csv with it

tmp <- data.frame(stringsAsFactors=FALSE,
   Experimental.Method = c("X-Ray", "NMR", "Electron Microscopy", "Other",
                           "Multi Method", "Total"),
              Proteins = c(126880, 11062, 2277, 256, 129, 140604),
         Nucleic.Acids = c(2012, 1279, 31, 4, 5, 3331),
    ProteinComplex = c(6547, 259, 800, 6, 2, 7614),
                 Other = c(8, 8, 0, 13, 1, 30),
                 Total = c(135447, 12608, 3108, 279, 137, 151579)
)

View(tmp)
```

> Q2 Type HIV in the PDB website search box on the home page and determine how many HIV-1 protease structures are in the current PDB? There are 1157 structures as of 2019-05-07 (<http://www.rcsb.org/pdb/results/results.do?tabtoshow=Current&qrid=ADAE5411>)

Now working with Bio3D and pdb
------------------------------

``` r
library(bio3d)
example(plot.bio3d)
```

    ## 
    ## plt.b3> ## No test: 
    ## plt.b3> ##D # PDB server connection required - testing excluded
    ## plt.b3> ##D 
    ## plt.b3> ##D ## Plot of B-factor values along with secondary structure from PDB
    ## plt.b3> ##D pdb <- read.pdb( "1bg2" )
    ## plt.b3> ##D bfac <- pdb$atom[pdb$calpha,"b"]
    ## plt.b3> ##D plot.bio3d(bfac, sse=pdb, ylab="B-factor", col="gray")
    ## plt.b3> ##D points(bfac, typ="l")
    ## plt.b3> ## End(No test)
    ## plt.b3> 
    ## plt.b3> ## Not run: 
    ## plt.b3> ##D ## Use PDB residue numbers and include short secondary structure elements
    ## plt.b3> ##D plot.bio3d(pdb$atom[pdb$calpha,"b"], sse=pdb, resno=pdb, ylab="B-factor",
    ## plt.b3> ##D   typ="l", lwd=1.5, col="blue", sse.min.length=0)
    ## plt.b3> ##D 
    ## plt.b3> ##D 
    ## plt.b3> ##D ## Calculate secondary structure using stride() or dssp()
    ## plt.b3> ##D #sse <- stride(pdb)
    ## plt.b3> ##D sse <- dssp(pdb)
    ## plt.b3> ##D 
    ## plt.b3> ##D ## Plot of B-factor values along with calculated secondary structure
    ## plt.b3> ##D plot.bio3d(pdb$atom[pdb$calpha,"b"], sse=sse, ylab="B-factor", typ="l",
    ## plt.b3> ##D col="blue", lwd=2)
    ## plt.b3> ## End(Not run)
    ## plt.b3> 
    ## plt.b3> ## No test: 
    ## plt.b3> ##D # PDB server connection required - testing excluded
    ## plt.b3> ##D 
    ## plt.b3> ##D ## Plot 'aligned' data respecting gap positions
    ## plt.b3> ##D attach(transducin)
    ## plt.b3> ##D 
    ## plt.b3> ##D pdb = read.pdb("1tnd") ## Reference PDB see: pdbs$id[1]
    ## plt.b3> ##D pdb = trim.pdb(pdb, inds=atom.select(pdb, chain="A"))
    ## plt.b3> ##D 
    ## plt.b3> ##D ## Plot of B-factor values with gaps
    ## plt.b3> ##D plot.bio3d(pdbs$b, resno=pdb, sse=pdb, ylab="B-factor")
    ## plt.b3> ##D 
    ## plt.b3> ##D ## Plot of B-factor values after removing all gaps 
    ## plt.b3> ##D plot.bio3d(pdbs$b, rm.gaps=TRUE, resno = pdb, sse=pdb, ylab="B-factor")
    ## plt.b3> ##D 
    ## plt.b3> ##D detach(transducin)
    ## plt.b3> ## End(No test)
    ## plt.b3> 
    ## plt.b3> ## Fancy secondary structure elements
    ## plt.b3> ##plot.bio3d(pdb$atom[pdb$calpha,"b"], sse=pdb, ssetype="fancy")
    ## plt.b3> ## Currently not implemented
    ## plt.b3> 
    ## plt.b3> 
    ## plt.b3>

Reading a pdb file

``` r
pdb <- read.pdb("1hsg.pdb")
pdb
```

    ## 
    ##  Call:  read.pdb(file = "1hsg.pdb")
    ## 
    ##    Total Models#: 1
    ##      Total Atoms#: 1686,  XYZs#: 5058  Chains#: 2  (values: A B)
    ## 
    ##      Protein Atoms#: 1514  (residues/Calpha atoms#: 198)
    ##      Nucleic acid Atoms#: 0  (residues/phosphate atoms#: 0)
    ## 
    ##      Non-protein/nucleic Atoms#: 172  (residues: 128)
    ##      Non-protein/nucleic resid values: [ HOH (127), MK1 (1) ]
    ## 
    ##    Protein sequence:
    ##       PQITLWQRPLVTIKIGGQLKEALLDTGADDTVLEEMSLPGRWKPKMIGGIGGFIKVRQYD
    ##       QILIEICGHKAIGTVLVGPTPVNIIGRNLLTQIGCTLNFPQITLWQRPLVTIKIGGQLKE
    ##       ALLDTGADDTVLEEMSLPGRWKPKMIGGIGGFIKVRQYDQILIEICGHKAIGTVLVGPTP
    ##       VNIIGRNLLTQIGCTLNF
    ## 
    ## + attr: atom, xyz, seqres, helix, sheet,
    ##         calpha, remark, call

``` r
aa321(pdb$atom$resid)
```

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: MK1

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ## Warning in FUN(X[[i]], ...): Unknown 3-letters code for aminoacid: HOH

    ##    [1] "P" "P" "P" "P" "P" "P" "P" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "I"
    ##   [18] "I" "I" "I" "I" "I" "I" "I" "T" "T" "T" "T" "T" "T" "T" "L" "L" "L"
    ##   [35] "L" "L" "L" "L" "L" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W"
    ##   [52] "W" "W" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "R" "R" "R" "R" "R" "R"
    ##   [69] "R" "R" "R" "R" "R" "P" "P" "P" "P" "P" "P" "P" "L" "L" "L" "L" "L"
    ##   [86] "L" "L" "L" "V" "V" "V" "V" "V" "V" "V" "T" "T" "T" "T" "T" "T" "T"
    ##  [103] "I" "I" "I" "I" "I" "I" "I" "I" "K" "K" "K" "K" "K" "K" "K" "K" "K"
    ##  [120] "I" "I" "I" "I" "I" "I" "I" "I" "G" "G" "G" "G" "G" "G" "G" "G" "Q"
    ##  [137] "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "L" "L" "L" "L" "L" "L" "L" "L" "K"
    ##  [154] "K" "K" "K" "K" "K" "K" "K" "K" "E" "E" "E" "E" "E" "E" "E" "E" "E"
    ##  [171] "A" "A" "A" "A" "A" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L"
    ##  [188] "L" "L" "L" "L" "D" "D" "D" "D" "D" "D" "D" "D" "T" "T" "T" "T" "T"
    ##  [205] "T" "T" "G" "G" "G" "G" "A" "A" "A" "A" "A" "D" "D" "D" "D" "D" "D"
    ##  [222] "D" "D" "D" "D" "D" "D" "D" "D" "D" "D" "T" "T" "T" "T" "T" "T" "T"
    ##  [239] "V" "V" "V" "V" "V" "V" "V" "L" "L" "L" "L" "L" "L" "L" "L" "E" "E"
    ##  [256] "E" "E" "E" "E" "E" "E" "E" "E" "E" "E" "E" "E" "E" "E" "E" "E" "M"
    ##  [273] "M" "M" "M" "M" "M" "M" "M" "S" "S" "S" "S" "S" "S" "L" "L" "L" "L"
    ##  [290] "L" "L" "L" "L" "P" "P" "P" "P" "P" "P" "P" "G" "G" "G" "G" "R" "R"
    ##  [307] "R" "R" "R" "R" "R" "R" "R" "R" "R" "W" "W" "W" "W" "W" "W" "W" "W"
    ##  [324] "W" "W" "W" "W" "W" "W" "K" "K" "K" "K" "K" "K" "K" "K" "K" "P" "P"
    ##  [341] "P" "P" "P" "P" "P" "K" "K" "K" "K" "K" "K" "K" "K" "K" "M" "M" "M"
    ##  [358] "M" "M" "M" "M" "M" "I" "I" "I" "I" "I" "I" "I" "I" "G" "G" "G" "G"
    ##  [375] "G" "G" "G" "G" "I" "I" "I" "I" "I" "I" "I" "I" "G" "G" "G" "G" "G"
    ##  [392] "G" "G" "G" "F" "F" "F" "F" "F" "F" "F" "F" "F" "F" "F" "I" "I" "I"
    ##  [409] "I" "I" "I" "I" "I" "K" "K" "K" "K" "K" "K" "K" "K" "K" "V" "V" "V"
    ##  [426] "V" "V" "V" "V" "R" "R" "R" "R" "R" "R" "R" "R" "R" "R" "R" "Q" "Q"
    ##  [443] "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Y" "Y" "Y" "Y" "Y" "Y" "Y" "Y" "Y" "Y"
    ##  [460] "Y" "Y" "D" "D" "D" "D" "D" "D" "D" "D" "Q" "Q" "Q" "Q" "Q" "Q" "Q"
    ##  [477] "Q" "Q" "I" "I" "I" "I" "I" "I" "I" "I" "L" "L" "L" "L" "L" "L" "L"
    ##  [494] "L" "I" "I" "I" "I" "I" "I" "I" "I" "E" "E" "E" "E" "E" "E" "E" "E"
    ##  [511] "E" "I" "I" "I" "I" "I" "I" "I" "I" "C" "C" "C" "C" "C" "C" "G" "G"
    ##  [528] "G" "G" "H" "H" "H" "H" "H" "H" "H" "H" "H" "H" "K" "K" "K" "K" "K"
    ##  [545] "K" "K" "K" "K" "A" "A" "A" "A" "A" "I" "I" "I" "I" "I" "I" "I" "I"
    ##  [562] "G" "G" "G" "G" "T" "T" "T" "T" "T" "T" "T" "V" "V" "V" "V" "V" "V"
    ##  [579] "V" "L" "L" "L" "L" "L" "L" "L" "L" "V" "V" "V" "V" "V" "V" "V" "G"
    ##  [596] "G" "G" "G" "P" "P" "P" "P" "P" "P" "P" "T" "T" "T" "T" "T" "T" "T"
    ##  [613] "P" "P" "P" "P" "P" "P" "P" "V" "V" "V" "V" "V" "V" "V" "N" "N" "N"
    ##  [630] "N" "N" "N" "N" "N" "I" "I" "I" "I" "I" "I" "I" "I" "I" "I" "I" "I"
    ##  [647] "I" "I" "I" "I" "G" "G" "G" "G" "R" "R" "R" "R" "R" "R" "R" "R" "R"
    ##  [664] "R" "R" "N" "N" "N" "N" "N" "N" "N" "N" "L" "L" "L" "L" "L" "L" "L"
    ##  [681] "L" "L" "L" "L" "L" "L" "L" "L" "L" "T" "T" "T" "T" "T" "T" "T" "Q"
    ##  [698] "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "I" "I" "I" "I" "I" "I" "I" "I" "G"
    ##  [715] "G" "G" "G" "C" "C" "C" "C" "C" "C" "T" "T" "T" "T" "T" "T" "T" "L"
    ##  [732] "L" "L" "L" "L" "L" "L" "L" "N" "N" "N" "N" "N" "N" "N" "N" "F" "F"
    ##  [749] "F" "F" "F" "F" "F" "F" "F" "F" "F" "P" "P" "P" "P" "P" "P" "P" "Q"
    ##  [766] "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "I" "I" "I" "I" "I" "I" "I" "I" "T"
    ##  [783] "T" "T" "T" "T" "T" "T" "L" "L" "L" "L" "L" "L" "L" "L" "W" "W" "W"
    ##  [800] "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "Q" "Q" "Q" "Q" "Q" "Q"
    ##  [817] "Q" "Q" "Q" "R" "R" "R" "R" "R" "R" "R" "R" "R" "R" "R" "P" "P" "P"
    ##  [834] "P" "P" "P" "P" "L" "L" "L" "L" "L" "L" "L" "L" "V" "V" "V" "V" "V"
    ##  [851] "V" "V" "T" "T" "T" "T" "T" "T" "T" "I" "I" "I" "I" "I" "I" "I" "I"
    ##  [868] "K" "K" "K" "K" "K" "K" "K" "K" "K" "I" "I" "I" "I" "I" "I" "I" "I"
    ##  [885] "G" "G" "G" "G" "G" "G" "G" "G" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q"
    ##  [902] "L" "L" "L" "L" "L" "L" "L" "L" "K" "K" "K" "K" "K" "K" "K" "K" "K"
    ##  [919] "E" "E" "E" "E" "E" "E" "E" "E" "E" "A" "A" "A" "A" "A" "L" "L" "L"
    ##  [936] "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "D" "D" "D" "D"
    ##  [953] "D" "D" "D" "D" "T" "T" "T" "T" "T" "T" "T" "G" "G" "G" "G" "A" "A"
    ##  [970] "A" "A" "A" "D" "D" "D" "D" "D" "D" "D" "D" "D" "D" "D" "D" "D" "D"
    ##  [987] "D" "D" "T" "T" "T" "T" "T" "T" "T" "V" "V" "V" "V" "V" "V" "V" "L"
    ## [1004] "L" "L" "L" "L" "L" "L" "L" "E" "E" "E" "E" "E" "E" "E" "E" "E" "E"
    ## [1021] "E" "E" "E" "E" "E" "E" "E" "E" "M" "M" "M" "M" "M" "M" "M" "M" "S"
    ## [1038] "S" "S" "S" "S" "S" "L" "L" "L" "L" "L" "L" "L" "L" "P" "P" "P" "P"
    ## [1055] "P" "P" "P" "G" "G" "G" "G" "R" "R" "R" "R" "R" "R" "R" "R" "R" "R"
    ## [1072] "R" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "W" "K" "K"
    ## [1089] "K" "K" "K" "K" "K" "K" "K" "P" "P" "P" "P" "P" "P" "P" "K" "K" "K"
    ## [1106] "K" "K" "K" "K" "K" "K" "M" "M" "M" "M" "M" "M" "M" "M" "I" "I" "I"
    ## [1123] "I" "I" "I" "I" "I" "G" "G" "G" "G" "G" "G" "G" "G" "I" "I" "I" "I"
    ## [1140] "I" "I" "I" "I" "G" "G" "G" "G" "G" "G" "G" "G" "F" "F" "F" "F" "F"
    ## [1157] "F" "F" "F" "F" "F" "F" "I" "I" "I" "I" "I" "I" "I" "I" "K" "K" "K"
    ## [1174] "K" "K" "K" "K" "K" "K" "V" "V" "V" "V" "V" "V" "V" "R" "R" "R" "R"
    ## [1191] "R" "R" "R" "R" "R" "R" "R" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Y"
    ## [1208] "Y" "Y" "Y" "Y" "Y" "Y" "Y" "Y" "Y" "Y" "Y" "D" "D" "D" "D" "D" "D"
    ## [1225] "D" "D" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "I" "I" "I" "I" "I" "I"
    ## [1242] "I" "I" "L" "L" "L" "L" "L" "L" "L" "L" "I" "I" "I" "I" "I" "I" "I"
    ## [1259] "I" "E" "E" "E" "E" "E" "E" "E" "E" "E" "I" "I" "I" "I" "I" "I" "I"
    ## [1276] "I" "C" "C" "C" "C" "C" "C" "G" "G" "G" "G" "H" "H" "H" "H" "H" "H"
    ## [1293] "H" "H" "H" "H" "K" "K" "K" "K" "K" "K" "K" "K" "K" "A" "A" "A" "A"
    ## [1310] "A" "I" "I" "I" "I" "I" "I" "I" "I" "G" "G" "G" "G" "T" "T" "T" "T"
    ## [1327] "T" "T" "T" "V" "V" "V" "V" "V" "V" "V" "L" "L" "L" "L" "L" "L" "L"
    ## [1344] "L" "V" "V" "V" "V" "V" "V" "V" "G" "G" "G" "G" "P" "P" "P" "P" "P"
    ## [1361] "P" "P" "T" "T" "T" "T" "T" "T" "T" "P" "P" "P" "P" "P" "P" "P" "V"
    ## [1378] "V" "V" "V" "V" "V" "V" "N" "N" "N" "N" "N" "N" "N" "N" "I" "I" "I"
    ## [1395] "I" "I" "I" "I" "I" "I" "I" "I" "I" "I" "I" "I" "I" "G" "G" "G" "G"
    ## [1412] "R" "R" "R" "R" "R" "R" "R" "R" "R" "R" "R" "N" "N" "N" "N" "N" "N"
    ## [1429] "N" "N" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L" "L"
    ## [1446] "L" "T" "T" "T" "T" "T" "T" "T" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q" "Q"
    ## [1463] "I" "I" "I" "I" "I" "I" "I" "I" "G" "G" "G" "G" "C" "C" "C" "C" "C"
    ## [1480] "C" "T" "T" "T" "T" "T" "T" "T" "L" "L" "L" "L" "L" "L" "L" "L" "N"
    ## [1497] "N" "N" "N" "N" "N" "N" "N" "F" "F" "F" "F" "F" "F" "F" "F" "F" "F"
    ## [1514] "F" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1531] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1548] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1565] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1582] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1599] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1616] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1633] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1650] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1667] "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X" "X"
    ## [1684] "X" "X" "X"

Atom selection is done via the function **atom.select()**

``` r
prot.pdb <- atom.select(pdb, "protein", value=TRUE)
write.pdb(prot.pdb, file="1hsg_protein.pdb")
```

``` r
lig.pdb <- atom.select(pdb, "ligand", value=TRUE)
write.pdb(lig.pdb, file="1hsg.ligand.pdb")
```

Section 5 of the hands on worksheet

``` r
aa <- get.seq("1ake_A")
```

    ## Warning in get.seq("1ake_A"): Removing existing file: seqs.fasta

``` r
# Blast or hmmer search
b <- blast.pdb(aa)
```

    ##  Searching ... please wait (updates every 5 seconds) RID = FDF9AZGX014 
    ##  ..
    ##  Reporting 97 hits

``` r
# Plot a summary of search results
hits <- plot(b)
```

    ##   * Possible cutoff values:    197 -3 
    ##             Yielding Nhits:    12 97 
    ## 
    ##   * Chosen cutoff value of:    197 
    ##             Yielding Nhits:    12

![](class11_files/figure-markdown_github/unnamed-chunk-13-1.png)

``` r
head(hits$pdb.id)
```

    ## [1] "1AKE_A" "4X8M_A" "4X8H_A" "3HPR_A" "1E4V_A" "5EJE_A"

``` r
# Fetch PDBs
files <- get.pdb(hits$pdb.id, path="pdbs", split=TRUE, gzip=TRUE) 
```

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/1AKE.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/4X8M.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/4X8H.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/3HPR.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/1E4V.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/5EJE.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/1E4Y.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/3X2S.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/4K46.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/4NP6.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/3GMT.pdb.gz exists. Skipping download

    ## Warning in get.pdb(hits$pdb.id, path = "pdbs", split = TRUE, gzip = TRUE):
    ## pdbs/4PZL.pdb.gz exists. Skipping download

    ## 
      |                                                                       
      |                                                                 |   0%
      |                                                                       
      |=====                                                            |   8%
      |                                                                       
      |===========                                                      |  17%
      |                                                                       
      |================                                                 |  25%
      |                                                                       
      |======================                                           |  33%
      |                                                                       
      |===========================                                      |  42%
      |                                                                       
      |================================                                 |  50%
      |                                                                       
      |======================================                           |  58%
      |                                                                       
      |===========================================                      |  67%
      |                                                                       
      |=================================================                |  75%
      |                                                                       
      |======================================================           |  83%
      |                                                                       
      |============================================================     |  92%
      |                                                                       
      |=================================================================| 100%

``` r
# Align structures
pdbs <- pdbaln(files)
```

    ## Reading PDB files:
    ## pdbs/split_chain/1AKE_A.pdb
    ## pdbs/split_chain/4X8M_A.pdb
    ## pdbs/split_chain/4X8H_A.pdb
    ## pdbs/split_chain/3HPR_A.pdb
    ## pdbs/split_chain/1E4V_A.pdb
    ## pdbs/split_chain/5EJE_A.pdb
    ## pdbs/split_chain/1E4Y_A.pdb
    ## pdbs/split_chain/3X2S_A.pdb
    ## pdbs/split_chain/4K46_A.pdb
    ## pdbs/split_chain/4NP6_A.pdb
    ## pdbs/split_chain/3GMT_A.pdb
    ## pdbs/split_chain/4PZL_A.pdb
    ##    PDB has ALT records, taking A only, rm.alt=TRUE
    ## ...   PDB has ALT records, taking A only, rm.alt=TRUE
    ## ..   PDB has ALT records, taking A only, rm.alt=TRUE
    ## ...   PDB has ALT records, taking A only, rm.alt=TRUE
    ## ....
    ## 
    ## Extracting sequences
    ## 
    ## pdb/seq: 1   name: pdbs/split_chain/1AKE_A.pdb 
    ##    PDB has ALT records, taking A only, rm.alt=TRUE
    ## pdb/seq: 2   name: pdbs/split_chain/4X8M_A.pdb 
    ## pdb/seq: 3   name: pdbs/split_chain/4X8H_A.pdb 
    ## pdb/seq: 4   name: pdbs/split_chain/3HPR_A.pdb 
    ##    PDB has ALT records, taking A only, rm.alt=TRUE
    ## pdb/seq: 5   name: pdbs/split_chain/1E4V_A.pdb 
    ## pdb/seq: 6   name: pdbs/split_chain/5EJE_A.pdb 
    ##    PDB has ALT records, taking A only, rm.alt=TRUE
    ## pdb/seq: 7   name: pdbs/split_chain/1E4Y_A.pdb 
    ## pdb/seq: 8   name: pdbs/split_chain/3X2S_A.pdb 
    ## pdb/seq: 9   name: pdbs/split_chain/4K46_A.pdb 
    ##    PDB has ALT records, taking A only, rm.alt=TRUE
    ## pdb/seq: 10   name: pdbs/split_chain/4NP6_A.pdb 
    ## pdb/seq: 11   name: pdbs/split_chain/3GMT_A.pdb 
    ## pdb/seq: 12   name: pdbs/split_chain/4PZL_A.pdb

``` r
# Vector containing PDB codes
ids <- basename.pdb(pdbs$id)
# Draw schematic alignment
plot(pdbs, labels=ids)
```

![](class11_files/figure-markdown_github/unnamed-chunk-15-1.png)

``` r
# Calculate sequence conservation
cons <- conserv(pdbs, method="entropy22") 
# SSE annotations
sse <- pdbs2sse(pdbs, ind=1, rm.gaps=FALSE)
```

    ## Extracting SSE from pdbs$sse attribute

``` r
# Plot conservation per residue
plotb3(cons, sse=sse, ylab="Sequence entropy")
```

![](class11_files/figure-markdown_github/unnamed-chunk-16-1.png)

``` r
library(XML)
library(RCurl)
```

    ## Loading required package: bitops

``` r
anno <- pdb.annotate(ids) 
```

    ## Warning in pdb.annotate(ids): ids should be standard 4 character PDB-IDs:
    ## trying first 4 characters...

``` r
print(unique(anno$source))
```

    ## [1] "Escherichia coli"          "Photobacterium profundum" 
    ## [3] "Vibrio cholerae"           "Burkholderia pseudomallei"
    ## [5] "Francisella tularensis"
