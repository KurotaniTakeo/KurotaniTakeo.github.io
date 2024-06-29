---
title: C++面向对象——一些基础例题及代码
date: 2023-11-22 01:19:44
tags:
	- C++
	- Study
categories: Self-use
excerpt: "一些个人C++作业例题以及自己的解答。"
---

{% notel 注意看…… %} 这是一篇自用文章，可能对你没有什么帮助。<br/>为了节约你的时间，也许你可以跳过这篇文章: ) {% endnotel %}

在此存放一些个人的 C++作业例题以及自己的解答。

过于简单，以至于这篇文章归于“自用”分类下，也许对你并没有太大的帮助（

随缘更新 uwu

## 1. Destructure Function and Overloaded Operator.

### The Question:

Give the definitions for the member function addValue, the copy constructor, the overloaded assignment operator, the overloaded operator<<, and the destructor for the following class. This class is intended to be a class for a partially filled array. The member variable numberUsed contains the number of array positions currently filled. The other constructor definition is given to help you get started.

```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

class PartFilledArray {
public:
  PartFilledArray(int arraySize);
  PartFilledArray(const PartFilledArray& object);
  ~PartFilledArray();
  void operator=(const PartFilledArray& rightSide);
  friend ostream& operator<<(ostream& outs, const PartFilledArray& object);
  void addValue(double newEntry);
private:
  double *a;
  int maxNumber;
  int numberUsed;
};

PartFilledArray::PartFilledArray(int arraySize) : maxNumber(arraySize), numberUsed(0)
{
 a = new double[maxNumber];
}

```

### My Solution:

```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

class PartFilledArray
{
public:
    PartFilledArray(int arraySize);
    PartFilledArray(const PartFilledArray &object);
    ~PartFilledArray();
    void operator=(const PartFilledArray &rightSide);
    friend ostream &operator<<(ostream &outs, const PartFilledArray &object);
    void addValue(double newEntry);

private:
    double *a;
    int maxNumber;
    int numberUsed;
};

PartFilledArray::PartFilledArray(int arraySize) : maxNumber(arraySize), numberUsed(0)
{
    a = new double[maxNumber];
}

// Member function addValue
void PartFilledArray::addValue(double newEntry) {
    if (numberUsed < maxNumber) {
        a[numberUsed] = newEntry;
        numberUsed++;
    } else {
        cout << "Error: Adding value to a full array.\n";
        exit(EXIT_FAILURE);
    }
}

// Copy constructor
PartFilledArray::PartFilledArray(const PartFilledArray& object) : maxNumber(object.maxNumber), numberUsed(object.numberUsed) {
    a = new double[maxNumber];
    for (int i = 0; i < numberUsed; i++) {
        a[i] = object.a[i];
    }
}

// Overloaded assignment operator
void PartFilledArray::operator =(const PartFilledArray& rightSide) {
    if (this != &rightSide) {
        delete [] a;
        maxNumber = rightSide.maxNumber;
        numberUsed = rightSide.numberUsed;
        a = new double[maxNumber];
        for (int i = 0; i < numberUsed; i++) {
            a[i] = rightSide.a[i];
        }
    }
}

// Overloaded operator<<
ostream& operator<<(ostream& outs, const PartFilledArray& object) {
    for (int i = 0; i < object.numberUsed; i++) {
        outs << object.a[i] << " ";
    }
    return outs;
}

// Destructor
PartFilledArray::~PartFilledArray() {
    delete [] a;
}

int main() {
    // Create a PartFilledArray object of size 5
    PartFilledArray pfa1(5);

    // Add values to pfa1
    pfa1.addValue(1.1);
    pfa1.addValue(2.2);
    pfa1.addValue(3.3);

    cout << "pfa1: " << pfa1 << endl;

    // Create a new PartFilledArray object using the copy constructor
    PartFilledArray pfa2(pfa1);

    cout << "pfa2: " << pfa2 << endl;

    // Create a PartFilledArray object of size 10
    PartFilledArray pfa3(10);

    // Add values to pfa3
    for (int i = 0; i < 10; i++) {
        pfa3.addValue(i * 1.1);
    }

    cout << "pfa3: " << pfa3 << endl;

    // Use the assignment operator to assign the value of pfa3 to pfa1
    pfa1 = pfa3;

    cout << "pfa1: " << pfa1 << endl;

    return 0;
}

```

