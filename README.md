# DS-opp-Project
Esi ki tesi sab ki
Author ---- The Great Ahmed Shaheer 
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// -------------------- dATE CLASS --------------------
class dATE {
public:
    int Day;
    int Month;
    int Year;

    dATE() {}

    dATE(int d, int m, int y) {
        Day = d;
        Month = m;
        Year = y;
    }

    void printDate() {
        cout << Day << "/" << Month << "/" << Year;
    }
};

// -------------------- ITEM CLASS --------------------
class Item {
private:
    string ItemName;
    int ItemPrice;

public:
    Item() {}

    Item(string name, int price) {
        ItemName = name;
        ItemPrice = price;
    }

    string getItemName() { return ItemName; }
    int getItemPrice()   { return ItemPrice; }

    void setItemPrice(int price) { ItemPrice = price; }
};

// -------------------- STOCK CLASS --------------------
class Stock {
private:
    int StockQuantity;
    Item StockItem;

public:
    Stock(int quantity, Item item) {
        StockQuantity = quantity;
        StockItem = item;
    }

    // Reduce stock
    int setStock(int qminus) {
        if (qminus <= StockQuantity) {
            StockQuantity -= qminus;
            return StockQuantity;
        }
        else {
            cout << "Not enough stock available!" << endl;
            return -1;
        }
    }

    string getStkItemName()  { return StockItem.getItemName(); }
    int getStkItemPrice()    { return StockItem.getItemPrice(); }
    int getQuantity()        { return StockQuantity; }
};

// -------------------- ORDER CLASS --------------------
class Order {
private:
    string OrderBy;
    dATE OrderOn;
    vector<string> OrderOf;   // list<string> as shown in UML
    int OrderTotal;

public:
    Order(string name, dATE date) {
        OrderBy = name;
        OrderOn = date;
        OrderTotal = 0;
    }

    void AddItem(Stock &stk, int No) {
        if (stk.getQuantity() < No) {
            cout << "Not enough stock. Cannot add item.\n";
            return;
        }

        stk.setStock(No);  // reduce stock

        // push item name for listing
        for (int i = 0; i < No; i++)
            OrderOf.push_back(stk.getStkItemName());

        // update total
        OrderTotal += No * stk.getStkItemPrice();
    }

    void PrintOrder() {
        cout << "------------------------------\n";
        cout << "Order By: " << OrderBy << endl;
        cout << "Order Date: ";
        OrderOn.printDate();
        cout << "\nItems Ordered:\n";

        for (string x : OrderOf)
            cout << "- " << x << endl;

        cout << "Total Bill = Rs. " << OrderTotal << endl;
        cout << "------------------------------\n";
    }
};

// -------------------- MAIN FUNCTION --------------------
int main() {

    // Create item
    Item shampoo("Shampoo", 100);

    // Add stock
    Stock stk1(50, shampoo);

    // Create date
    dATE today(1, 1, 2025);

    // Create order
    Order o1("Khurram", today);

    // Add items
    o1.AddItem(stk1, 20);

    // Print order
    o1.PrintOrder();

    return 0;
}
