# We're using make to generate alignments and trees because, well why not!


help:
	@echo "Please use \`make <target>' where <target> is one of"
	@echo "    country_tree    create a neighbor joining tree and rename the leaves using the country= tag in the fasta def line"
	@echo "    blastn    blast all the sequences against crAssphage (e.g. to check orientation)"
	@echo "    align     align the sequences using the default aligner (mafft)"
	@echo "	   seqs.fa   assemble the DNA sequences into a single fasta file"
	@echo "    renum     renumber the fasta file and create a separate id.map"
	@echo "    clustalw  run a clustalw alignment on the sequences"
	@echo "    mafft     run a mafft alignment on the sequences (this makes a phylip format output used for building the trees)"
	@echo "    mafft_fa  run a mafft alignment but write the alignment in fasta format"
	@echo "    neighbor_tree   create a neighbor joining tree"
	@echo ""
	@echo "You probably want to start with country_tree, as it will create the sequences, build an alignment, rename the sequences, etc etc"

country_tree: neighbor_tree
	python ../bin/rename_trees.py -t seqs.nonneg.tree -i id.map -n country  > seqs.tree

tree: neighbor_tree
	python ../bin/rename_trees.py -t seqs.nonneg.tree -i id.map > seqs.named.tree

neighbor_tree: trim_seqs
	cp seqs.aln infile
	rm -f outfile 
	echo "y\n" | phylip dnadist
	mv -f outfile infile
	echo "y\n" | phylip neighbor
	mv outtree seqs.neighbor
	mv infile seqs.dnadist
	rm -f outfile
	python ../bin/negative_branch_lengths.py -t seqs.neighbor > seqs.nonneg.tree


blastn: seqs.fa
	blastn -db ../data/JQ995537.fna -query seqs.fa -out seqs.crassphage.blastn -outfmt '6 std qlen slen'


trim_seqs: seqs.aln
	mv seqs.aln seqs.aln.full
	python ../bin/trim_alignment.py -p seqs.aln.full -c 0.9 > seqs.aln

seqs.aln: align

align: mafft

mafft_fa: seqs.fa renum
	mafft --adjustdirectionaccurately --localpair  --maxiterate 16 --reorder seqs.renum.fa > seqs.faln

mafft: seqs.fa renum
	mafft --adjustdirectionaccurately --localpair  --maxiterate 16 --phylipout --reorder seqs.renum.fa > seqs.aln

clustalw:
	clustalw seqs.fa

renum: id.map

id.map: seqs.fa
	perl ../bin/renumber_fasta.pl seqs.fa seqs.renum.fa id.map

seqs.fa:
	cat ../../Sequences/PrimerB/*.fasta > seqs.fa

clean:
	rm -f *.aln *.dnd seqs.fa seqs.renum.fa id.map seqs.dnadist  seqs.neighbor  seqs.tree seqs.aln.full seqs.faln  seqs.named.tree seqs.nonneg.tree
