# The maximum sum of adjacent numbers

## Description

Given a number list, we must find the **maximum** sum of all possible adjacent pairs present in the list.  
For example, for the input `[2, 4, 1, 5, 6, 3, 11]`, the result will be `14` since the pair `[3, 11]` sum is `14`.  
The solution must be at least _O(n),_ and input can also be *empty* or *null*, so we must be aware of these weird cases.

## Solution - *O(n)*

### Regular version
````Java
public static Integer getMaxSumOfAdjacentNumbers(final List<Integer> numbers) {

        if (Objects.isNull(numbers)) {
            throw new InvalidParameterException("Number list cannot be null.");
        }

        return
                IntStream
                        .range(1, numbers.size())
                        .mapToObj(i -> numbers.get(i - 1) + numbers.get(i))
                        .max(Integer::compareTo)
                        .orElseThrow(() -> new InvalidParameterException("Cannot operate with items whose size is less than two."));

    }
````

### Parallelized version

```Java
public static Integer getMaxSumOfAdjacentNumbersParallelized(final List<Integer> numbers) {

        if (Objects.isNull(numbers)) {
            throw new InvalidParameterException("Number list cannot be null.");
        }

        return
                IntStream
                        .range(1, numbers.size())
                        .parallel()
                        .mapToObj(i -> numbers.get(i - 1) + numbers.get(i))
                        .max(Integer::compareTo)
                        .orElseThrow(() -> new InvalidParameterException("Cannot operate with items whose size is less than two."));

    }
```

## Benchmarks

```
Benchmark                                                                                              Mode  Cnt     Score     Error  Units
CodingChallengesSexde.Challenges.ChallengeOneBenchmark.benchmarkChallengeOneWithLargeList              avgt   25  8952,020 ± 406,975  us/op
CodingChallengesSexde.Challenges.ChallengeOneBenchmark.benchmarkChallengeOneWithSmallList              avgt   25     0,088 ±   0,001  us/op
CodingChallengesSexde.Challenges.ChallengeOneBenchmark.benchmarkChallengeParallelizedOneWithLargeList  avgt   25  9188,726 ±  16,352  us/op
CodingChallengesSexde.Challenges.ChallengeOneBenchmark.benchmarkChallengeParallelizedOneWithSmallList  avgt   25     0,090 ±   0,001  us/op
```

In this case parallel implementation is slower than the regular one. Sometimes it's not worth it to use parallelization if the operation is not too much time consuming hence we must identify the real situation where we can extract the true potential of parallelization in Java.
