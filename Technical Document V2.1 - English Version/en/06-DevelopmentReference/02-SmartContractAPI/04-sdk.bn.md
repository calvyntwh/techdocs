# sdk/bn

The code package `blockchain/smcsdk/sdk/bn` encapsulates a class `Number` that deals with large numbers. When performing addition, subtraction, multiplication and division operations, you do not need to consider overflow.

## 1 functions

This chapter describes the simple constructor of class `Number` provided by package `sdk/bn`

### 1.1 func N()

Function prototype:

```
func N(x int64) Number
```

Convert `x` of type `int64` to an object of type `Number` and return.



### 1.2 func N1()

Function prototype:

```
func N1(b int64, d int64) Number
```

According to the incoming `b` and `d`, the result of `b * d` is calculated and an object of type `Number` is returned.



### 1.3 func N2()

Function prototype:

```
func N2(b int64, d1, d2 int64) Number
```

According to the incoming `b`，`d1`，`d2`, calculate the result of `b * d1 * d2` and return the `Number` type object.



### 1.4 func NB()

Function prototype:

```
func NB(x *big.Int) Number
```

Convert the big integer `x` of type `big.Int` to an object of type `Number` and return it.



### 1.5 func NBS()

Function prototype:

```
func NBS(x []byte) Number
```

Convert the byte slice `x` representing the unsigned large integer by large end to an object of type `Number` and return.



### 1.6 func NSBS()

Function prototype:

```
func NSBS(x []byte) Number
```

Convert the `x` byte slice representing signed large integer by big end to an object of type `Number` and return.



### 1.7 func NBytes()

Function prototype:

```
func NBytes(x []byte) Number
```

Convert the byte slice `x` representing the unsigned large integer by large end to an object of type `Number` and return.



### 1.8 func NSBytes()

Function prototype:

```
func NSBytes(x []byte) Number
```

Convert the `x` byte slice representing signed large integer by big end to an object of type `Number` and return.



### 1.9 func NString()

Function prototype:

```
func NS(s string) Number
```

Convert the large integer `s` represented by the decimal string to an object of type `Number` and return, if failed to parse, return `0`.



### 1.10 func NStringHex()

Function prototype:

```
func NS(s string) Number
```

将以 `0x` 或 `0X` 开头的十六进制字符串表示的无符号大整数 `s` 转换成 `Number` 类型对象并返回，如果解析失败将返回 `0`。
Convert the unsigned large integer `s` represented by hexadecimal string starting with `0x` or `0X` to an object of type `Number` and return it. If the resolution fails, it will return `0`.


### 1.11 func NewNumber()

Function prototype:

```
func NewNumber(x int64) Number
```

Convert the large integer `x` of type `int64` to an object of type `Number` and return.



### 1.12 func NewNumberStringBase()

Function prototype:

```
func NewNumberStringBase(s string, base int) Number
```

Convert the large integer `s` represented by the string into an object of type `Number` and return it. The string is parsed according to the given cardinality `base`. If the parsing fails, it will return `0`.

The cardinality `base` must be an integer between `0` or `2` and `MaxBase`. If `base` is `0`, the prefix of string `s` determines the actual conversion cardinality: prefix `"0x"`、`"0X"` indicates hexadecimal; prefix `"0b"`、`"0B"` indicates binary; prefix `"0"` indicates octal; others automatically use decimal as cardinality.


For the cardinal number of `<= 36`, upper case letters and lower case letters express the same number. The letters `'a'` to `'z'` and `'A'` to `'Z'` express the values `10` to `35`.


For cardinal numbers of `>36`, capital letters `'A'` to `'Z'` represent values of `36` to `61`.



### 1.13 func NewNumberBigInt()

Function prototype:

```
func NewNumberBigInt(x *big.Int) Number
```

Convert the big integer `x` of type `big.Int` to an object of type `Number` and return it.



### 1.14 func NewNumberLong()

Function prototype:

```
func NewNumberLong(b int64, d int64) Number
```

According to the incoming `b` and `d`, the result of `b * d` is calculated and an object of type `Number` is returned.



### 1.15 func NewNumberLongLong()

Function prototype:

```
func NewNumberLongLong(b int64, d1, d2 int64) Number
```

According to the incoming `b`，`d1`，`d2`, calculate the result of `b * d1 * d2` and return the `Number` type object.



