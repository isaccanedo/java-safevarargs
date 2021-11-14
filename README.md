# 1. Visão Geral
Neste tutorial rápido, daremos uma olhada na anotação @SafeVarargs.

# 2. A anotação @SafeVarargs
Java 5 introduziu o conceito de varargs, ou um parâmetro de método de comprimento variável, bem como tipos parametrizados.

Combinar estes pode causar problemas para nós:

```
public static <T> T[] unsafe(T... elements) {
    return elements; // unsafe! don't ever return a parameterized varargs array
}

public static <T> T[] broken(T seed) {
    T[] plant = unsafe(seed, seed, seed); // broken! This will be an Object[] no matter what T is
    return plant;
}

public static void plant() {
   String[] plants = broken("seed"); // ClassCastException
}
```

Esses problemas são difíceis para um compilador confirmar e, por isso, ele dá avisos sempre que os dois são combinados, como no caso do inseguro:


```
warning: [unchecked] Possible heap pollution from parameterized vararg type T
  public static <T> T[] unsafe(T... elements) {
```

Este método, se usado incorretamente, como no caso de quebrado, poluirá um array Object [] no heap em vez do tipo pretendido b.

Para reprimir esse aviso, podemos adicionar a anotação @SafeVarargs nos métodos e construtores finais ou estáticos.

@SafeVarargs é como @SupressWarnings, pois nos permite declarar que um determinado aviso do compilador é um falso positivo. Assim que garantirmos que nossas ações sejam seguras, podemos adicionar esta anotação:

```
public class Machine<T> {
    private List<T> versions = new ArrayList<>();

    @SafeVarargs
    public final void safe(T... toAdd) {
        for (T version : toAdd) {
            versions.add(version);
        }
    }
}
```

O uso seguro de varargs é um conceito complicado por si só. Para obter mais informações, Josh Bloch tem uma ótima explicação em seu livro, Effective Java.

# 3. Conclusão
Neste artigo rápido, vimos como usar a anotação @SafeVarargs em Java.