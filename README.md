Analysis
=====

run some performance benchmarks and explain them, explain how this solution can scale(vertically or horizontally - your preference), 
what  do we have to do to make it run 2x faster, is there any difference if we ask for 2 or 200 attributes?

Results
=====

Included is a `resources/summary.csv` file that includes some statistics; 10000 runs of a Population of 100000. 
commandline args were: female 100000 10000
The only difference between the execution of the two is this line:

```
        long[] timings = LongStream.generate(() -> timing(args[0], peopleCount)).limit(iterations).toArray();
```

to

```
        long[] timings = LongStream.generate(() -> timing(args[0], peopleCount)).parallel().limit(iterations).toArray();
```

By using multi-threading we achieved an **astonishing 5.8x improvement in throughput!** You can see that all metrics are dramatically better given the same input.
This is only a vertical scaling. 

Like all map-reduce functionality we can horizontally scale this problem by diverting a portion of the job set 
to other hosts and them reducing them in a finalization pipeline.


