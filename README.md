## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

### DEVELOPER: ADITHYA NM
### REG NO: 212225040011
 

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

char table[5][5];

void createTable(char key[]) {
    int used[26] = {0}, k = 0;
    used['J' - 'A'] = 1; // 'J' is treated as 'I'

    for (int i = 0; key[i]; i++) {
        char c = toupper(key[i]);
        if (isalpha(c) && !used[c - 'A']) {
            table[k / 5][k % 5] = c;
            used[c - 'A'] = 1;
            k++;
        }
    }

    for (int i = 0; i < 26; i++) {
        if (!used[i]) {
            table[k / 5][k % 5] = 'A' + i;
            k++;
        }
    }
}

void getPos(char ch, int *r, int *c) {
    if (ch == 'J') ch = 'I';
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (table[i][j] == ch) {
                *r = i;
                *c = j;
                return;
            }
        }
    }
}

void processText(char text[], char clean[]) {
    int len = 0;
    for (int i = 0; text[i]; i++) {
        if (isalpha(text[i])) {
            clean[len++] = toupper(text[i]) == 'J' ? 'I' : toupper(text[i]);
        }
    }

    int k = 0;
    for (int i = 0; i < len; i++) {
        clean[k++] = clean[i];
        if (i + 1 < len && clean[i] == clean[i + 1]) {
            clean[k++] = 'X';
        }
    }
    if (k % 2 != 0) clean[k++] = 'X';
    clean[k] = '\0';
}

void cipher(char text[], int dir) {
    int r1, c1, r2, c2;
    for (int i = 0; text[i]; i += 2) {
        getPos(text[i], &r1, &c1);
        getPos(text[i + 1], &r2, &c2);

        if (r1 == r2) { // Same Row
            text[i] = table[r1][(c1 + dir + 5) % 5];
            text[i + 1] = table[r2][(c2 + dir + 5) % 5];
        } else if (c1 == c2) { // Same Column
            text[i] = table[(r1 + dir + 5) % 5][c1];
            text[i + 1] = table[(r2 + dir + 5) % 5][c2];
        } else { // Rectangle Rule
            text[i] = table[r1][c2];
            text[i + 1] = table[r2][c1];
        }
    }
}

int main() {
    char key[100], text[200], clean[200];

    printf("Enter Key: ");
    scanf("%s", key);
    printf("Enter Plaintext: ");
    scanf("%s", text);

    createTable(key);
    processText(text, clean);

    printf("\nKey Table:\n");
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) printf("%c ", table[i][j]);
        printf("\n");
    }

    cipher(clean, 1);
    printf("\nEncrypted: %s\n", clean);

    cipher(clean, -1);
    printf("Decrypted: %s\n", clean);

    return 0;
}
```

## Output:

<img width="365" height="310" alt="image" src="https://github.com/user-attachments/assets/df5156e9-0163-4eae-b77e-d99d34961725" />


## Result:
Thus, the C program to implement the Playfair Cipher substitution technique was successfully compiled and executed, and the plaintext was correctly encrypted and decrypted.





