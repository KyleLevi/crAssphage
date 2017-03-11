# SRA_PhageSearch 
##--Work in Progress--  Everything should be cleaned, documented and uploaded by 3/20
Small programs used to search Sequence Read Archive metagenomes for phages (Or anything really - but we did phages)


## samstats.py
*Requires pysam
Analyze a single samfile and append some statistics to a file (Feel free to change the output)
USAGE:  python samstats.py  -i <samfile>
                            -o <CSV file to be appended>
                            -l <optional match length requirement for 'clean' reads>
                            -c (flag to convert to bamfile, sort and index
EX: python samstats.py -i test_samfiles/SRR3403834.crass.sam -o SRAstats.csv -l 50 
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
on a single line in the output file 'SRAstats.csv'.
'Clean' matches are matches longer than 50 base pairs 
