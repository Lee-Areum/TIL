enum class
```
 enum class IntArithmetics : BinaryOperator<Int>, IntBinaryOperator{
   PLUS {
     override fun apply(t:Int, u:Int):Int = t+u
   }
   TIMES{
      override fun apply(t:Int, u:Int):Int = t*u
   }
   override fun applyAsInt(t:Int, u:Int) = apply(t,u)
```
```
PLUS(13,31) = 44
TIMES(13,31) = 403
```
