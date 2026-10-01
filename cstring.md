# cstring

## Functions

|                                     |
| ----------------------------------- |
| [strcpy](#strcpy)                   |
| [strncpy](#strncpy)                 |
| [strcat](#strcat)                   |
| [strncat](#strncat)                 |
| [strxfrm](#strxfrm)                 |
| [strlen](#strlen)                   |
| [strcmp](#strcmp)                   |
| [strncmp](#strncmp)                 |
| [strcoll](#strcoll)                 |
| [strchr](#strchr)                   |
| [strrchr](#strrchr)                 |
| [strspn](#strspn)                   |
| [strcspn](#strcspn)                 |
| [strpbrk](#strpbrk)                 |
| [strstr](#strstr)                   |
| [strtok](#strtok)                   |
| [memchr](#memchr)                   |
| [memcmp](#memcmp)                   |
| [memset](#memset)                   |
| [memset_explicit](#memset_explicit) |
| [memcpy](#memcpy)                   |
| [memmove](#memmove)                 |
| [strerror](#strerror)               |

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
| Notes       | Because strcat needs to seek to the end of dest on each call, it is inefficient to concatenate many strings into one using strcat.                                                                                                                                                                                                                                                                                                                       |

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
| Notes       | Because strncat needs to seek to the end of dest on each call, it is inefficient to concatenate many strings into one using strncat.                                                                                                                                                                                                                                                                                 |

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

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Obtains     | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Description | Transforms the null-terminated byte string pointed to by `src` into the implementation-defined form such that comparing two transformed strings with [strcmp](#strcmp) gives the same result as comparing the original strings with [strcoll](#strcoll), in the current C locale.<br>The first count characters of the transformed string are written to destination, including the terminating null character, and the length of the full transformed string is returned, excluding the terminating null character.<br>The behavior is undefined if the `dest` array is not large enough. The behavior is undefined if `dest` and `src` overlap.<br>If count is 0, then `dest` is allowed to be a null pointer. |
| Notes       | The correct length of the buffer that can receive the entire transformed string is `1 + std::strxfrm(nullptr, src, 0)`.<br>This function is used when making multiple locale-dependent comparisons using the same string or set of strings, because it is more efficient to use std::strxfrm to transform all the strings just once, and subsequently compare the transformed strings with [strcmp](#strcmp).                                                                                                                                                                                                                                                                                                    |

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

|             |                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                                                                                                                                                                                                                            |
| Obtains     | `int`                                                                                                                                                                                                                                                                                                                                              |
| Description | Compares two null-terminated byte strings lexicographically.<br>The sign of the result is the sign of the difference between the values of the first pair of characters (both interpreted as unsigned char) that differ in the strings being compared.<br>The behavior is undefined if `lhs` or `rhs` are not pointers to null-terminated strings. |

```cpp
// int strcmp( const char* lhs, const char* rhs );

int result = std::strcmp("abc", "def");

// result: -1 (abc before def)
```

## strncmp

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Obtains     | `int`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Description | Compares at most `count` characters of two possibly null-terminated arrays. The comparison is done lexicographically. Characters following the null character are not compared.<br>The sign of the result is the sign of the difference between the values of the first pair of characters (both interpreted as unsigned char) that differ in the arrays being compared.<br>The behavior is undefined when access occurs past the end of either array `lhs` or `rhs`. The behavior is undefined when either `lhs` or `rhs` is the null pointer. |
| Notes       | This function is not locale-sensitive, unlike [strcoll](#strcoll) and [strxfrm](#strxfrm).                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

```cpp
// int strncmp( const char* lhs, const char* rhs, std::size_t count );

int result = std::strncmp("Hello world", "Hello everyone", 6);

// result: 0 (the first 6 characters of the strings are equal)
```

## strcoll

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Uses        | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Obtains     | `int`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Description | Compares two null-terminated byte strings according to the current locale as defined by the `LC_COLLATE` category.                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Notes       | Collation order is the dictionary order: the position of the letter in the national alphabet (its equivalence class) has higher priority than its case or variant. Within an equivalence class, lowercase characters collate before their uppercase equivalents and locale-specific order may apply to the characters with diacritics. In some locales, groups of characters compare as single collation units. For example, `"ch"` in Czech follows `"h"` and precedes `"i"`, and `"dzs"` in Hungarian follows `"dz"` and precedes `"g"`. |

```cpp
// int strcoll( const char* lhs, const char* rhs );

std::setlocale(LC_COLLATE, "cs_CZ.utf8");

int result = std::strcoll("hrnec", "chrt");

// in czech locale: hrnec before chrt
// in lexicographical comparison: chrt before hrnec
```

## strchr

|             |                                                                                                                                                                                                                                  |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`, `int`                                                                                                                                                                                                                   |
| Obtains     | `char*`                                                                                                                                                                                                                          |
| Description | Finds the first occurrence of the character `static_cast<char>(ch)` in the byte string pointed to by `str`.<br>The terminating null character is considered to be a part of the string and can be found if searching for `'\0'`. |

```cpp
// const char* strchr( const char* str, int ch );
//       char* strchr(       char* str, int ch );

const char* result = std::strchr(
  "Hello world",  // pointer to the null-terminated byte string to be analyzed
  'o' // character to search for
);

// result: points to 'o' char at index 4
```

## strrchr

|             |                                                                                                                                                                                                                        |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`, `int`                                                                                                                                                                                                         |
| Obtains     | `char*`                                                                                                                                                                                                                |
| Description | Finds the last occurrence of `ch` (after conversion to char) in the byte string pointed to by `str`. The terminating null character is considered to be a part of the string and can be found if searching for `'\0'`. |

```cpp
// const char* strrchr( const char* str, int ch );
//       char* strrchr(       char* str, int ch );

const char* result = std::strrchr(
  "Hello world", // pointer to the null-terminated byte string to be analyzed
  'o' // character to search for
);

// result: points to 'o' char at index 7
```

## strspn

|             |                                                                                                                                                                                  |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                                                          |
| Obtains     | `size_t`                                                                                                                                                                         |
| Description | Returns the length of the maximum initial segment (span) of the byte string pointed to by `dest`, that consists of only the characters found in byte string pointed to by `src`. |

```cpp
// size_t strspn( const char* dest, const char* src );

const char* dest = "abcde312$#@";

const char* src = "qwertyuiopasdfghjklzxcvbnm";

size_t span = std::strspn(
  dest, // pointer to the null-terminated byte string to be analyzed
  src // pointer to the null-terminated byte string that contains the characters to search for
);

// span: 5 (abcde matched)
```

## strcspn

|             |                                                                                                                                                                                                                                     |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                                                                                                             |
| Obtains     | `size_t`                                                                                                                                                                                                                            |
| Description | Returns the length of the maximum initial segment of the byte string pointed to by `dest`, that consists of only the characters not found in byte string pointed to by `src`.<br>The function name stands for "complementary span". |

```cpp
// std::size_t strcspn( const char *dest, const char *src );

const char* dest = "abcde312$#@";

const char* src = "$#";

size_t span = std::strcspn(
  dest, // pointer to the null-terminated byte string to be analyzed
  src // pointer to the null-terminated byte string that contains the characters to search for
);

// span: 8 (abcde312 matched)
```

## strpbrk

|             |                                                                                                                                                                                      |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Uses        | `char*`                                                                                                                                                                              |
| Obtains     | `char*`                                                                                                                                                                              |
| Description | Scans the null-terminated byte string pointed to by `dest` for any character from the null-terminated byte string pointed to by `breakset`, and returns a pointer to that character. |
| Notes       | The name stands for "string pointer break", because it returns a pointer to the first of the separator ("break") characters.                                                         |

```cpp
// const char* strpbrk( const char* dest, const char* breakset );
//       char* strpbrk(       char* dest, const char* breakset );

const char* dest = "Hello world";
const char* breakset = "dlr";

char* result = std::strpbrk(
  dest, // pointer to the null-terminated byte string to be analyzed
  breakset // pointer to the null-terminated byte string that contains the characters to search for
);

// result: points to first 'l' char (index 2)
```

## strstr

|             |                                                                                                                                                       |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                               |
| Obtains     | `char*`                                                                                                                                               |
| Description | Finds the first occurrence of the byte string `needle` in the byte string pointed to by `haystack`. The terminating null characters are not compared. |

```cpp
// const char* strstr( const char* haystack, const char* needle );
//       char* strstr(       char* haystack, const char* needle );

const char* result = std::strstr(
  "Hello world", // pointer to the null-terminated byte string to examine
  "world" // pointer to the null-terminated byte string to search for
);

// result: points to 'w' char (index 6)
```

## strtok

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Obtains     | `char*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Description | Tokenizes a null-terminated byte string.<br>A sequence of calls to strtok breaks the string pointed to by `str` into a sequence of tokens, each of which is delimited by a character from the string pointed to by `delim`. Each call in the sequence has a search target :<br>- If `str` is non-null, the call is the first call in the sequence. The search target is null-terminated byte string pointed to by `str`.<br>- If `str` is null, the call is one of the subsequent calls in the sequence. The search target is determined by the previous call in the sequence.<br><br>Each call in the sequence searches the search target for the first character that is not contained in the separator string pointed to by `delim`, the separator string can be different from call to call.<br>- If no such character is found, then there are no tokens in the search target. The search target for the next call in the sequence is unchanged.<br>- If such a character is found, it is the start of the current token. strtok then searches from there for the first character that is contained in the separator string.<br>a. If no such character is found, the current token extends to the end of search target. The search target for the next call in the sequence is an empty string.<br>b. If such a character is found, it is overwritten by a null character, which terminates the current token. The search target for the next call in the sequence starts from the following character.<br>If `str` or `delim` is not a pointer to a null-terminated byte string, the behavior is undefined.<br>1. A token may still be formed in a subsequent call with a different separator string.<br>2. No more tokens can be formed in subsequent calls. |
| Notes       | This function is destructive: it writes the `'\0'` characters in the elements of the string str. In particular, a string literal cannot be used as the first argument of strtok.<br>Each call to this function modifies a static variable: is not thread safe.<br>Unlike most other tokenizers, the delimiters in strtok can be different for each subsequent token, and can even depend on the contents of the previous tokens.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

```cpp
// char* strtok( char* str, const char* delim );

char str[] = "one + two * (three - four)!";
const char* delim = "! +- (*)";

char* token = std::strtok(input, delimiters);

while (token)
{
  std::cout << std::quoted(token) << ' ';

  token = std::strtok(nullptr, delimiters);
}

// token after each call: "one" "two" "three" "four"

// final str: "one\0+ two\0* (three\0- four\0!\0"
```

## memchr

|             |                                                                                                                                                                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `void*`, `int`, `size_t`                                                                                                                                                                                                                     |
| Obtains     | `void*`                                                                                                                                                                                                                                      |
| Description | Converts `ch` to unsigned char and locates the first occurrence of that value in the initial `count` bytes (each interpreted as unsigned char) of the object pointed to by `ptr`.                                                            |
| Notes       | This function behaves as if it reads the bytes sequentially and stops as soon as a matching bytes is found: if the array pointed to by `ptr` is smaller than `count`, but the match is found within the array, the behavior is well-defined. |

```cpp
// const void* memchr( const void* ptr, int ch, std::size_t count );
//       void* memchr(       void* ptr, int ch, std::size_t count );

char arr[] = {'a', '\0', 'a', 'A', 'a', 'a', 'A', 'a'};

char *pc = static_cast<char*>(
  std::memchr(
    arr, // pointer to the object to be examined
    'A', // byte to search for
    sizeof(arr) // max number of bytes to examine
  )
);

// pc: points to char 'A' at index 3 of the array
```

## memcmp

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `void*`, `void*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Obtains     | `int`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Description | Reinterprets the objects pointed to by `lhs` and `rhs` as arrays of unsigned char and compares the first count bytes of these arrays. The comparison is done lexicographically.<br>The sign of the result is the sign of the difference between the values of the first pair of bytes (both interpreted as unsigned char) that differ in the objects being compared.                                                                                                                                                                                       |
| Notes       | This function reads object representations, not the object values, and is typically meaningful for only trivially-copyable objects that have no padding. For example, memcmp() between two objects of type `std::string` or `std::vector` will not compare their contents, memcmp() between two objects of type `struct { char c; int n; }` will compare the padding bytes whose values may differ when the values of `c` and `n` are the same, and even if there were no padding bytes, the int would be compared without taking into account endianness. |

```cpp
// int memcmp( const void* lhs, const void* rhs, std::size_t count );

char a1[] = {'a', 'b', 'd'};
char a2[] = {'a', 'b', 'c'};

int result = std::memcmp(a1, a2, 3);

// result: 1 (a1 follows a2 in lexicographical order)
```

## memset

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `void*`, `int`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Obtains     | `void*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Description | Copies the value `static_cast<unsigned char>(ch)` into each of the first count characters of the object pointed to by `dest`.<br>Invoking [memset_explicit](#memset_explicit) always results in a memory store (i.e. never elided), regardless of optimizations. (since C++26)<br>If the object pointed to by `dest` satisfies any of the following conditions, the behavior is undefined:<br>- The object is a potentially-overlapping subobject.<br>- The object is not TriviallyCopyable.<br>- count is greater than the size of the object.                                                |
| Notes       | memset may be optimized away (under the as-if rules) if the object modified by this function is not accessed again for the rest of its lifetime (e.g., gcc bug 8537). For that reason, this function cannot be used to scrub memory (e.g., to fill an array that stored a password with zeroes).<br>This optimization is prohibited for [memset_explicit](#memset_explicit): they are guaranteed to perform the memory write. Before C++26,`std::fill` can be used with volatile pointers.<br>Third-party solutions for that include FreeBSD `explicit_bzero` or Microsoft `SecureZeroMemory`. |

```cpp
// void* memset( void* dest, int ch, std::size_t count );

int a[4];

void* pDest = std::memset(a, 0b1111'0000'0011, sizeof a);

using bits = std::bitset<sizeof(int) * CHAR_BIT>;

for (int ai : a) {
  std::cout << bits(ai) << '\n';
}

// Output:
// 00000011000000110000001100000011
// 00000011000000110000001100000011
// 00000011000000110000001100000011
// 00000011000000110000001100000011
```

## memset_explicit

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `void*`, `int`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Obtains     | `void*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Description | Copies the value `static_cast<unsigned char>(ch)` into each of the first count characters of the object pointed to by `dest`.<br>Invoking [memset_explicit](#memset_explicit) always results in a memory store (i.e. never elided), regardless of optimizations. (since C++26)<br>If the object pointed to by `dest` satisfies any of the following conditions, the behavior is undefined:<br>- The object is a potentially-overlapping subobject.<br>- The object is not TriviallyCopyable.<br>- count is greater than the size of the object.                                                |
| Notes       | memset may be optimized away (under the as-if rules) if the object modified by this function is not accessed again for the rest of its lifetime (e.g., gcc bug 8537). For that reason, this function cannot be used to scrub memory (e.g., to fill an array that stored a password with zeroes).<br>This optimization is prohibited for [memset_explicit](#memset_explicit): they are guaranteed to perform the memory write. Before C++26,`std::fill` can be used with volatile pointers.<br>Third-party solutions for that include FreeBSD `explicit_bzero` or Microsoft `SecureZeroMemory`. |

```cpp
// void* memset_explicit( void* dest, int ch, std::size_t count );

int a[4];

void* pDest = std::memset_explicit(a, 0b1111'0000'0011, sizeof a);

using bits = std::bitset<sizeof(int) * CHAR_BIT>;

for (int ai : a) {
  std::cout << bits(ai) << '\n';
}

// Output:
// 00000011000000110000001100000011
// 00000011000000110000001100000011
// 00000011000000110000001100000011
// 00000011000000110000001100000011
```

## memcpy

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uses        | `void*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Obtains     | `void*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Description | Performs the following operations in order:<br>1. Implicitly creates objects at `dest`.<br>2. Copies `count` characters (as if of type unsigned char) from the object pointed to by `src` into the object pointed to by `dest`.<br>If any of the following conditions is satisfied, the behavior is undefined:<br>- `dest` or `src` is a null pointer or invalid pointer.<br>- Copying takes place between objects that overlap.                                                 |
| Notes       | memcpy is meant to be the fastest library routine for memory-to-memory copy. It is usually more efficient than [strcpy](#strcpy), which must scan the data it copies or [memmove](#memmove), which must take precautions to handle overlapping inputs.<br>Several C++ compilers transform suitable memory-copying loops to memcpy calls.<br>Where strict aliasing prohibits examining the same memory as values of two different types, memcpy may be used to convert the values. |

```cpp
// void* memcpy( void* dest, const void* src, std::size_t count );

char dest[4];
char src[] = "once upon a daydream...";

void* pDest = std::memcpy(dest, src, sizeof dest);

// dest: {'o', 'n', 'c', 'e'}
```

## memmove

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Uses        | `void*`, `size_t`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Obtains     | `void*`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Description | Performs the following operations in order:<br>1. Copies `count` characters (as if of type unsigned char, the same below) from the object pointed to by `src` into a temporary array `arr` of `count` characters, where `arr` does not overlap the objects pointed to by `dest` and `src`.<br>2. Implicitly creates objects at `dest`.<br>3. Copies `count` characters from `arr` into the object pointed to by `dest`.<br>If `dest` or `src` is a null pointer or invalid pointer, the behavior is undefined.                                                                                                                                   |
| Notes       | Despite the specification says a temporary buffer is used, actual implementations of this function do not incur the overhead of double copying or extra memory. For small count, it may load up and write out registers; for larger blocks, a common approach (glibc and bsd libc) is to copy bytes forwards from the beginning of the buffer if the destination starts before the source, and backwards from the end otherwise, with a fall back to std::memcpy when there is no overlap at all.<br>Where strict aliasing prohibits examining the same memory as values of two different types, std::memmove may be used to convert the values. |

```cpp
// void* memmove( void* dest, const void* src, std::size_t count );

char str[] = "1234567890";

// copies from [4, 5, 6] to [5, 6, 7]
void* pDest = std::memmove(str + 4, str + 3, 3);

// str: 1234456890
```

## strerror

|||
|-|-|
|Uses| `int` |
|Obtains | `char*` |
|Description | Returns a pointer to the textual description of the system error code `errnum`, identical to the description that would be printed by `std::perror`.<br>`errnum` is usually acquired from the `errno` variable, however the function accepts any value of type int. The contents of the string are locale-specific.<br>The returned string must not be modified by the program, but may be overwritten by a subsequent call to the strerror function. strerror is not required to be thread-safe. Implementations may be returning different pointers to static read-only string literals or may be returning the same pointer over and over, pointing at a static buffer in which strerror places the string.  | 
|Notes | POSIX allows subsequent calls to strerror to invalidate the pointer value returned by an earlier call. It also specifies that it is the `LC_MESSAGES` locale facet that controls the contents of these messages.<br>POSIX has a thread-safe version called `strerror_r` defined. Glibc defines an incompatible version.  | 

```cpp
// char* strerror( int errnum );

const double not_a_number = std::log(-1.0);

// errno comes from <cerrno>
if (errno == EDOM)
{
  std::cout << "Error: " << std::strerror(errno) << '\n';

  std::setlocale(LC_MESSAGES, "de_DE.utf8");
  std::cout << "Error in German, " << std::strerror(errno) << '\n';
}

// Output:
// Error: Numerical argument out of domain
// Error in German: Das numerische Argument ist ausserhalb des Definitionsbereiches
```