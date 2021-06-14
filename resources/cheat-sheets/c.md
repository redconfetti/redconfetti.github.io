---
layout: page
title: C Programming Language
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

## Data Types

| Type                            | Keyword            | Bytes | Range                                                   |
| ------------------------------- | ------------------ | ----- | ------------------------------------------------------- |
| Character                       | char               | 1     | -128 to 128                                             |
| Short integer                   | short              | 2     | -32767 to 3276                                          |
| Integer                         | int                | 4     | -2,147,483,647 - 2,147,483,647                          |
| Long Integer                    | long               | 4     | -2,147,483,647 - 2,147,483,647                          |
| Long Long Integer               | long long          | 8     | -9,223,372,036,854,775,807 - -9,223,372,036,854,775,807 |
| Unsigned character              | unsigned char      | 1     | 0 to 255                                                |
| Unsigned short integer          | unsigned short     | 2     | 0 to 65525                                              |
| Unsigned integer                | unsigned int       | 4     | 0 to 4,294,967,295                                      |
| Unsigned long integer           | unsigned long      | 4     | 0 to 4,294,967,295                                      |
| Unsigned long long integer      | unsigned long long | 8     | 0 to 18,446,744,073,709,551,615                         |
| Single-precision floating-point | float              | 4     | 1.2E-38 to 3.4E38¹                                      |
| Double-precision floating-point | double             | 8     | 2.2E-308 to 1.8E308²                                    |

## Constants

```c
#define PI 3.14159

const int count = 100;
```

## Operator Precedence

| Operators         | Relative Precedence   |
| ----------------- | --------------------- |
| ++ --             | 1                     |
| * / %             | 2                     |
| + -               | 3                     |
