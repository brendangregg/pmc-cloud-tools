# pmc-cloud-tools

Linux tools for measuring PMCs (Performance Monitoring Counters) in the cloud.

These are works-in-progress for analyzing some production issues, so the output may be adjusted as needed and more options added to these tools.

## Architectural PMCs

These are seven PMCs that are enabled on some newer AWS EC2 types, eg, m4.16xl. See <a href="http://www.brendangregg.com/blog/2017-05-04/the-pmcs-of-ec2.html">The PMCs of EC2</a>.

### pmcarch

Summarizes the architectural PMCs. Eg:

<pre>
# <b>./pmcarch</b>
K_CYCLES   K_INSTR      IPC BR_RETIRED   BR_MISPRED  BMR% LLCREF      LLCMISS     LLC%
96163187   87166313    0.91 19730994925  679187299   3.44 656597454   174313799  73.45
93988372   87205023    0.93 19669256586  724072315   3.68 666041693   169603955  74.54
93863787   86981089    0.93 19548779510  669172769   3.42 649844207   176100680  72.90
93739565   86349653    0.92 19339320671  634063527   3.28 642506778   181385553  71.77
94495981   86815232    0.92 19482504710  648954409   3.33 628548666   180975066  71.21
</pre>

- `K_CYCLES`: CPU Cycles x 1000
- `K_INSTR`: CPU Instructions x 1000
- `IPC`: Instructions-Per-Cycle
- `BMR%`: Branch Misprediction Ratio, as a percentage
- `LLC%`: Last Level Cache hit ratio, as a percentage

USAGE:

<pre>
# <b>./pmcarch -h</b>
USAGE: pmcarch {-C CPU | -p PID | -c CMD} [interval [duration]]
                 -C CPU         # measure this CPU only
                 -p PID         # measure this PID only
                 -c 'CMD'       # measure this command only (quote it)
                 interval       # output interval in secs (default 1)
                 duration       # total seconds (default infinityish)
  eg,
       pmcarch                  # show stats across all CPUs
       pmcarch 5                # show stats every 5 seconds
       pmcarch -C 0             # measure CPU 0 only
       pmcarch -p 181           # measure PID 181 only
       pmcarch -c 'cksum /boot/*'  # measure run and measure this cmd
</pre>

## Extended PMCs

These are available on some AWS Nitro hypervisor instance types. Eg, c5.9xl.

### tlbstat

Summarizes TLB (Translation Lookaside Buffer) statistics (an MMU cache). Eg:

<pre>
# <b>./tlbstat</b>
K_CYCLES   K_INSTR      IPC DTLB_WALKS ITLB_WALKS K_DTLBCYC  K_ITLBCYC  DTLB% ITLB%
93091508   86971957    0.93 135562325  45028565   3053416    1121015     3.28  1.20
94354781   88708445    0.94 136898873  49736338   3135383    1199061     3.32  1.27
94274360   88372901    0.94 138668282  48503863   3200281    1194388     3.39  1.27
92040379   86763153    0.94 133141376  44859310   3060742    1118921     3.33  1.22
92152495   87144845    0.95 135446984  50308740   3156780    1200598     3.43  1.30
</pre>

- `K_CYCLES`: CPU Cycles x 1000
- `K_INSTR`: CPU Instructions x 1000
- `IPC`: Instructions-Per-Cycle
- `DTLB_WALKS`: Data TLB walks (count)
- `ITLB_WALKS`: Instruction TLB walks (count)
- `K_DTLBCYC`: Cycles at least one PMH is active with data TLB walks x 1000
- `K_ITLBCYC`: Cycles at least one PMH is active with instr. TLB walks x 1000
- `DTLB%`: Data TLB active cycles as a ratio of total cycles
- `ITLB%`: Instruction TLB active cycles as a ratio of total cycles

USAGE:

<pre>
# <b>./tlbstat -h</b>
USAGE: tlbstat {-C CPU | -p PID | -c CMD} [interval [duration]]
                 -C CPU         # measure this CPU only
                 -p PID         # measure this PID only
                 -c 'CMD'       # measure this command only (quote it)
                 interval       # output interval in secs (default 1)
                 duration       # total seconds (default infinityish)
  eg,
       tlbstat                  # show stats across all CPUs
       tlbstat 5                # show stats every 5 seconds
       tlbstat -C 0             # measure CPU 0 only
       tlbstat -p 181           # measure PID 181 only
       tlbstat -c 'cksum /boot/*'  # measure run and measure this cmd
</pre>

## ALL PMCs

These may not work on the cloud yet, and may be works-in-progress. They may also require more recent Linux versions for the PMC aliases.

### cpucache

Summarizes CPU L1/L2/LLC cache hit ratios. Eg:

<pre>
# <b>./cpucache</b>
All counter columns are x 1000
CYCLES     INSTR        IPC L1DREF    L1DMISS    L1D% L2REF    L2MISS     L2% LLCREF   LLCMISS   LLC%
55827883   44795258    0.80 11674473  98067     99.16 161026   78908    51.00 97522    31338    67.87
55841905   44803990    0.80 11685292  98216     99.16 162127   79934    50.70 98810    30975    68.65
55832518   44838601    0.80 11700599  98181     99.16 160586   78725    50.98 96895    31497    67.49
55832741   44831064    0.80 11700963  98102     99.16 160849   78377    51.27 97296    31477    67.65
55832729   44763451    0.80 11672814  98216     99.16 163742   80630    50.76 99961    30679    69.31
55847526   44815072    0.80 11691652  98299     99.16 162176   79552    50.95 99092    30798    68.92
55832810   44748131    0.80 11667573  98132     99.16 163209   82497    49.45 101771   31035    69.50
55832325   44788718    0.80 11670262  98459     99.16 161834   79713    50.74 97504    31057    68.15
55832453   44795688    0.80 11682541  98069     99.16 162384   77339    52.37 95222    30347    68.13
55832224   44749672    0.80 11688124  98007     99.16 163076   79868    51.02 100960   30842    69.45
55837742   44793089    0.80 11689272  98273     99.16 161173   79280    50.81 97667    31776    67.46
[...]
</pre>

- 'CYCLES': CPU Cycles x 1000
- 'INSTR': CPU Instructions x 1000
- 'IPC': Instructions-Per-Cycle
- 'L1DREF': Level 1 data cache loads x 1000
- 'L1DMISS': Level 1 data cache load misses x 1000
- 'L1D%': Level 1 data cache hit ratio, as a percentage
- 'L2REF': Level 2 requests x 1000
- 'L2MISS': Level 2 misses x 1000
- 'L2%': Level 2 hit ratio, as a percentage
- 'LLCREF': Last Level Cache references x 1000
- 'LLCMISS': Last Level Cache misses x 1000
- 'LLC%': Last Level Cache hit ratio, as a percentage

USAGE:

<pre>
# <b>./cpucache -h</b>
USAGE: cpucache {-C CPU | -p PID | -c CMD} [interval [duration]]
                 -C CPU         # measure this CPU only
                 -p PID         # measure this PID only
                 -c 'CMD'       # measure this command only (quote it)
                 interval       # output interval in secs (default 1)
                 duration       # total seconds (default infinityish)
  eg,
       cpucache                 # show stats across all CPUs
       cpucache 5               # show stats every 5 seconds
       cpucache -C 0            # measure CPU 0 only
       cpucache -p 181          # measure PID 181 only
       cpucache -c 'cksum /boot/*' # measure run and measure this cmd
</pre>
