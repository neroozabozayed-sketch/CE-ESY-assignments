#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

typedef struct {
    char *buffer;
    int head;
    int tail;
    int size;
} CircularBuffer;

void initBuffer(CircularBuffer *cb, int size) {
    cb->size = size;
    cb->buffer = (char *)malloc(size * sizeof(char));
    cb->head = 0;
    cb->tail = 0;
}
bool isFull(CircularBuffer *cb) {
    return ((cb->head + 1) % cb->size) == cb->tail;
}
bool isEmpty(CircularBuffer *cb) {
    return cb->head == cb->tail;
}

bool writeBuffer(CircularBuffer *cb, char data) {
    if (isFull(cb)) {
        return false;
    }
    cb->buffer[cb->head] = data;
    cb->head = (cb->head + 1) % cb->size;
    return true;
}

bool readBuffer(CircularBuffer *cb, char *data) {
    if (isEmpty(cb)) {
        return false; 
    }
    *data = cb->buffer[cb->tail];
    cb->tail = (cb->tail + 1) % cb->size;
    return true;
}

void runTest(int bufferSize, char *inputData) {
    CircularBuffer cb;
    initBuffer(&cb, bufferSize);
    printf("\n--- اختبار بمجسم حجمه: %d ---\n", bufferSize);
    printf("البيانات المراد إدخالها: %s\n", inputData);
    int i = 0;
    while (inputData[i] != '\0') {
        if (!writeBuffer(&cb, inputData[i])) {
            printf("[Overflow] المخزن امتلأ لم يتم تخزين: '%c'\n", inputData[i]);
            break;
        }
        i++;
    }

  
    printf("البيانات المستخرجة من المخزن: ");
    char temp;
    while (readBuffer(&cb, &temp)) {
        printf("%c", temp);
    }
    printf("\n");
    if (isEmpty(&cb)) {
        printf("الحالة النهائية: المخزن فارغ تماماً.\n");
    }

    free(cb.buffer);
}

int main() {
    char name[50];
    char suffix[] = "CE_ESY";
    char fullData[100];
    printf("nairouz CE-ESY ");
    scanf("%s", name);

    strcpy(fullData, name);
    strcat(fullData, suffix);
    
    int dataLen = strlen(fullData);
    runTest(dataLen + 5, fullData);
    runTest(dataLen / 2, fullData);

    return 0;
}
مخزن حلقي 
