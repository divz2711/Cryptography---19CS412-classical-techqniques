# Cryptography---19CS412-classical-techqniques
### DATE: 22.02.2023
### REGISTER NUMBER : 212221040042

# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To develop a simple C program to implement Caeser Cipher.

## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>

void caesarEncrypt(char *text, int key) {
    for (int i = 0; text[i] != '\0'; i++) {
        char c = text[i];
        // Check if the character is an uppercase letter
        if (c >= 'A' && c <= 'Z') {
            text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
        }
        // Check if the character is a lowercase letter
        else if (c >= 'a' && c <= 'z') {
            text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
        }
    }
}
void caesarDecrypt(char *text, int key) {
    // Decryption is the same as encryption with a negative key
    caesarEncrypt(text, -key);
}

int main() {
    char message[100]; // Declare a character array to store the message
    int key;
    
    printf("Enter the message to encrypt: ");
    fgets(message, sizeof(message), stdin); // Read input from the user
    
    printf("Enter the Caesar Cipher key (an integer): ");
    scanf("%d", &key); // Read the key from the user
    
    // Encrypt the message using the Caesar Cipher
    caesarEncrypt(message, key);
    printf("Encrypted Message: %s", message);
    
    // Decrypt the message back to the original
    caesarDecrypt(message, key);
    printf("Decrypted Message: %s", message);
    
    return 0;
}

```
## OUTPUT:
```
Enter the message to encrypt: TECH COMMUNITY
Enter the Caesar Cipher key (an integer): 3
Encrypted Message: WHFK FRPPXQLWB
Decrypted Message: TECH COMMUNITY
```

## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To develop a simple C program to implement PlayFair Cipher.

## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 30
// Function to convert the string to lowercase
void toLowerCase(char plain[], int ps) {
    int i;
    for (i = 0; i < ps; i++) {
        if (plain[i] > 64 && plain[i] < 91)
            plain[i] += 32;
    }
}

// Function to remove all spaces in a string
int removeSpaces(char* plain, int ps) {
    int i, count = 0;
    for (i = 0; i < ps; i++)
        if (plain[i] != ' ')
            plain[count++] = plain[i];
    plain[count] = '\0';
    return count;
}
void generateKeyTable(char key[], int ks, char keyT[5][5]) {
    int i, j, k, flag = 0, *dicty;
    // a 26 character hashmap
    // to store count of the alphabet
    dicty = (int*)calloc(26, sizeof(int));
    for (i = 0; i < ks; i++) {
        if (key[i] != 'j')
            dicty[key[i] - 97] = 2;
    }
    dicty['j' - 97] = 1;
    i = 0;
    j = 0;
    for (k = 0; k < ks; k++) {
        if (dicty[key[k] - 97] == 2) {
            dicty[key[k] - 97] -= 1;
            keyT[i][j] = key[k];
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
    for (k = 0; k < 26; k++) {
        if (dicty[k] == 0) {
            keyT[i][j] = (char)(k + 97);
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
}

// Function to search for the characters of a digraph
// in the key square and return their position
void search(char keyT[5][5], char a, char b, int arr[]) {
    int i, j;
    if (a == 'j')
        a = 'i';
    else if (b == 'j')
        b = 'i';
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            if (keyT[i][j] == a) {
                arr[0] = i;
                arr[1] = j;
            }
            else if (keyT[i][j] == b) {
                arr[2] = i;
                arr[3] = j;
            }
        }
    }
}

// Function to find the modulus with 5
int mod5(int a) {
    return (a % 5);
}

// Function to make the plain text length to be even
int prepare(char str[], int ptrs) {
    if (ptrs % 2 != 0) {
        str[ptrs++] = 'z';
        str[ptrs] = '\0';
    }
    return ptrs;
}

// Function for performing the encryption
void encrypt(char str[], char keyT[5][5], int ps) {
    int i, a[4];
    for (i = 0; i < ps; i += 2) {
        search(keyT, str[i], str[i + 1], a);
        if (a[0] == a[2]) {
            str[i] = keyT[a[0]][mod5(a[1] + 1)];
            str[i + 1] = keyT[a[0]][mod5(a[3] + 1)];
        }
        else if (a[1] == a[3]) {
            str[i] = keyT[mod5(a[0] + 1)][a[1]];
            str[i + 1] = keyT[mod5(a[2] + 1)][a[1]];
        }
        else {
            str[i] = keyT[a[0]][a[3]];
            str[i + 1] = keyT[a[2]][a[1]];
        }
    }
}

// Function to encrypt using Playfair Cipher
void encryptByPlayfairCipher(char str[], char key[]) {
    char ps, ks, keyT[5][5];
    // Key
    ks = strlen(key);
    ks = removeSpaces(key, ks);
    toLowerCase(key, ks);
    // Plaintext
    ps = strlen(str);
    toLowerCase(str, ps);
    ps = removeSpaces(str, ps);
    ps = prepare(str, ps);
    generateKeyTable(key, ks, keyT);
    encrypt(str, keyT, ps);
}

// Driver code
int main() {
    char str[SIZE], key[SIZE];
    // Key to be encrypted
    strcpy(key, "HARDWARE");
    printf("Key text: %s\n", key);
    // Plaintext to be encrypted
    strcpy(str, "SOFTWARE ENGINEERING");
    printf("Plain text: %s\n", str);
    // encrypt using Playfair Cipher
    encryptByPlayfairCipher(str, key);
    printf("Cipher text: %s\n", str);
    return 0;
}

```
## OUTPUT:
```
Key text: HARDWARE
Plain text: SOFTWARE ENGINEERING
Cipher text: tpgshrhcgienigchkinw
```

## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

char* encode(char a, char b, char c) {
    static char ret[4];
    int x, y, z;
    int posa = (int)a - 65;
    int posb = (int)b - 65;
    int posc = (int)c - 65;
    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];
    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
    return ret;
}

char* decode(char a, char b, char c) {
    static char ret[4];
    int x, y, z;
    int posa = (int)a - 65;
    int posb = (int)b - 65;
    int posc = (int)c - 65;
    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];
    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
    return ret;
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    int n;

    strcpy(msg, "cryptocurrency");

    printf("Simulation of Hill Cipher\n");
    printf("Input message : %s\n", msg);

    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Remove spaces
    n = strlen(msg) % 3;
    // Append padding text X
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }
    
    printf("Padded message : %s\n", msg);

    for (int i = 0; i < strlen(msg); i += 3) {
        char a = msg[i];
        char b = msg[i + 1];
        char c = msg[i + 2];
        strcat(enc, encode(a, b, c));
    }
    printf("Encoded message : %s\n", enc);

    for (int i = 0; i < strlen(enc); i += 3) {
        char a = enc[i];
        char b = enc[i + 1];
        char c = enc[i + 2];
        strcat(dec, decode(a, b, c));
    }
    printf("Decoded message : %s\n", dec);
    return 0;
}

```

## OUTPUT:
```
Simulation of Hill Cipher
Input message : cryptocurrency
Padded message : CRYPTOCURRENCYX
Encoded message : GZIDLPYUHZUMSSV
Decoded message : CRYPTOCURRENCYX
```

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:

```
#include <stdio.h>
#include <string.h>

// Function to perform Vigenere encryption
void vigenereEncrypt(char* text, const char* key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);
    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + key[i % keyLen] - 'A') % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + key[i % keyLen] - 'A') % 26) + 'a';
        }
    }
}

// Function to perform Vigenère decryption
void vigenereDecrypt(char* text, const char* key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);
    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "KEY"; // Replace with your desired key
    char message[] = "This is a secret message."; // Replace with your message
    
    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);
    
    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);
    
    return 0;
}

