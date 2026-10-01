# cstring

## Functions

|                     |
| ------------------- |
| [strcpy](#strcpy)   |
| [strncpy](#strncpy) |
| [strcat](#strcat)   |
| [strncat](#strncat) |
| [strxfrm](#strxfrm) |
| [strlen](#strlen)   |

## strcpy

|             |                                                                                                                                                                                                                                                                             |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                                                                                                                                                     |
| Obtains     | `char*`                                                                                                                                                                                                                                                                     |
| Description | Copies the character string pointed to by `src`, including the null terminator, to the character array whose first element is pointed to by `dest`.<br>The behavior is undefined if the `dest` array is not large enough. The behavior is undefined if the strings overlap. |

```cpp
// char* strcpy( char* dest, const char* src );

const char* src = "Hello world";

std::unique_ptr<char[]> dest = std::make_unique<char[]>(std::strlen(src) + 1);

char* pDest = std::strcpy(
  dest.get(), // dest; pointer to the character array to write to
  src // src; pointer to the null-terminated byte string to copy from
);
```

## strncpy

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Obtains     | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Description | Copies at most `count` characters of the byte string pointed to by `src` (including the terminating null character) to character array pointed to by `dest`.<br>If `count` is reached before the entire string `src` was copied, the resulting character array is not null-terminated.<br>If, after copying the terminating null character from `src`, `count` is not reached, additional null characters are written to `dest` until the total of `count` characters have been written.<br>If the strings overlap, the behavior is undefined. |

```cpp
// char* strncpy( char* dest, const char* src, std::size_t count );

const char* src = "hi";

char dest[6] = ['a', 'b', 'c', 'd', 'e', 'f'];

char* pDest = std::strncpy(
  dest, // dest; pointer to the character array to copy to
  src, // src; pointer to the byte string to copy from
  5 // count; maximum number of characters to copy
);

// dest: h i \0 \0 \0 f
```

## strcat

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Obtains     | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Description | Appends a copy of the character string pointed to by `src` to the end of the character string pointed to by `dest`. The character `src[0]` replaces the null terminator at the end of `dest`. The resulting byte string is null-terminated.<br>The behavior is undefined if the destination array is not large enough for the contents of both `src` and `dest` and the terminating null character.<br>The behavior is undefined if the strings overlap. |

```cpp
// char* strcat( char* dest, const char* src );

const char* dest = "Hello ";

char* pDest = std::strcat(
  const_cast<char*>(dest), // dest: pointer to the null-terminated byte string to append to
  "World" // src; pointer to the null-terminated byte string to copy from
);

// dest: "Hello World"
```

## strncat

|             |                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                    |
| Obtains     | `char*`                                                                                                                                                                                                                                                                                                                                                                                                              |
| Description | Appends a byte string pointed to by `src` to a byte string pointed to by `dest`. At most `count` characters are copied. The resulting byte string is null-terminated.<br>The destination byte string must have enough space for the contents of both `dest` and `src` plus the terminating null character, except that the size of `src` is limited to `count`.<br>The behavior is undefined if the strings overlap. |

```cpp
// char* strncat( char* dest, const char* src, std::size_t count );

const char* dest = "Hello ";

char* pDest = strncat(
  const_cast<char*>(dest),
  "World",
  3
);

// dest: "Hello Wor"
```

## strxfrm

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Obtains     | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Description | Transforms the null-terminated byte string pointed to by `src` into the implementation-defined form such that comparing two transformed strings with `std::strcmp` gives the same result as comparing the original strings with `std::strcoll`, in the current C locale.<br>The first count characters of the transformed string are written to destination, including the terminating null character, and the length of the full transformed string is returned, excluding the terminating null character.<br>The behavior is undefined if the `dest` array is not large enough. The behavior is undefined if `dest` and `src` overlap.<br>If count is 0, then `dest` is allowed to be a null pointer. |
| Notes       | The correct length of the buffer that can receive the entire transformed string is `1 + std::strxfrm(nullptr, src, 0)`.<br>This function is used when making multiple locale-dependent comparisons using the same string or set of strings, because it is more efficient to use std::strxfrm to transform all the strings just once, and subsequently compare the transformed strings with `std::strcmp`.                                                                                                                                                                                                                                                                                               |

```cpp
// std::size_t strxfrm( char* dest, const char* src, std::size_t count );

std::setlocale(LC_COLLATE, "cs_CZ.iso88592");

const char* src1 = "hrnec";
const char* src2 = "chrt";

size_t requiredLen1 = std::strxfrm(nullptr, src1, 0);
size_t requiredLen2 = std::strxfrm(nullptr, src2, 0);

char dest1[requiredLen1 + 1];
char dest2[requiredLen2 + 1];

std::strxfrm(dest1, src1, std::size(dest1));
std::strxfrm(dest2, src2, std::size(dest2));

// in czech locale: dest1 < dest2 (hrnec before chrt)
// in lexicographical comparison: dest2 < dest1 (chrt before hrnec)
```

## strlen

|             |                                                                                                                                                                                                                                                                                                  |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Uses        | `char*`                                                                                                                                                                                                                                                                                          |
| Obtains     | `size_t`                                                                                                                                                                                                                                                                                         |
| Description | Returns the length of the given byte string, that is, the number of characters in a character array whose first element is pointed to by str up to and not including the first null character. The behavior is undefined if there is no null character in the character array pointed to by str. |

```cpp
// std::size_t strlen( const char* str );

size_t len = std::strlen("Hello");

// len: 5
```

## strcmp

|             |                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                                                                                                                                                                                                                        |
| Obtains     | `int`                                                                                                                                                                                                                                                                                                                                          |
| Description | Compares two null-terminated byte strings lexicographically.<br>The sign of the result is the sign of the difference between the values of the first pair of characters (both interpreted as unsigned char) that differ in the strings being compared.<br>The behavior is undefined if lhs or rhs are not pointers to null-terminated strings. |

```cpp
// int strcmp( const char* lhs, const char* rhs );

int result = std::strcmp("abc", "def");

// result: -1 (abc before def)
```
