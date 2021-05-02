---
description: Haskell的语法着实给了一些声明式编程的启发，这里尝试着用这种思想在Java中实现部分排序
---

# Java流实现函数式排序算法

## QuickSort　

Java实现/Haskell实现

```java
    public Stream<Integer> qSort(Stream<Integer> inputListStream) {
        List<Integer> temp = inputListStream.collect(Collectors.toList());
        return temp.isEmpty() ? Stream.empty() :
                concat(concat(
                        qSort(temp.stream().skip(1).filter(x -> x < temp.get(0))),
                        Stream.of(temp.get(0))),
                        qSort(temp.stream().skip(1).filter(x -> x >= temp.get(0))));
    }
```

```haskell
qsort :: [Int] -> [Int]
qsort [] = []
qsort (pivot:others) = (qsort lowers) ++ [pivot] ++ (qsort highers)
    where lowers  = filter (<pivot)  others
          highers = filter (>=pivot) others
```

