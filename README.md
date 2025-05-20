#include <iostream>
#include <vector>
#include <string>
#include <locale.h>

using namespace std;

class Product {
public:
    string name;
    string description;
    double price;
    int stock;

    Product(string n, string d, double p, int s) {
        name = n;
        description = d;
        price = p;
        stock = s;
    }

    void displayProduct(bool showDescription = false) {
        cout << "Название: " << name << endl;
        cout << "Цена: $" << price << endl;
        if (showDescription) {
            cout << "Описание: " << description << endl;
        }
        cout << "Остаток: " << stock << endl;
    }
};

vector<Product> products;
vector<pair<string, int>> orders;
int totalOrders = 0;
string lang;

string welcomeRU = "Добро пожаловать в супермаркет StrawberryUP!";
string welcomeEN = "Welcome to the StrawberryUP supermarket!";
string loginRU = "Введите логин: ";
string loginEN = "Enter login: ";
string passwordRU = "Введите пароль: ";
string passwordEN = "Enter password: ";
string wrongRU = "Неверный логин или пароль.";
string wrongEN = "Incorrect login or password.";
string successRU = "Успешный вход!";
string successEN = "Login successful!";
string menuRU = "\n1. Просмотреть товары\n2. Заказать товар\n3. Посмотреть описание товара\n4. Проверить остатки\n5. Статистика покупок\n6. Выход\nВыберите пункт: ";
string menuEN = "\n1. View Products\n2. Order Product\n3. View Product Description\n4. Check Stock\n5. Purchase Statistics\n6. Exit\nChoose an option: ";

void chooseLanguage() {
    int choice;
    cout << "Select language / Выберите язык:\n1. Русский\n2. English\nChoice: ";
    cin >> choice;
    if (choice == 1)
        lang = "RU";
    else
        lang = "EN";
}

bool login() {
    string userLogin, userPassword;
    const string correctLogin = "cpp";
    const string correctPassword = "1212";
    int attempts = 0;

    while (attempts < 3) {
        cout << (lang == "RU" ? loginRU : loginEN);
        cin >> userLogin;
        cout << (lang == "RU" ? passwordRU : passwordEN);
        cin >> userPassword;

        if (userLogin == correctLogin && userPassword == correctPassword) {
            cout << (lang == "RU" ? successRU : successEN) << endl;
            return true;
        } else {
            cout << (lang == "RU" ? wrongRU : wrongEN) << endl;
            attempts++;
        }
    }
    return false;
}

void showMainMenu() {
    cout << (lang == "RU" ? menuRU : menuEN);
}

void listProducts() {
    cout << (lang == "RU" ? "\nСписок товаров:\n" : "\nProduct list:\n");
    for (size_t i = 0; i < products.size(); ++i) {
        cout << i + 1 << ". " << products[i].name << " - $" << products[i].price << endl;
    }
}

void orderProduct() {
    listProducts();
    int choice, quantity;
    cout << (lang == "RU" ? "\nВведите номер товара для заказа: " : "\nEnter product number to order: ");
    cin >> choice;
    if (choice < 1 || choice > products.size()) {
        cout << (lang == "RU" ? "Неверный выбор товара.\n" : "Invalid product choice.\n");
        return;
    }

    cout << (lang == "RU" ? "Введите количество: " : "Enter quantity: ");
    cin >> quantity;

    if (quantity > products[choice - 1].stock) {
        cout << (lang == "RU" ? "Недостаточно товара на складе.\n" : "Not enough stock.\n");
    } else {
        products[choice - 1].stock -= quantity;
        orders.push_back({products[choice - 1].name, quantity});
        totalOrders++;
        cout << (lang == "RU" ? "Товар добавлен в заказ!\n" : "Product added to order!\n");
    }
}

void viewProductDescription() {
    listProducts();
    int choice;
    cout << (lang == "RU" ? "\nВведите номер товара для описания: " : "\nEnter product number for description: ");
    cin >> choice;
    if (choice < 1 || choice > products.size()) {
        cout << (lang == "RU" ? "Неверный выбор.\n" : "Invalid choice.\n");
        return;
    }
    cout << endl;
    products[choice - 1].displayProduct(true);
}

