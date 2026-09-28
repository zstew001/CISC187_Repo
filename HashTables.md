# Part 1: Understanding Hash Functions
## Keys:
```C++
 555223 - 5 + 5 + 5 + 2 + 2 + 3 = 22 // Index = 22%10 = 2
 555980 - 5 + 5 + 5 + 9 + 8 + 0 = 32 // Index = 32%10 = 2
 555000 - 5 + 5 + 5 + 0 + 0 + 0 = 15 // Index = 15%10 = 5
 555890 - 5 + 5 + 5 + 8 + 9 + 0 = 32 // Index = 32%10 = 2
```
## Analysis:
### Yes, index 555223, 555980 and 555890 all produce index 2. This is called a *Hash Collision*. Applying %10 to the key value always results in a valid table index because the possible values %10 provides range from 0-9. The table specified contains 10 slots which would have index 0-9. Although increasing the table size reduces the likelihood of collisions, it doesn't guarantee they won't occur. For example, if the table size was 100 and we used ``` index = digit sum % 100 ``` 555980 and 555890 would still result in a matching table index: 32.

# Part 2: Implement a Hash Function
```C++
#include <iostream>
using namespace std;

int hashFunction(int key, int tableSize) {
    int digitSum = 0;

    while (key > 0) {
        digitSum += key % 10;
        key /= 10;
    }

    return digitSum % tableSize;
}

int main() {
    int tableSize = 10;

    int myKeys[] = { 555223, 555980, 555000, 555890 };
    int numOfKeys = 4;

    for (int i = 0; i < numOfKeys; i++) {
        cout << "Key: " << myKeys[i] << endl;
        cout << "Index: " << hashFunction(myKeys[i], tableSize) << endl;
    }

    return 0;
}
```

# Part 3: Build a Hash Table
```C++
#include <iostream>
#include <string>
#include <vector>
using namespace std;

struct Record{
    int key;
    string value;
};

int hashFunction(int key, int tableSize){
    int digitSum = 0;

    while(key > 0){
        digitSum += key % 10;
        key /= 10;
    }

    return digitSum % tableSize;
}

int main(){
    const int tableSize = 11;

    Record emptyRecord = {-1, ""}; // Initializes every empty slot with key == -1
    vector<Record> hashTable(tableSize, emptyRecord);

    return 0;
}
```

# Part 4: Linear Probing
```C++
#include <iostream>
#include <string>
#include <vector>
using namespace std;

struct Record {
    int key;
    string value;
};

int hashFunction(int key, int tableSize) {
    int digitSum = 0;

    while (key > 0) {
        digitSum += key % 10;
        key /= 10;
    }

    return digitSum % tableSize;
}

bool insertRecord(vector<Record>& hashTable, int key, string value) {
    int tableSize = hashTable.size();
    int originalIndex = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++) {
        int index = (originalIndex + i) % tableSize;

        
        if (hashTable[index].key == key) { // Updates value stored at index if key already exists
            hashTable[index].value = value;
            return true;
        }

        
        if (hashTable[index].key == -1) { // Inserts index into an empty slot if found
            hashTable[index].key = key;
            hashTable[index].value = value;
            return true;
        }
    }

    
    return false; // If every slot was checked and the table is full
}

int main() {
    const int tableSize = 11;

    Record emptyRecord = { -1, "" }; // Initializes every empty slot with key == -1
    vector<Record> hashTable(tableSize, emptyRecord);


    return 0;
}
```
## Explanation: 
### My implementation avoids an infinite loop by limiting the for loop to the size of the hash table. The table only has 11 slots, so the loop can only check up to 11 positions. If an empty slot is found, the index value is inserted and the function returns true. If the key exists already, the value is updated and the function also returns true. If all 11 slots are checked and the table is full, the function returns false, ending the loop. 

# Part 5: Home Position and Actual Position
