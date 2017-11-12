---
title:  C# List<T> 정렬하기
date: 2017-11-10 22:35:03
tags: 
  - C#
  - .NET
  - List(Collection)
  - Sort
  - Delegate
  - Lamda function
  - 씨샵
  - 닷넷
  - 리스트(Collection)
  - 정렬
  - 델리게이트
  - 람다식
categories:
  - 개발(dev)
  - c#(c#)
---

오랜만에 블로깅을 남기는 것 같다. 그동안 일을 좀 하느라 정리할 시간이 없어 블로깅을 하지 못했다.
C#에서 List<T>는 엄청 많이 사용되는 Collection중에 하나이다. 나는 Array보다 List를 더 선호한다. 이번 포스트에서는 클래스 객체를 담은 List를 정렬하는 것을 이야기 하려고 한다. 

## Sort 메소드 형식
List Collection에는 기본적으로 Sort라는 메소드가 존재한다. Collection이라는 상위 클래스가 가지는 메소드이다. Collection을 상속받는 클래스는 모두 존재한다고 해도 무방할 것이다.
Sort가 가지는 파라메터는 다음의 4가지가 존재한다.

Sample이라는 클래스를 생성하여 예를 들겠다.
```java
class Sample
{
    public int code;
    public string name;
}
```

- #### Sort();
단일 타입에서 사용하는 메소드로 오름차순으로 정렬이 된다.
```java
list.Sort();
```

- #### Sort(Comparison<T> comparison);
값을 비교하는 함수를 활용하여 정렬
```java
public int compare(Sample x, Sample y)
{
  if (x.code > y.code) return 1;
  else if(x.code < y.code) return -1;
  return 0;
  // (문자열 비교 : x.name.CompareTo(y.name))
}

list.Sort(compare); // 함수를 파라메터로 전달
```

- #### Sort(IComparer<T> comparer);
IComparer라는 인터페이스를 상속한 클래스를 활용하여 정렬 (클래스 내부에 인터페이스 함수를 구현)
```java
public class SampleCompare : IComparer
{
  public int compare(Sample x, Sample y)
  {
    if (x.code > y.code) return 1;
    else if(x.code < y.code) return -1;
    return 0;
    // (문자열 비교 : x.name.CompareTo(y.name))
  }
}

SampleCompare sc = new SampleCompare();

list.Sort(sc);  // ICompare를 상속한 클래스의 인스턴스를 파라메터로 전달

```

- #### Sort(int index, int count, IComparer<T> comparer); 
IComparer와 기능이 동일하고 차이점은 index에 시작위치를 count에 비교 개수를 입력
```java
public class SampleCompare : IComparer
{
  public int compare(Sample x, Sample y)
  {
    if (x.code > y.code) return 1;
    else if(x.code < y.code) return -1;
    return 0;
    // (문자열 비교 : x.name.CompareTo(y.name))
  }
}

SampleCompare sc = new SampleCompare();

list.Sort(index, count, sc);  // ICompare를 상속한 클래스의 인스턴스와 시작위치, 개수를 파라메터로 전달
```

## 델리게이트를 활용한 정렬
위의 메소드 4가지 형식중에 2번째 형식을 사용하는데 함수를 별도로 만들면 정렬 기준을 알려면 정의한 함수를 찾아서 봐야하는 번거로움이 생긴다. 이를 조금은 편하게 하기 위해 델리게이트를 사용해서 Sort() 메소드 내에 함수를 정의해서 사용할 수 있다. 예제 소스를 보면 금방 이해가 갈 것이라고 생각한다.

```java
list.sort(delegate (Sample x, Sample y) {
  if (x.code > y.code) return 1;
  else if(x.code < y.code) return -1;
  return 0;
  // (문자열 비교 : x.name.CompareTo(y.name))
});
```

이를 람다식을 활용해서 더 간단하게 표현하면 다음과 같다.
```java
list.sort((x, y) => (x.code > y.code) ? 1 : -1;
// (문자열 비교 : list.sort((x, y) => x.name.CompareTo(y.name));
```

