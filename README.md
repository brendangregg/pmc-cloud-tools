# pmc-cloud-tools

Linux tools for measuring PMCs (Performance Monitoring Counters) in the cloud.

## Architectural PMCs

These are seven PMCs that are enabled on some newer AWS EC2 types, eg, m4.16xl. See <a href="http://www.brendangregg.com/blog/2017-05-04/the-pmcs-of-ec2.html">The PMCs of EC2</a>.

### pmcarch

Summarizes the architectural PMCs. Eg:

<pre>
# ./pmcarch
K_CYCLES   K_INSTR      IPC BR_RETIRED   BR_MISPRED  BMR% LLCREF      LLCMISS     LLC%
96163187   87166313    0.91 19730994925  679187299   3.44 656597454   174313799  73.45
93988372   87205023    0.93 19669256586  724072315   3.68 666041693   169603955  74.54
93863787   86981089    0.93 19548779510  669172769   3.42 649844207   176100680  72.90
93739565   86349653    0.92 19339320671  634063527   3.28 642506778   181385553  71.77
94495981   86815232    0.92 19482504710  648954409   3.33 628548666   180975066  71.21
</pre>

USAGE:

<pre>
# ./pmcarch -h
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

## All PMCs

These are available on some AWS Nitro hypervisor instance types. Eg, c5.9xl.

### tlbstat

Summarizes TLB (Translation Lookaside Buffer) statistics (an MMU cache). Eg:

<pre>
# ./tlbstat
K_CYCLES   K_INSTR      IPC DTLB_WALKS ITLB_WALKS K_DTLBCYC  K_ITLBCYC  DTLB% ITLB%
93091508   86971957    0.93 135562325  45028565   3053416    1121015     3.28  1.20
94354781   88708445    0.94 136898873  49736338   3135383    1199061     3.32  1.27
94274360   88372901    0.94 138668282  48503863   3200281    1194388     3.39  1.27
92040379   86763153    0.94 133141376  44859310   3060742    1118921     3.33  1.22
92152495   87144845    0.95 135446984  50308740   3156780    1200598     3.43  1.30
</pre>

USAGE:

<pre>
# ./tlbstat -h
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
