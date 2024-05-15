# **Day 0: Hello, World.**

最近從研究所畢業了，時間突然空了一大段出來
剛好看到HackerRank 30 天挑戰，想想不如來試試好了。

一是剛好挑戰難易度是從低到高，很適合拿來練習我一直很想學的C，也順便鞏固我的Python跟PHP
畢竟一直都是在公司框架中開發，很多寫法、宣告方式都跟原生不太一樣

二是我一直想要有個BLOG的地方，把自己的想法、遇到的困難整理起來，不過礙於自己的懶懶病、累累病一直沒有開始，趁著這個挑戰把這個習慣養成起來。



PHP:
```
<?php
$_fp = fopen("php://stdin", "r");

$inputString = fgets($_fp); // get a line of input from stdin and save it to our variable

// Your first line of output goes here
print("Hello, World.\n");
print($inputString);

// Write the second line of output

fclose($_fp);
?>
```

C:
```
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() {
    // Declare a variable named 'input_string' to hold our input.
    char input_string[105]; 
    
    // Read a full line of input from stdin and save it to our variable, input_string.
    scanf("%[^\n]", input_string); 
    
    // Print a string literal saying "Hello, World." to stdout using printf.
    printf("Hello, World.\n");
    printf(input_string);
    // TODO: Write a line of code here that prints the contents of input_string to stdout.
    
    return 0;
}

```
Python:
```
# Read a full line of input from stdin and save it to our dynamically typed variable, input_string.
input_string = input()

# Print a string literal saying "Hello, World." to stdout.
print('Hello, World.')
print(input_string)

# TODO: Write a line of code here that prints the contents of input_string to stdout.
```