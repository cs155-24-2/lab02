# CS 155 24.2 Lab 02

## Instructions

1. This is an ungraded exercise that you would still be required to submit.

2. Specs are uploaded in our Google Classroom (although the pdf file says that it is Lab 3, this is actually Lab 0e in our case since we started with Lab 00).

3. Just a few hints, suppose that you declare a token ```WORD``` in your bison ```.y``` parser and you want to assign to this token string values (i.e. ```char *``` pointers). 

There are many ways to do this, but one way is to cast ```yylval``` as a ```union``` of attributes by including the following in the first section (i.e. before the first ```%%``` in your bison file):

```
%union {
   char* s;
}
```

And you declare the token ```WORD``` in your bison file as:

```
%token <s> WORD
```

Now, whenever flex recognizes a string that matches your assigned regex to ```WORD```, to assign this string to the ```s``` attribute of ```WORD```, you may do something like this in your flex lexer file:

```
[a-zA-Z]+   {yylval.s = strdup(yytext); return WORD;}
```

The code above sets the ```s``` attribute of ```WORD``` (found in ```yylval.s```) to the string stored in ```yytext``` that matches the regex ```[a-zA-Z]+```. 

By setting ```yylval``` as a union, everytime you fetch its values, you would automatically get a ```char *```. For instance, suppose that you have a C function ```void f(const char *s)``` in your bison parser and that everytime your parse tree encounters ```WORD```, it executes ```f``` whose input is the string contained in ```WORD```, you simply can write something like the following code in the respective parse tree of your bison file:

```
tree :
    | tree WORD {f($2);} 
```

The code above fetches the ```s``` attribute of ```WORD``` by calling ```$2```.

