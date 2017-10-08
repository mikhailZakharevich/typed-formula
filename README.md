# Overview

Type Formula allows you write lazy formula in a type safe mode and calculate value when it needed. 

# Generic Formulas
Typed Formula provides functionality to build formulas for numeric calculations on top of Integers, Longs and Double wrapped into [Numbers](https://github.com/IgorWolkov/typed-formula/blob/master/src/main/scala/karazinscalausersgroup/typed/formula/numbers/Number.scala).

Let's define simple formulas to calculate area and a volume of a figure.
Firstly define measurements.

```scala
import karazinscalausersgroup.typed.formula.Value
import karazinscalausersgroup.typed.formula.numbers.{Number, Value}
import karazinscalausersgroup.typed.formula.numbers.operations._


scala> val length = Value[Number](1)
length: Value[Number] = Expression(IntNumber(1))

scala> val width = Value[Number](3)
width: Value[numbers.Number] = Expression(IntNumber(3))

scala> val height = Value[Number](2.0)
height: Value[numbers.Number] = Expression(DoubleNumber(2.0))

```

Now we can easily define formulas

```scala
scala> val area = length * width
area: Value[Number] * Value[Number] = (Expression(IntNumber(1)) * Expression(IntNumber(3)))

scala> val volume = area * height
volume: Value[Number] * Value[Number] * Value[Number] = ((Expression(IntNumber(1)) * Expression(IntNumber(3))) * Expression(DoubleNumber(2.0)))
```

and calculate values 

```scala
scala> area.v
res0: Number = IntNumber(3)

scala> volume.v
res1: Number = DoubleNumber(6.0)

```

# Custom formulas
Let's rewrite the example with custom classes form formula terms

```scala
import karazinscalausersgroup.typed.formula.Value
import karazinscalausersgroup.typed.formula.numbers.{Number, Value}
import karazinscalausersgroup.typed.formula.numbers.operations._


case class Length(value: Int) extends Value[Number] {
  def v = Number(value)
}

case class Width(value: Int) extends Value[Number] {
  def v = Number(value)
}

case class Height(value: Double) extends Value[Number] {
  def v = Number(value)
}

scala>  val length = Length(1)
length: Length = Expression(IntNumber(1))

scala> val width = Width(3)
width: Width = Expression(IntNumber(3))

scala> val height = Height(2.0)
height: Height = Expression(DoubleNumber(2.0))

```

Now we can define formulas with custom term
```scala
scala> val area = length * width
area: Length * Width = (Expression(IntNumber(1)) * Expression(IntNumber(3)))

scala> val volume = area * height
volume: Length * Width * Height = ((Expression(IntNumber(1)) * Expression(IntNumber(3))) * Expression(DoubleNumber(2.0)))

```

and calculate values 

```scala
scala> area.v
res0: Number = IntNumber(3)

scala> volume.v
res1: Number = DoubleNumber(6.0)

```

# Supported operations
In current version only operations for Numbers are supported.

## Linear operations
Let's define define two variables
```scala
import karazinscalausersgroup.typed.formula.Value
import karazinscalausersgroup.typed.formula.numbers.{Number, Value}
import karazinscalausersgroup.typed.formula.numbers.operations._


scala> val a = Value[Number](3)
a: Value[Number] = Expression(IntNumber(3))

scala> val b = Value[Number](5)
b: Value[Number] = Expression(IntNumber(5))
```

Typed Formula supports following linear operation: 
### addition
```scala

scala> a + b
res1: Value[Number] + Value[Number] = (Expression(IntNumber(3)) + Expression(IntNumber(5)))

```

### subtraction
```scala
scala> a - b
res2: Value[Number] - Value[Number] = (Expression(IntNumber(3)) - Expression(IntNumber(5)))

```

### unary minus
Unlike `unary_` definitions for methods, it is impossible to overload types. That's why `~` (tilde) represents unary minus on type level. However it's possible to use `-` as unary minus with values.
```scala
scala> -a
res3: ~[Value[Number]] = -Expression(IntNumber(3)))

```

### multiplication
```scala
scala> a * b
res4: Value[Number] * Value[Number] = (Expression(IntNumber(3)) * Expression(IntNumber(5)))

```

### division

```scala
scala> a / b
res9: Value[numbers.Number] / Value[numbers.Number] = (Expression(IntNumber(3)) / Expression(IntNumber(5)))

```




Please review test:
* [How can you build formulas and manipulate with formulas.](https://github.com/IgorWolkov/typed-formula/blob/master/src/test/scala/karazinscalausersgroup/typed/formula/numbers/OperationsSpecification.scala)
* [How build loan booking accounting calculations in 1 day.](https://github.com/IgorWolkov/typed-formula/blob/master/src/test/scala/karazinscalausersgroup/typed/formula/numbers/entities/CoveringSpecification.scala)