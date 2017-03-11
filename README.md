# SRA_PhageSearch 
##--Work in Progress--  Everything should be cleaned, documented and uploaded by 3/20
Small programs used to search Sequence Read Archive metagenomes for phages and organize/analyze the data.
(It doesn't need to be phage - thats just what we did)


## samstats.py
*Requires pysam
Analyze a single samfile and append some statistics to a file (Feel free to change the output).  If you're splitting this across multiple cores give -i $SGE_TASK_ID
USAGE:  python samstats.py  -i <samfile>
                            -o <CSV file to be appended>
                            -l <optional match length requirement for 'clean' reads>
                            -c flag to convert to bamfile, sort and index
EX: python samstats.py -i test_samfiles/SRR3403834.crass.sam -o test_outfiles/SRAstats.csv -l 50 
Will look in the directory 'samfiles' for the file 'SRR3403834.sam'
it will count the reads in the samfile and append the data:
          run accession,
          num matches,
          Avg Match Length,
          Total Matched Bases,
          Clean # of Matches,
          Clean Avg Match Length,
          Clean Total Matched Bases,
          Change in [1] -> [4],
          Change in [2] -> [5],
          Change in [3] -> [6],
          Crass or P2
on a single line in the output file 'SRAstats.csv' that is inside the directory test_outfiles.
'Clean' matches will be matches longer than 50 base pairs. 
