
# Antigenic annotation pipeline

This pipeline was created to predict and annotate potential vaccine candidate proteins from draft Schistosome genomes.

<br /> <br /> <br /> 
Problems: 
* Some genomes of medically important parasites are in draft form and lack functional annotation that inhibits the screeing of potential vaccine candidates.
* When faced with multiple draft genomes it can be beyond the scope of a single project to annotate multiple genomes for comparative studies.

Solution: 
* `Antigenic_annotation.sh` predicts and annotates single protein queries from draft genomes and provides protein annotation of transmembrane regions, conserved domains and antigenic regions.<br /> <br /> <br /> 


      Usage:  ./Antigenic_annotation.sh  /path/to/genomes/ protein_query best_n  gene_id  translation_table


<br /> <br /> <br /> 
<br /> <br /> <br /> 
<br /> <br /> <br /> 

## Part I
* Predicts best scoring coding-exon sequences from target genome/s using exonerate protein2genome model
* Extracts coding-exon sequence from exonerate output using extract_exonerate.py (see Sequence-tools repository)
* Gets the highest scoring coding-exon alignment and translates it using Transeq (EMBOSS) (highest scoring is predicted using best_n 1 in exonerate)
* Uses longest_orf.py (see Sequence-tools) to predict the longest open reading frame.
* Stores the prediction from each species/genome in a single mf file.<br /> <br /> <br /> 

![alt text](https://github.com/camilla-eldridge/Antigenic-annotation-pipeline/blob/main/diagram/part_I.png)

<br /> <br /> <br /> 

## Part II
* Aligns all proteins from the mf file (Output of Part I).
* Runs Bepipred, Tmhmm and a CDD search on each protein (after separating the mf file using mulif_to_singlef.py, see Sequence-tools) 
* Aligns antigenicity predition scores to the sequence alignment using align_bepipred_scores.py (see Sequence-tools)
* Plots aligned antigenic scores in R (see R-plots repository).<br /> <br /> <br /> 


![alt text](https://github.com/camilla-eldridge/Antigenic-annotation-pipeline/blob/main/diagram/part_II.png)


<br /> <br /> <br /> 
# Notes on usage and requirements 

This pipeline relies on a number of python scripts (found in Sequence-tools repository). To run Antigenic-annotation.sh you will need:
 * extract_exonerate.py
 * longest_orf.py
 * align_bepipred_scores.py
 * cdd_search.sh
 * multif_to_singlef.py
 * Ag_plot.R

All scripts mentioned above and the following programs need to be made available to $PATH.
 * Exonerate (tested with v2.4.0).
 * Transeq (from emboss v6.6.0.0)
 * cowsay (v3.03)
 * Bepipred (tested with v1.0)
 * Clustalo (tested with V1.2.4)


<br /> <br /> <br />

# Notes on personalisation
 
* If you want to change the axes in the plot the easiest way is to edit them in align_bepipred_scores.py at:
            `out.write("Sequence" + "," + "AA" + "," + "Ag" + "," + "code" + "\n" + "\n".join(final))`
            
* At the moment the ids of the predicted sequences cannot be edited.

* Target genomes need to have the .fna extension, if you want to change this alter the extension in the line: `for f in *.fna*;do` in `Antigenic-annotation.sh`.
<br /> <br /> <br /> 


# Notes and improvement:

On gene models and repeats<br /> <br /> <br /> 
It is important to know before hand the gene model of your protein so that you can manually check the predictions. <br /> <br /> <br /> 
Unlike in whole genome annotation, there is no step in this pipeline to remove repeat regions from the draft genome before prediction, this is a work in progress. 
<br /> <br /> <br /> 
To rectify this you can generate a species specific repeat library (using RepArk and/or Repeatmasker) and BLAST the coding-exon predictions against this library  to see if any repeat regions have been included in their predictions. <br /> <br /> <br /> 
This pipeline can also be run on cDNA datasets; and by using Transdecoder you can separate out the long coding transcripts then use these to check the validity of your gene model i.e are the number of exons predicted in the expressed transcript (as predicted only from coding transcripts) the same as in the coding-exon prediction.
<br /> <br /> <br /> 