```

## OUTPUT:
```
Encrypted Message: Dlgc gc y wcmvcd kowqkkc.
Decrypted Message: This is a secret message.
```

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <stdbool.h>

// Function to encrypt a message using Rail Fence Cipher
char* encryptRailFence(char* text, int key) {
    int len = strlen(text);
    char rail[key][len];

    // Filling the rail matrix to distinguish filled spaces from blank ones
    for (int i = 0; i < key; i++) {
        for (int j = 0; j < len; j++) {
            rail[i][j] = '\0';
        }
    }

    // Finding the direction
    bool dir_down = false;
    int row = 0, col = 0;

    // Filling the rail matrix with the text
    for (int i = 0; i < len; i++) {
        // Check the direction of flow and adjust the row and column accordingly
        if (row == 0 || row == key - 1) {
            dir_down = !dir_down;
        }

        // Fill the rail matrix with characters from the text
        rail[row][col++] = text[i];

        // Find the next row using direction flag
        dir_down ? row++ : row--;
    }

    // Construct the encrypted message
    char* result = malloc(len + 1);
    int k = 0;
    for (int i = 0; i < key; i++) {
        for (int j = 0; j < len; j++) {
            if (rail[i][j] != '\0') {
                result[k++] = rail[i][j];
            }
        }
    }
    result[k] = '\0';

    return result;
}

// Function to decrypt a message using Rail Fence Cipher
char* decryptRailFence(char* text, int key) {
    int len = strlen(text);
    char rail[key][len];

    // Filling the rail matrix with dummy characters
    for (int i = 0; i < key; i++) {
        for (int j = 0; j < len; j++) {
            rail[i][j] = '.';
        }
    }

    // Finding the direction
    bool dir_down = false;
    int row = 0, col = 0;

    // Filling the rail matrix with '*' to mark the positions of the encrypted characters
    for (int i = 0; i < len; i++) {
        if (row == 0 || row == key - 1) {
            dir_down = !dir_down;
        }
        rail[row][col++] = '*';
        dir_down ? row++ : row--;
    }

    // Filling the rail matrix with the encrypted characters
    int index = 0;
    for (int i = 0; i < key; i++) {
        for (int j = 0; j < len; j++) {
            if (rail[i][j] == '*' && index < len) {
                rail[i][j] = text[index++];
            }
        }
    }

    // Construct the decrypted message
    char* result = malloc(len + 1);
    row = 0, col = 0;
    for (int i = 0; i < len; i++) {
        if (row == 0) {
            dir_down = true;
        }
        if (row == key - 1) {
            dir_down = false;
        }
        if (rail[row][col] != '*') {
            result[i] = rail[row][col++];
        }
        dir_down ? row++ : row--;
    }
    result[len] = '\0';

    return result;
}

int main() {
    char text[1000];
    int key;

    // Input message
    printf("Enter a Secret Message:\n");
    fgets(text, sizeof(text), stdin);
    if (text[strlen(text) - 1] == '\n') {
        text[strlen(text) - 1] = '\0'; // Removing newline character
    }

    // Input number of rails
    printf("Enter number of rails:\n");
    scanf("%d", &key);

    // Encrypt the message
    char* encrypted = encryptRailFence(text, key);

    // Output the encrypted message
    printf("Encrypted Message:\n%s\n", encrypted);

    // Decrypt the message
    char* decrypted = decryptRailFence(encrypted, key);

    // Output the decrypted message
    printf("Decrypted Message:\n%s\n", decrypted);

    // Free dynamically allocated memory
    free(encrypted);
    free(decrypted);

    return 0;
}


```

## OUTPUT:
```
Enter a Secret Message:
I AM COMING FROM CHENNAI
Enter number of rails:
3
Encrypted Message:
I IF N MCMN RMCENIAOGOHA
Decrypted Message:
I AM COMING FROM CHENNAI
```

## RESULT:
The program is executed successfully
