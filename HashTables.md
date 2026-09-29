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

void displayHashTable(const vector<Record>& hashTable){
    int tableSize = hashTable.size();

    for(int i = 0; i < tableSize; i++){
        if(hashTable[i].key != -1){ // Checks if the current slot contains a record
            int homePosition = hashFunction(hashTable[i].key, tableSize); // Finds original position of the key

            cout << "Key: " << hashTable[i].key << endl;
            cout << "Value: " << hashTable[i].value << endl;
            cout << "Home position: " << homePosition << endl;
            cout << "Actual position: " << i << endl;
            cout << endl;
        }
    }
}

int main() {
    const int tableSize = 11;

    Record emptyRecord = { -1, "" }; // Initializes every empty slot with key == -1
    vector<Record> hashTable(tableSize, emptyRecord);


    return 0;
}
```
## Analysis:
### 1. A key may not be stored at the home position if another key is stored there, resulting in a collision. 
### 2. Linear probing determines the actual position by checking the next position in the hash table until an empty slot is found. This empty slot becomes the key's position.
### 3. A large distance between the home position and actual position makes operations slower because the function will often need to check many positions before finding the key or an empty slot. 

# Part 6: Searching with Linear Probing
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

void displayHashTable(const vector<Record>& hashTable) {
    int tableSize = hashTable.size();

    for (int i = 0; i < tableSize; i++) {
        if (hashTable[i].key != -1) { // Checks if the current slot contains a record
            int homePosition = hashFunction(hashTable[i].key, tableSize); // Finds original position of the key

            cout << "Key: " << hashTable[i].key << endl;
            cout << "Value: " << hashTable[i].value << endl;
            cout << "Home position: " << homePosition << endl;
            cout << "Actual position: " << i << endl;
            cout << endl;
        }
    }
}

bool searchRecord(const vector<Record>& hashTable, int key, int& positionsExamined) {
    int tableSize = hashTable.size();
    int originalIndex = hashFunction(key, tableSize);
    positionsExamined = 0;

    for (int i = 0; i < tableSize; i++) {
        int index = (originalIndex + i) % tableSize;
        positionsExamined++; // Counts each position checked

        if (hashTable[index].key == key) { // Returns true if key is found
            return true;
        }

        if (hashTable[index].key == -1) { // Stops searching if empty slot is found
            return false;
        }
    }

    return false; // If key isn't found after checking every slot
}

int main() {
    const int tableSize = 11;

    Record emptyRecord = { -1, "" }; // Initializes every empty slot with key == -1
    vector<Record> hashTable(tableSize, emptyRecord);


    return 0;
}
```
 ## Analysis:
 ### A displaced key may require multiple table accesses because collisions caused the key to be stored in a position other than its home position. The search follows the linear probing sequence and check each position until the key is found. This is still considered O(1) because only a small number of positions are checked in the average case, regardless of how large the table is. In this sense, the search scales at a constant rate rather than a linear rate as N becomes infinitely large. 

 # Part 7: Deletion and Tombstones
```C++
#include <iostream>
#include <string>
#include <vector>
using namespace std;

enum SlotState {
    EMPTY,
    OCCUPIED,
    DELETED
};

struct Record {
    int key;
    string value;
    SlotState state;
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
    int deletedIndex = -1;

    for (int i = 0; i < tableSize; i++) {
        int index = (originalIndex + i) % tableSize;


        if (hashTable[index].state == OCCUPIED && hashTable[index].key == key) { // Updates value stored at index if key already exists
            hashTable[index].value = value;
            return true;
        }


        if (hashTable[index].state == DELETED && deletedIndex == -1) { // Saves the first deleted slot found
            deletedIndex = index;
        }


        if (hashTable[index].state == EMPTY) { // Inserts record into empty slot if found
            if (deletedIndex != -1) {
                index = deletedIndex;
            }

            hashTable[index].key = key;
            hashTable[index].value = value;
            hashTable[index].state = OCCUPIED;
            return true;
        }
    }


    if (deletedIndex != -1) { // Inserts record into the first deleted slot found
        hashTable[deletedIndex].key = key;
        hashTable[deletedIndex].value = value;
        hashTable[deletedIndex].state = OCCUPIED;
        return true;
    }


    return false; // If every slot was checked and the table is full
}

void displayHashTable(const vector<Record>& hashTable) {
    int tableSize = hashTable.size();

    for (int i = 0; i < tableSize; i++) {
        if (hashTable[i].state == OCCUPIED) { // Checks if the current slot contains a record
            int homePosition = hashFunction(hashTable[i].key, tableSize); // Finds original position of the key

            cout << "Key: " << hashTable[i].key << endl;
            cout << "Value: " << hashTable[i].value << endl;
            cout << "Home position: " << homePosition << endl;
            cout << "Actual position: " << i << endl;
            cout << endl;
        }
    }
}

bool searchRecord(const vector<Record>& hashTable, int key, int& positionsExamined) {
    int tableSize = hashTable.size();
    int originalIndex = hashFunction(key, tableSize);
    positionsExamined = 0;

    for (int i = 0; i < tableSize; i++) {
        int index = (originalIndex + i) % tableSize;
        positionsExamined++; // Counts each position checked

        if (hashTable[index].state == OCCUPIED && hashTable[index].key == key) { // Returns true if key is found
            return true;
        }

        if (hashTable[index].state == EMPTY) { // Stops searching if an empty slot is found
            return false;
        }
    }

    return false; // If key isn't found after checking every slot
}

bool removeRecord(vector<Record>& hashTable, int key) {
    int tableSize = hashTable.size();
    int originalIndex = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++) {
        int index = (originalIndex + i) % tableSize;

        if (hashTable[index].state == OCCUPIED && hashTable[index].key == key) { // Marks record as deleted if key is found
            hashTable[index].state = DELETED;
            return true;
        }

        if (hashTable[index].state == EMPTY) { // Stops searching if an empty slot is found
            return false;
        }
    }

    return false; // If key isn't found after checking every slot
}

int main() {
    const int tableSize = 11;

    Record emptyRecord = { -1, "", EMPTY };
    vector<Record> hashTable(tableSize, emptyRecord);

    insertRecord(hashTable, 100, "First");
    insertRecord(hashTable, 10, "Second");
    insertRecord(hashTable, 1000, "Third");

    cout << "Table before:" << endl;
    displayHashTable(hashTable);

    removeRecord(hashTable, 100);

    cout << "Table after:" << endl;
    displayHashTable(hashTable);

    int positionsExamined;
    bool found = searchRecord(hashTable, 1000, positionsExamined);

    cout << "Searching for key 1000:" << endl;
    cout << "Found: " << (found ? "Yes" : "No") << endl;
    cout << "Positions examined: " << positionsExamined << endl;

    return 0;
}
```
## Analysis:
### If the deleted position were marked as ```Empty``` it could produce an incorrect result because the search stops when it finds an empty position. For example, if the deleted key was the first collision, the search would stop before it checked the keys stored after it. Using ```Deleted``` signals for the search algorithm to continue beyond the empty position. 