## 2 class Number

This chapter describes the member functions of class `Number`.

### 2.1 func String()

Function prototype:

```
func (x Number) String() string
```

Implement the standard string expression interface to convert `x` to decimal string. `x` does not set the initial large value, return `nil`.



### 2.2 func Value()

Function prototype:

```
func (x Number) Value() *big.Int
```

Get the value of `big.Int` of `x`. `x` does not set the initial large value, return `nil`.



### 2.3 func CmpI()

Function prototype:

```
func (x Number) CmpI(y int64) int
```

Compare `x` with `y`, and return `-1` for `x < y`, `0` for `x == y`, and `+1` for `x > y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.4 func Cmp()

Function prototype:

```
func (x Number) Cmp(y Number) int
```

Compare `x` with `y`, and return `-1` for `x < y`, `0` for `x == y`, and `+1` for `x > y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.5 func IsZero()

Function prototype:

```
func (x Number) IsZero() bool
```

Judge whether `x` is `0`. if `x` is equal to `0` and returns `true`. `x` will trigger `panic` if the initial value is not set.



### 2.6 func IsPositive()

Function prototype:

```
func (x Number) IsPositive() bool
```

Judge whether `x` is a positive integer (excluding `0`). Return `true` if `x` is a positive number. `x` will trigger `panic` if the initial value is not set.



### 2.7 func IsNegative()

Function prototype:

```
func (x Number) IsNegative() bool
```

Determine whether `x` is a negative integer. Return `true` if `x` is a negative number. `x` will trigger `panic` if the initial value is not set.



### 2.8 func IsEqualI()

Function prototype:

```
func (x Number) IsEqualI(y int64) bool
```

Compare `x` with `y`, and return `true` for `x == y`，and `false` for `x != y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.9 func IsEqual()

Function prototype:

```
func (x Number) IsEqual(y Number) bool
```

Compare `x` with `y`, and return `true` for `x == y`，and `false` for `x != y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.


### 2.10 func IsGreaterThanI()

Function prototype:

```
func (x Number) IsGreaterThanI(y int64) bool
```

Compare `x` with `y`, and return `true` for `x > y`，and `false` for `x <= y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.11 func IsGreaterThan()

Function prototype:

```
func (x Number) IsGreaterThan(y Number) bool
```

Compare `x` with `y`, and return `true` for `x > y`，and `false` for `x <= y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.


### 2.12 func IsLessThanI()

Function prototype:

```
func (x Number) IsLessThanI(y int64) bool
```

Compare `x` with `y`, and return `true` for `x < y`，and `false` for `x >= y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.


### 2.13 func IsLessThan()

Function prototype:

```
func (x Number) IsLessThan(y Number) bool
```

Compare `x` with `y`, and return `true` for `x < y`，and `false` for `x >= y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.14 func IsGEI()

Function prototype:

```
func (x Number) IsGEI()(y int64) bool
```

Compare `x` with `y`, and return `true` for `x >= y`，and `false` for `x < y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.15 func IsGE()

Function prototype:

```
func (x Number) IsGE(y Number) bool
```

Compare `x` with `y`, and return `true` for `x >= y`，and `false` for `x < y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.


### 2.16 func IsLEI()

Function prototype:

```
func (x Number) IsLEI(y int64) bool
```

Compare `x` with `y`, and return `true` for `x <= y`，and `false` for `x > y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.17 func IsLE()

Function prototype:

```
func (x Number) IsLE(y Number) bool
```

Compare `x` with `y`, and return `true` for `x <= y`，and `false` for `x > y`. If no initial value is set for `x` or `y`, then `panic` will be triggered.


### 2.18 func AddI()

Function prototype:

```
func (x Number) AddI(y int64) Number
```

Calculate `x + y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.19 func Add()

Function prototype:

```
func (x Number) Add(y Number) Number
```

Calculate `x + y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.20 func SubI()

Function prototype:

```
func (x Number) SubI(y int64) Number
```

Calculate `x - y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.21 func Sub()

Function prototype:

```
func (x Number) Sub(y Number) Number
```

Calculate `x - y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.22 func MulI()

Function prototype:

```
func (x Number) MulI(y int64) Number
```

Calculate `x * y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.23 func Mul()

Function prototype:

```
func (x Number) Mul(y Number) Number
```

