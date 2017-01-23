
#============================================#
#          SAGs' GLOBAL ABUNDANCE            #
#============================================#


1) Merge SAGs and MMETSPs V9 in a single fasta file {{Rmarkdown}}:

	- GEN [903 SAGs]: 868 SAGs contain the V9 forward primer.
	- ICM [40 SAGs]: 31 SAGs contain the V9 forward primer.
	- BLA [169 SAGs]: 163 SAGs contain the V9 forward primer.
	- MMETSP [537 V9 sequences]

 	--> SAGs_V9_all.fasta [1599 sequences] 

______________________________________________

2) BLAST SAGs_V9_all.fasta on swarms db:

	2.1) VSEARCH 

		$ vsearch --usearch_global input/SAGs_V9_all.fasta --maxrejects 0 --maxaccept 0 -db ./input/swarms_ref_db.fasta --blast6out 			
		vsearch_BLAST_output.txt --id 1

		vsearch v2.3.4_linux_x86_64, 3.7GB RAM, 2 cores
		https://github.com/torognes/vsearch

		Reading file ./input/swarms_ref_db.fasta 100%  
		60032015 nt in 474303 seqs, min 50, max 262, avg 127
		Masking 100%  
		Counting unique k-mers 100%  
		Creating index of unique k-mers 100%  
		Searching 100%  
		Matching query sequences: 1185 of 1599 (74.11%)

		--> input: swarms_ref_db.fasta, SAGs_V9_all.fasta
		--> output: vsearch_BLAST_output.txt



	2.2) BLAST

		$ blastn -task megablast -max_target_seqs 1 -db swarms_blast_db -outfmt '6 qseqid sseqid pident length qlen slen qcovs evalue' -perc_identity 100 -query SAGs_V9_all.fasta -out SAGs_swarms_BLAST.out     

		- input --> swarms_ref_db.fasta, swarms_db.sh // SAGs_V9_all.fasta, SAGs_swarms_BLAST.sh
		- output --> SAGs_swarms_BLAST.out

		$ python3 remove_BLAST_repeated_otus.py

		- input --> SAGs_swarms_BLAST.out
		- output --> SAGs_swarms_BLAST_noreplicates.txt 

______________________________________________


3) Figure 'abundance vs no. of stations in which SAGs/swarms occurre'

	3.1) Get station no. and md5sum 
	  $ awk -F "\t" '{print $2, $(NF-1)}' data_abund_tb_piconano_nano.txt > data_abund_tb_piconano_nano_reduced.txt

	3.2) remove head of "data_abund_tb_piconano_nano_reduced.txt"
	  $ sed '1d' data_abund_tb_piconano_nano_reduced.txt > data_abund_tb_piconano_nano_reduced.noheader.txt

	3.3) python3 count_stations_per_swarm.py


	(repeat the same for "data_abund_tb_piconano.txt" and "data_abund_tb_nano.txt")

______________________________________________

