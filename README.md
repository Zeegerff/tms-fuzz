# TMS-Fuzz
An AFL extension to increase code coverage by targeting rare branches. TMS-Fuzz has a particular advantage on programs with highly nested structure (packet analyzers, archivers, programs compiled with laf-intel etc.). AFL is written and maintained by Michał Zalewski. TMS-Fuzz extension by Wen Zhang, Jinfu Chen, Saihua Cai, Kun Wang, Yisong Liu, and Haotong Ding.

## Summary
This is a modified version of AFL which changes (1) the way in which AFL selects inputs for mutation and (2) the way in which AFL schedules seed groups for fuzzing. These changes target rare branches through adaptive seed clustering and Thompson Sampling-based seed scheduling, achieving 5.9% improvement in edge coverage and 42% improvement in crash detection compared with state-of-the-art fuzzers. TMS-Fuzz seems particularly strong on programs structured with many nested conditional statements. The graphs below show the number of edges covered over time for the targets we evaluated; each figure shows the average of five runs:
<img width="1665" height="668" alt="image" src="https://github.com/user-attachments/assets/f1fcb8ad-3a2a-4cda-8892-af5a331f64f3" />
Evaluation was conducted on an AFL 2.57b. All runs were done with the AFL dictionary; the emphasis on rare branches applies to guide the input mutations over the 20 runs. For AFLFast, EcoFuzz, and SLIME, the number of rare finding positions of the sequence are also noted. FairFuzz is another benchmark that also targets rare branches.
For TMS-Fuzz on the eight real-world programs, the number of edge coverage over 24 hours is shown below, organized by target program. Each entry shows the average edge coverage / unique paths achieved over five 24-hour runs. TMS-Fuzz achieves higher coverage on most programs:
<img width="1662" height="549" alt="image" src="https://github.com/user-attachments/assets/337a69d8-2f98-49d0-bd76-32a79161f790" />
More details in article.

## Usage Summary
For basic AFL usage, check the online AFL docs or AFL github.
For basic seed usage, the same initialization and input/output directories as AFL apply:
# Compile target program with afl-gcc or afl-clang
CC=/path/to/afl-gcc ./configure
make

# Create input and output directories
mkdir input output

# Run TMS-Fuzz
./afl-fuzz -i input -o output -d -- /path/to/target @@

## Notes:

The -d flag disables AFL's deterministic stage, focusing fuzzing on the more efficient Havoc stage where TMS-Fuzz's seed scheduling has the most impact.
TMS-Fuzz works best on programs with complex nested conditional structures where different execution paths have significantly different coverage patterns.