Calculate `x * y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.24 func DivI()

Function prototype:

```
func (x Number) DivI(y int64) Number
```

Calculate `x / y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.25 func Div()

Function prototype:

```
func (x Number) Div(y Number) Number
```

Calculate `x / y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.26 func ModI()

Function prototype:

```
func (x Number) ModI(y int64) Number
```

Calculate `x % y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.27 func Mod()

Function prototype:

```
func (x Number) Mod(y Number) Number
```

Calculate `x % y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.


### 2.28 func Sq()

Function prototype:

```
func (x Number) Sq() Number
```

Calculate `x ** 2` and return the calculation result. If no initial value is set for `x`, then `panic` will be triggered.


### 2.29 func Sqrt()

Function prototype:

```
func (x Number) Sqrt() Number
```

Calculate the square of `x` and return the calculation result. If no initial value is set for `x`, then `panic` will be triggered.



### 2.30 func Exp()

Function prototype:

```
func (x Number) Exp(y Number) Number
```

Calculate `x ** y` and return the calculation result. If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.31 func Lsh()

Function prototype:

```
func (x Number) Lsh(n uint) Number
```

Move `x` to the left by `n`, and return the result. If no initial value is set for `x`, then `panic` will be triggered.


### 2.32 func Rsh()

Function prototype:

```
func (x Number) Rsh(n uint) Number
```

Move `x` to the right by `n`, and return the result. If no initial value is set for `x`, then `panic` will be triggered.



### 2.33 func And()

Function prototype:

```
func (x Number) And(y Number) Number
```

Calculate `x & y` bit by bit, and return the calculation result (note that `x` and `y` may be negative here, and use binary complement form for bitwise sum). If no initial value is set for `x` or `y`, then `panic` will be triggered.


### 2.34 func Or()

Function prototype:

```
func (x Number) Or(y Number) Number
```

Calculate `x | y` bit by bit, and return the calculation result (note that `x` and `y` may be negative here, and use binary complement form for bitwise or). If no initial value is set for `x` or `y`, then `panic` will be triggered.



### 2.35 func Xor()

Function prototype:

```
func (x Number) Xor(y Number) Number
```

Calculate `x ^ y` bit by bit, and return the calculation result (note that `x` and `y` may be negative here, and use binary complement form for exclusive or bit by bit). If no initial value is set for `x` or `y`, then `panic` will be triggered.




### 2.36 func Not()

Function prototype:

```
func (x Number) Not() Number
```

Calculate `^ x` bit by bit and return the calculation result (note that `x` may be a negative number here, which is inverted in the form of binary complement). If no initial value is set for `x`, then `panic` will be triggered.



### 2.37 func Bytes()

Function prototype:

```
func (x Number) Bytes() []byte
```

Implement the standard get byte slice interface, convert `x` to byte slice in large end order (the first byte is interpreted as the symbol, the negative number is `0xFF`, the non negative number is `0x00`), and return the conversion result. For example: `380` will be converted to `0x00017C`; `-380` will be converted to `0xFF017C`. If no initial value is set for `x`, then `panic` will be triggered.



### 2.37 func SetBytes()

Function prototype:

```
func (x *Number) SetBytes(buf []byte) Number
```

Set the value of `x` to a byte slice in large endian order (the first byte is `0xFF`, which means it is a negative number, and its absolute value is encoded from the second byte, otherwise the whole byte slice represents a non negative integer), and return the value of `x`. For example: `0x00017C`and`0x017C` will be converted to `380`; and `0xFF017C` will be converted to `-380`.If no initial value is set for `x`, then `panic` will be triggered.



### 2.38 func MarshalJSON()

Function prototype:

```
func (x Number) MarshalJSON() (data []byte, err error)
```

实现标准的 JSON 序列化接口。将 `x` 转成简化版的 JSON 字符串，例如字符串`380`。`x` 未设定初始大数值将触发 `panic`。
Implement the standard JSON serialization interface. Convert `x` to a simplified version of JSON string, such as string `380`. If no initial value is set for `x`, then `panic` will be triggered.


### 2.39 func UnmarshalJSON()

Function prototype:

```
func (x *Number) UnmarshalJSON(data []byte) error
```

Implement the standard JSON deserialization interface. Set the value of `x` to the large number corresponding to the JSON string entered. Support simplified version of JSON string (for example: String `380`) and structural version of JSON string (for example: String `{"v":380}`).