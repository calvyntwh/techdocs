# sigorg

sigorg：输入.tar.gz文件名和开发者签名文件文件名，验签并输出组织签名文件（对开发者签名数据进行签名）
Input the file name with `.tar.gz` and developer signature, verify signature and output organization signature file (sign developer signature data)

## 1. Usage

The format of the command is as follows:

```
Usage:
  sigorg --smc=./dice2win_2.0.tar.gz  --name=hotwal001 [--password=aBig62_123]

Flags:
  -h, --help   help for sigorg
```

## 2. Detailed command

- **command**

  ```
  sigorg --smc=./dice2win_2.0.tar.gz  --name=hotwal001 [--password=aBig62_123]
  ```

- **Input Parameters**

  | **Grammer** | **Type** | **Comment**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
  | -------- | :------: | ------------------------------------------------------------ |
  | name     |  String  | wallet name                                               |
  | password |  String  | optional, wallet password, 8-20 characters in length (it can be composed of any printable ASCII characters, and it is required to have at least one lowercase letter, at least one uppercase letter, at least one number, and at least one non alphanumeric character). <br>If the command line does not specify a password, the program prompts the user for the password. |

- **Output SUCCESS Result**

  ```
  OK
  SignatureFile: dice2win_2.0.sig.sig
  ```
