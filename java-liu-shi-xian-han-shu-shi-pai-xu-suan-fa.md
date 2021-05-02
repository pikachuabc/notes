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

## MergeSort

Java实现/Haskell实现

```java
public Stream<Integer> mSort(Stream<Integer> inputListStream) {
    List<Integer> temp = inputListStream.collect(Collectors.toList());
    if (temp.size()==1) return Stream.of(temp.get(0));
    return merge(mSort(temp.subList(0, temp.size() / 2).stream()),
            mSort(temp.subList(temp.size() / 2, temp.size()).stream()));
}

public Stream<Integer> merge(Stream<Integer> stream1, Stream<Integer> stream2) {
    List<Integer> list1 = stream1.collect(Collectors.toList());
    List<Integer> list2 = stream2.collect(Collectors.toList());
    if (list1.isEmpty()) return list2.stream();
    if (list2.isEmpty()) return list1.stream();
    return list1.get(0) > list2.get(0) ?
            Stream.concat(Stream.of(list2.get(0)),
                    merge(list1.stream(), list2.stream().skip(1)))
            : Stream.concat(Stream.of(list1.get(0)),
            merge(list2.stream(), list1.stream().skip(1)));
}
```

```haskell
msort :: [Int] -> [Int]
msort []  = []
msort [x] = [x]
msort xs  = merge (msort as) (msort bs)
    where as = take n xs
          bs = drop n xs
          n  = (length xs) `div` 2

merge :: [Int] -> [Int] -> [Int]
merge [] ys = ys
merge xs [] = xs
merge (x:xs) (y:ys)
    | x < y     = x : merge xs (y:ys)
    | otherwise = y : merge (x:xs) ys
```