void checkStock() {
    cout << (lang == "RU" ? "\nОстатки товаров на складе:\n" : "\nStock availability:\n");
    for (const auto& product : products) {
        cout << product.name << ": " << product.stock << endl;
    }
}

void viewStatistics() {
    cout << (lang == "RU" ? "\nСтатистика покупок:\n" : "\nPurchase statistics:\n");
    cout << (lang == "RU" ? "Общее количество заказов: " : "Total orders: ") << totalOrders << endl;
    for (const auto& order : orders) {
        cout << order.first << " x" << order.second << endl;
    }
}

int main() {
    setlocale(LC_ALL, "Russian");
    products.push_back(Product("Яблоки", "Свежие красные яблоки", 0.50, 50));
    products.push_back(Product("Лимонад", "Освежающий лимонад", 3.2, 30));
    products.push_back(Product("Кола", "Газированный напиток 0.5л", 0.99, 100));
    products.push_back(Product("Банана", "Вкусный фрукт", 1.3, 43));
    products.push_back(Product("Жвачка", "Отличная жвачка relax", 0.12, 41));
    products.push_back(Product("Feastabels", "Шоколадка мистера биста", 3.2, 23));
    products.push_back(Product("Груша", "Сладкая груша", 0.70, 78));
    products.push_back(Product("Апельсин", "Освежит в любое время", 0.85, 126));
    products.push_back(Product("Мандарин", "Поможет в ситуациях", 0.45, 53));
    products.push_back(Product("Клубника", "Самый лучший фрукт!", 0.60, 310));
    products.push_back(Product("Виноград", "Наедитесь с радостью!", 0.55, 102));
    products.push_back(Product("Ананас", "Кислющяя еда!", 1.05, 71));
    products.push_back(Product("Киви", "Кислая еда", 0.70, 65));
    products.push_back(Product("Помидоры", "Сотечается с другими питаниями", 0.40, 35));
    products.push_back(Product("Манго", "Обычное сладкое манго", 0.30, 142));
    products.push_back(Product("Огурцы", "Добавка на плов и на счастье", 0.60, 87));
    products.push_back(Product("Pepsi", "Газированный напиток 1л", 1.5, 86));
    products.push_back(Product("Sprite", "Газированный напиток 1л", 1.4, 109));
    products.push_back(Product("Вода минеральная", "Освежающая вода 1л", 0.85, 62));
    products.push_back(Product("Липтон Зеленый", "Сладкий напиток 1л", 1.99, 52));
    products.push_back(Product("Морковь", "Улучшает ваше зрение", 0.50, 67));
    products.push_back(Product("Картофель", "Можно приготовить чипсы!", 0.80, 89));
    products.push_back(Product("Болгарский перец", "можно приготовить голубцы", 0.90, 56));
    products.push_back(Product("Лук репчатый", "Лук отличный", 0.40, 35));
    products.push_back(Product("Кофе растворимый", "Можно сделать отличное кофе", 0.27, 51));
    products.push_back(Product("Энергетик", "Добавка на плов и на счастье", 0.60, 76));
    products.push_back(Product("Сливки", "Вкусные сливки которые подходят для сладостей", 1.7, 33));
    products.push_back(Product("Чеснок", "Чеснок для плова", 0.4, 109));
    products.push_back(Product("Тыква", "Отличная тыква", 0.85, 35));
    products.push_back(Product("Хлеб", "Сладкий напиток 1л", 0.4, 84));



    chooseLanguage();
    cout << (lang == "RU" ? welcomeRU : welcomeEN) << endl;

    if (!login()) {
        cout << (lang == "RU" ? "Слишком много попыток. Выход.\n" : "Too many attempts. Exiting.\n");
        return 0;
    }

    int choice;
    do {
        showMainMenu();
        cin >> choice;
        switch (choice) {
            case 1:
                listProducts();
                break;
            case 2:
                orderProduct();
                break;
            case 3:
                viewProductDescription();
                break;
            case 4:
                checkStock();
                break;
            case 5:
                viewStatistics();
                break;
            case 6:
                cout << (lang == "RU" ? "Выход из программы..." : "Exiting the program...") << endl;
                break;
            default:
                cout << (lang == "RU" ? "Неверный выбор.\n" : "Invalid choice.\n");
        }
    } while (choice != 6);

    return 0;
}
