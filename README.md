# COLLABORATIVE LEARNING ACTIVITY

# Problem Statement
Create a CGPA calculator using Flex and Bison that allows users to enter grades and credits for multiple semesters using a menu-driven interface and compute GPA and CGPA.
# Register number:212224040298
# Date:31.3.2026
# Aim:
To create a CGPA calculator using Flex and Bison that allows users to enter grades and credits for multiple semesters using a menu-driven interface and compute GPA and CGPA.

# Algorithm:

1.Start the program.
2.Initialize an empty symbol table.
3.Read the input string ending with ‘$’.
4.Set index to the first character.
5.Check if the current character is a variable.
6.If yes, check the next character.
7.If the next character is an operator:
8.Allocate memory using malloc.
9.Store the variable and its memory address in the symbol table.
10.Move to the next character.
11.Repeat steps 5 to 10 until ‘$’ is reached.
12.Ask the user to enter a variable to search.
13.Search the symbol table for the variable.
14.If found, display the variable and its memory address.
Else, display “Variable not found”.
15.Stop the program.

# Program:
cgp.l
```
%{
#include "calc.tab.h"
#include <stdlib.h>
%}

%%
[0-9]+          { yylval.ival = atoi(yytext); return NUMBER; }
"O"             { yylval.fval = 10.0; return GRADE; }
"A+"            { yylval.fval = 9.0;  return GRADE; }
"A"             { yylval.fval = 8.0;  return GRADE; }
"B+"            { yylval.fval = 7.0;  return GRADE; }
"B"             { yylval.fval = 6.0;  return GRADE; }
"C"             { yylval.fval = 5.0;  return GRADE; }
"P"             { yylval.fval = 4.0;  return GRADE; }
"F"             { yylval.fval = 0.0;  return GRADE; }
[ \t\n]         ; /* skip whitespace */
.               { return yytext[0]; }
%%

int yywrap() { return 1; }
```

cgp.y
```
%{
#include <stdio.h>
#include <stdlib.h>

float total_grade_points = 0;
int total_credits = 0;
float semester_gpa = 0;
float total_sem_gpa = 0;
int semester_count = 0;

void yyerror(const char *s);
int yylex();
%}

%union {
    int grade;
    int credit;
}

%token <grade> GRADE
%token <credit> NUMBER

%%
menu:
    command menu

    | /* empty */
    ;

command:
    '1' GRADE NUMBER { 
        /* MENU1: Add subject */
        total_grade_points += ($2 * $3);
        total_credits += $3;
        printf("Added subject: Grade Point %d, Credits %d\n", $2, $3);
    }
    | '2' { 
        /* MENU2: Next semester / Compute Sem GPA */
        if (total_credits > 0) {
            semester_gpa = total_grade_points / total_credits;
            total_sem_gpa += semester_gpa;
            semester_count++;
            printf("\n--- Semester %d GPA: %.2f ---\n", semester_count, semester_gpa);
            // Reset for next semester
            total_grade_points = 0;
            total_credits = 0;
        } else {
            printf("No data entered for this semester.\n");
        }
    }
    | '3' { 
        /* MENU3: Finish / Compute CGPA */
        if (semester_count > 0) {
            printf("\nFinal CGPA: %.2f\n", total_sem_gpa / semester_count);
        }
        exit(0);
    }
    ;

%%

void yyerror(const char *s) {
    fprintf(stderr, "Error: %s\n", s);
}

int main() {
    printf("CGPA Calculator Menu:\n");
    printf("1 <Grade> <Credits> - Add Subject\n");
    printf("2 - Calculate Semester GPA\n");
    printf("3 - Calculate Final CGPA and Exit\n");
    yyparse();
    return 0;
}
```
# Output:
<img width="973" height="680" alt="image" src="https://github.com/user-attachments/assets/ea2f06f2-f038-4d7b-bf07-47a99b5b13a9" />

# Result:
A LEX and YACC program for  creating a CGPA calculator using Flex and Bison that allows users to enter grades and credits for multiple semesters using a menu-driven interface and compute GPA and CGPA.
