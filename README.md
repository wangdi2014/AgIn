##Installation
    $ git clone git://github.com/hacone/AgIn.git AgIn

Demo files are available at

http://www.gi.k.u-tokyo.ac.jp/~hacone/Demo.tar.gz

##Analyse your data
AgIn tries to detect CpG methylated/unmethylated regions on a specified reference genome from PacBio sequencing data of read coverage as low as 20x.
The program requires a `modification.csv` file as a primary input, which is produced by `P_ModificationDetection` module of SMRT Pipe, with your intended reference file (use the same reference file throughout the analysis, for SMRT Pipe and for AgIn).
Then run as:

    $ target/dist/bin/launch -i path/to/modifications.csv -f path/to/reference.fasta -o output_prefix -b vector_specifier -g gamma -l min_length predict

It'll give three output files:

    output_prefix.gff
    output_prefix_class.wig
    output_prefix_coverage.wig

The program assumes the index file for the reference (`.fai` generated by samtools) is placed in the same directory as the reference.


####The parameters for prediction
`-b` : File containing a vector for decision hyperplane. For P6-C4, use `resource/P6C4.dat`. For old data, either `P5C3_HdrR` (for P5-C3 sequencing), or `LDAVector` (for P4-C2 and C2-C2 sequencing). 

`-l` : Minimum number of CpG sites the predicted regions must contain. Smaller value gives you better resolution with possibly worse accuracy. ()

`-g` : Decision threshold which controls the sensitivity/specificity trade-off.

Recommended (standard) combinations of parameters are:
    -b resource/P6C4.dat -g -0.55 -l 40 (for P6-C4)
    -b P5C3_HdrR -g -0.88 -l 40 (for P5-C3)
    -b LDAVector -g -1.80 -l 40 (for P4-C2 or C2-C2)


## Output files
The GFF file reports all the predicted hypomethylated regions.

`_class.wig` file describes the predicted class (0 for unmethylated, 1 for methylated) of each CpG site.

`_coverage.wig` file describes the effective coverage used for making the prediction. (i.e., # of IPD ratio measurements on 21 bases around the CpG site / 21.0 )

## Lisence

Copyright of this program `AgIn` is held by Yuta Suzuki
(ysuzuki@cb.k.u-tokyo.ac.jp (effective to 3/31/2017), esperhacone@gmail.com) and is distributed under the (modified) BSD license.

Copyright of `src/script/launch` and some other scripts is held by Taro L.
Saito(leo@xerial.org) and licensed under Apache License Version 2.0
(http://www.apache.org/licenses/LICENSE-2.0)
