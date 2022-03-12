# riceTElib
A collection of manually curated TE examplars in rice (Oryza sativa) provided by Ning Jiang and colleagues.  
The latest library, `ricex.x.x.liban`, is placed in the root folder. `x.x.x` is the version number. Older versions were placed in the `historic_versions` folder. Sequences contained in the library are non-redundant examplars that can be used to annotate all related TEs in the rice genome.  


# Commannds used to update the library

## Combine the SINE library with rice6.9.5.liban
### mask lib with new SINEs
`RepeatMasker -pa 36 -q -no_is -norna -nolow -div 40 -cutoff 225 -lib riceSINE_210517 rice6.9.5.liban`

### get non-SINE lib seqs
```bash
perl ~/bin/LTR_retriever/bin/output_by_list.pl 1 rice6.9.5.liban.masked 1 <(awk '{print $5}' rice6.9.5.liban.out) -FA -ex | \
	perl ~/bin/LTR_retriever/bin/output_by_list.pl 1 - 1 <(grep SINE rice6.9.5.liban) -FA -ex \
> rice6.9.5.liban.nosine
```

### get SINE-related non-SINE lib seqs
```bash
perl ~/bin/LTR_retriever/bin/output_by_list.pl 1 rice6.9.5.liban.masked 1 <(awk '{print $5}' rice6.9.5.liban.out|grep -v SINE) -FA \
> rice6.9.5.liban.masked.sine
```

### clean up SINE-masked lib seqs
`perl ~/bin/LTR_retriever/bin/cleanup.pl -misschar n -Nscreen 0 -minlen 80 -cleanN 1 -trf 0 -f rice6.9.5.liban.masked.sine > rice6.9.5.liban.masked.sine.cln`

### combine clean seqs and format
`cat rice6.9.5.liban.nosine rice6.9.5.liban.masked.sine.cln riceSINE_210517 | perl ~/bin/LTR_retriever/bin/fasta-reformat.pl - 50 > rice7.0.0.liban`

### check ID uniqueness
```bash
grep \> rice7.0.0.liban|sed 's/#.*//' |sort -u|wc -l
grep -c \> rice7.0.0.liban
```


## Citations
Different parts of this library were published in different studies:

### The LTR library
Ou S. and Jiang N.✉ (2018). LTR_retriever: A Highly Accurate and Sensitive Program for Identification of Long Terminal Repeat Retrotransposons. [Plant Physiol. 176(2): 1410-1422](http://www.plantphysiol.org/content/176/2/1410)

### The DNA TE library
Ou S., Su W., Liao Y., Chougule K., Agda J. R. A., Hellinga A. J., Lugo C. S. B., Elliott T. A., Ware D., Peterson T., Jiang N.✉, Hirsch C. N.✉ and Hufford M. B.✉ (2019). Benchmarking Transposable Element Annotation Methods for Creation of a Streamlined, Comprehensive Pipeline. [Genome Biol. 20(1): 275](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1905-y)

### The SINE library
Li Y., Jiang N., and Sun Y.✉ (2021). AnnoSINE: a short interspersed nuclear elements annotation tool for plant genomes. [Plant Physiol. 188(2): 955–970](https://academic.oup.com/plphys/article/188/2/955/6430992)