## 2. (Continue to the previous question)

### The Question:

Define a class called PartFilledArrayWMax that is a derived class of the class PartFilledArray. The class PartFilledArrayWMax has one additional member variable named maxValue that holds the maximum value stored in the array. Define a member accessor function named getMax that returns the maximum value stored in the array. Redefine the member function addValue and define two constructors, one of which has an int argument for the maximum number of entries in the array, another is a copy constructor. Also define an overloaded assignment operator, and a destructor.

```cpp
class PartFilledArrayWMax : public PartFilledArray {
public:
    PartFilledArrayWMax(int arraySize);
    PartFilledArrayWMax(const  PartFilledArrayWMax& object);
    ~PartFilledArrayWMax();
    void operator= (const PartFilledArrayWMax& rightSide);
    void addValue(double newEntry);
    double getMax();

private:
    double maxValue;

};
```

### My Solution:

```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

class PartFilledArray
{
public:
    PartFilledArray(int arraySize);
    PartFilledArray(const PartFilledArray &object);
    ~PartFilledArray();
    void operator=(const PartFilledArray &rightSide);
    friend ostream &operator<<(ostream &outs, const PartFilledArray &object);
    void addValue(double newEntry);

private:
    double *a;
    int maxNumber;
    int numberUsed;
};

PartFilledArray::PartFilledArray(int arraySize) : maxNumber(arraySize), numberUsed(0)
{
    a = new double[maxNumber];
}

class PartFilledArrayWMax : public PartFilledArray
{

public:
    PartFilledArrayWMax(int arraySize);
    PartFilledArrayWMax(const PartFilledArrayWMax &object);
    ~PartFilledArrayWMax();
    void operator=(const PartFilledArrayWMax &rightSide);
    void addValue(double newEntry);
    double getMax();

private:
    double maxValue;
};

double PartFilledArrayWMax::getMax()
{
    return maxValue;
}

void PartFilledArrayWMax::addValue(double newEntry)
{
    PartFilledArray::addValue(newEntry);
    maxValue = max(maxValue, newEntry);
}

PartFilledArrayWMax::PartFilledArrayWMax(const PartFilledArrayWMax &object) : PartFilledArray(object), maxValue(object.maxValue)
{
}

PartFilledArrayWMax::PartFilledArrayWMax(int arraySize) : PartFilledArray(arraySize), maxValue(0)
{
}

void PartFilledArrayWMax::operator=(const PartFilledArrayWMax &rightSide)
{
    PartFilledArray::operator=(rightSide);
    maxValue = rightSide.maxValue;
}

PartFilledArray::~PartFilledArray()
{
}

PartFilledArrayWMax::~PartFilledArrayWMax()
{
}

void PartFilledArray::addValue(double newEntry)
{
    if (numberUsed < maxNumber)
    {
        a[numberUsed] = newEntry;
        numberUsed++;
    }
}

PartFilledArray::PartFilledArray(const PartFilledArray &object)
    : maxNumber(object.maxNumber), numberUsed(object.numberUsed)
{
    a = new double[maxNumber];
    std::copy(object.a, object.a + object.numberUsed, a);
}

void PartFilledArray::operator=(const PartFilledArray &rightSide)
{
    if (this != &rightSide)
    {
        delete[] a;

        maxNumber = rightSide.maxNumber;
        numberUsed = rightSide.numberUsed;

        a = new double[maxNumber];
        std::copy(rightSide.a, rightSide.a + rightSide.numberUsed, a);
    }
}

int main()
{
    PartFilledArrayWMax arr(10);

    arr.addValue(5.0);
    cout << "Max Value: " << arr.getMax() << endl;

    arr.addValue(10.0);
    cout << "Max Value: " << arr.getMax() << endl;

    arr.addValue(3.0);
    cout << "Max Value: " << arr.getMax() << endl;

    return 0;
}

```
