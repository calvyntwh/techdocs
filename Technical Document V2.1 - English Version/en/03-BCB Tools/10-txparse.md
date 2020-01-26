# txparse

txparse：Tools for analyzing transaction hash.

## 1. Usage

The format of the command is as follows:

```
Usage:
  orgid --txStr=hello

Flags:
  -h, --help   help for txparse
```

## 2. Detailed command

- **command**

  ```
  txparse --txStr=
  	"local<tx>.v2.AetboYAmy2TEyUbsR731FTLDLyHE1MVKsSd4v7hS1jFnNkrtmGEVxVmWHR3
      jVSUffxKgW7aPawnQaVrZ4gwMt6aogUAJjhvnukfPWnxmsybqDgdjgecjsXa94bamPqgPhTTZC9Sz
      b.<1>.YTgiA1gdDGi2L8iCryAn34dXVYKUEdmBxivyHbK57wKpBcX5KrKyn1vdmZTuKKZ7PotCjcb
      ASbesv61VLE8H38TDiopHrs2eHG9z9iEDDyLcN7giLPCgFiLN9LPRiYZgxwpR95echr2bRPbijnKW
      j"
  ```

- **Input Parameters**

  | **Grammer** | **Type** | **Comment**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
  | -------- | :------: | ------------------------------------------------------------ |
  | tx       |  String  | Transaction string. It needs to support hexadecimal string and normal transaction string.                   |

- **Output SUCCESS Result**

  ```
  OK
  Tx: types.Transaction{Nonce:0x9, GasLimit:600, Note:"", Messages:[]types.Message{types.Message{Contract:"devtestLL6sMXu8s2hhFRoZH67Q8fig9djogVi3H", MethodID:0x44d8ca60, Items:[]common.HexBytes{A864657674657
  3744C766537674A556B4E6B4E595638755673327A573355425A5745625654686E4E44, 87000DA475ABF000}}}}
  PubKey:  b641206eee1e1e0c199a13fb3afa6757ec395effbc4943e1a942ea5549f14e33
  Contract:  localLL6sMXu8s2hhFRoZH67Q8fig9djogVi3H
  MethodID: 0x44d8ca60
  Items count:  2
  ```
