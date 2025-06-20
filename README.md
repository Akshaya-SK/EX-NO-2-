## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




## Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5
char matrix[SIZE][SIZE];

void createMatrix(char key[]) {
    int dict[26] = {0}, k = 0;
    dict['j' - 'a'] = 1;

    for (int i = 0; key[i]; i++) {
        char ch = tolower(key[i]);
        if (ch == 'j') ch = 'i';
        if (!dict[ch - 'a']) {
            matrix[k / SIZE][k % SIZE] = ch;
            dict[ch - 'a'] = 1;
            k++;
        }
    }

    for (char ch = 'a'; ch <= 'z'; ch++) {
        if (!dict[ch - 'a']) {
            matrix[k / SIZE][k % SIZE] = ch;
            k++;
        }
    }
}

void findPosition(char ch, int *row, int *col) {
    if (ch == 'j') ch = 'i';
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
}

void prepareText(char input[], char output[]) {
    int i = 0, j = 0;
    while (input[i]) {
        char a = tolower(input[i]);
        if (a < 'a' || a > 'z') {
            i++;
            continue;
        }
        if (a == 'j') a = 'i';
        output[j++] = a;
        if (input[i + 1]) {
            char b = tolower(input[i + 1]);
            if (a == b)
                output[j++] = 'x';
            else {
                output[j++] = b == 'j' ? 'i' : b;
                i++;
            }
        } else {
            output[j++] = 'x';
        }
        i++;
    }
    output[j] = '\0';
}

void encrypt(char text[]) {
    for (int i = 0; text[i] && text[i + 1]; i += 2) {
        int r1, c1, r2, c2;
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) {
            text[i] = matrix[r1][(c1 + 1) % SIZE];
            text[i + 1] = matrix[r2][(c2 + 1) % SIZE];
        } else if (c1 == c2) {
            text[i] = matrix[(r1 + 1) % SIZE][c1];
            text[i + 1] = matrix[(r2 + 1) % SIZE][c2];
        } else {
            text[i] = matrix[r1][c2];
            text[i + 1] = matrix[r2][c1];
        }
    }
}

int main() {
    char key[100], text[100], prepared[200];
    printf("Enter key: ");
    scanf("%s", key);
    printf("Enter plaintext: ");
    scanf("%s", text);

    createMatrix(key);
    prepareText(text, prepared);
    encrypt(prepared);
    printf("Encrypted Text: %s\n", prepared);
    return 0;
}
```




## Output:
![image](https://github.com/user-attachments/assets/54304e01-8b12-41f9-a7b1-972cfec95f2c)
