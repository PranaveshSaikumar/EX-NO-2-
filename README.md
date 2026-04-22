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
5.	
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

char keyT[5][5];

// Generate key table
void generateKeyTable(char key[]) {
    int used[26] = {0};
    int i, j, k = 0;

    used['j' - 'a'] = 1;

    for(i = 0; key[i]; i++) {
        char ch = tolower(key[i]);
        if(ch >= 'a' && ch <= 'z' && !used[ch - 'a']) {
            keyT[k / 5][k % 5] = ch;
            used[ch - 'a'] = 1;
            k++;
        }
    }

    for(i = 0; i < 26; i++) {
        if(!used[i]) {
            keyT[k / 5][k % 5] = i + 'a';
            k++;
        }
    }
}

// Find position of character
void findPos(char ch, int *r, int *c) {
    if(ch == 'j') ch = 'i';
    for(int i = 0; i < 5; i++) {
        for(int j = 0; j < 5; j++) {
            if(keyT[i][j] == ch) {
                *r = i; *c = j;
            }
        }
    }
}

// Prepare text
void prepare(char str[]) {
    int i, j = 0;
    char temp[100];

    for(i = 0; str[i]; i++) {
        if(isalpha(str[i])) {
            temp[j++] = tolower(str[i]);
        }
    }

    temp[j] = '\0';
    strcpy(str, temp);

    for(i = 0; i < j; i += 2) {
        if(str[i] == str[i+1]) {
            for(int k = j; k > i+1; k--)
                str[k] = str[k-1];
            str[i+1] = 'x';
            j++;
        }
    }

    if(j % 2 != 0) {
        str[j++] = 'x';
    }

    str[j] = '\0';
}

// Encrypt
void encrypt(char str[]) {
    int r1, c1, r2, c2;

    for(int i = 0; str[i]; i += 2) {
        findPos(str[i], &r1, &c1);
        findPos(str[i+1], &r2, &c2);

        if(r1 == r2) {
            str[i]   = keyT[r1][(c1 + 1) % 5];
            str[i+1] = keyT[r2][(c2 + 1) % 5];
        }
        else if(c1 == c2) {
            str[i]   = keyT[(r1 + 1) % 5][c1];
            str[i+1] = keyT[(r2 + 1) % 5][c2];
        }
        else {
            str[i]   = keyT[r1][c2];
            str[i+1] = keyT[r2][c1];
        }
    }
}

// Decrypt
void decrypt(char str[]) {
    int r1, c1, r2, c2;

    for(int i = 0; str[i]; i += 2) {
        findPos(str[i], &r1, &c1);
        findPos(str[i+1], &r2, &c2);

        if(r1 == r2) {
            str[i]   = keyT[r1][(c1 + 4) % 5];
            str[i+1] = keyT[r2][(c2 + 4) % 5];
        }
        else if(c1 == c2) {
            str[i]   = keyT[(r1 + 4) % 5][c1];
            str[i+1] = keyT[(r2 + 4) % 5][c2];
        }
        else {
            str[i]   = keyT[r1][c2];
            str[i+1] = keyT[r2][c1];
        }
    }
}

int main() {
    char text[100], key[100], original[100];

    printf("Enter key: ");
    scanf("%s", key);

    printf("Enter plaintext: ");
    scanf("%s", text);

    strcpy(original, text);

    generateKeyTable(key);
    prepare(text);

    encrypt(text);
    printf("Encrypted text: %s\n", text);

    decrypt(text);
    printf("Decrypted text: %s\n", text);

    return 0;
}
```

## Output:
<img width="262" height="198" alt="image" src="https://github.com/user-attachments/assets/b75eeb50-aaad-4f33-8ebc-94e372d30ed4" />

## Result: 
Thus the implementation of Playfair cipher had been executed successfully.
