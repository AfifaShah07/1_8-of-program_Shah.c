#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX 100

char stack[MAX];
int top = -1;

// Push
void push(char x) {
    stack[++top] = x;
}

// Pop
char pop() {
    if (top == -1)
        return -1;
    return stack[top--];
}

// Priority of operators
int priority(char x) {
    if (x == '(')
        return 0;
    if (x == '+' || x == '-')
        return 1;
    if (x == '*' || x == '/')
        return 2;
    return 0;
}

// Reverse a string
void reverse(char exp[]) {
    int n = strlen(exp);
    for (int i = 0; i < n/2; i++) {
        char temp = exp[i];
        exp[i] = exp[n - i - 1];
        exp[n - i - 1] = temp;
    }
}

// Infix to Postfix converter (used inside prefix logic)
void infixToPostfix(char infix[], char postfix[]) {
    char *e = infix;
    char x;
    int k = 0;

    while (*e != '\0') {

        if (isalnum(*e)) {
            postfix[k++] = *e;
        }
        else if (*e == '(') {
            push(*e);
        }
        else if (*e == ')') {
            while ((x = pop()) != '(')
                postfix[k++] = x;
        }
        else {  // Operator
            while (priority(stack[top]) >= priority(*e)) {
                postfix[k++] = pop();
            }
            push(*e);
        }
        e++;
    }

    while (top != -1) {
        postfix[k++] = pop();
    }

    postfix[k] = '\0';
}

int main() {
    char infix[MAX], postfix[MAX], prefix[MAX];

    printf("Enter infix expression: ");
    scanf("%s", infix);

    // Step 1: Reverse infix
    reverse(infix);

    // Step 2: Replace brackets
    for (int i = 0; i < strlen(infix); i++) {
        if (infix[i] == '(')
            infix[i] = ')';
        else if (infix[i] == ')')
            infix[i] = '(';
    }

    // Step 3: Convert to postfix
    infixToPostfix(infix, postfix);

    // Step 4: Reverse postfix → prefix
    reverse(postfix);

    printf("Prefix expression: %s\n", postfix);

    return 0;
}# 1_8-of-program_Shah.c
Infix to prefix
