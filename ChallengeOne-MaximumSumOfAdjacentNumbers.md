# The maximum sum of adjacent numbers

## Description

Given a number list, we must find the **maximum** sum of all possible adjacent pairs present in the list.  
For example, for the input `[2, 4, 1, 5, 6, 3, 11]`, the result will be `14` since the pair `[3, 11]` sum is `14`.  
The solution must be at least _O(n),_ and input can also be *empty* or *null*, so we must be aware of these weird cases.

## Solution

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
