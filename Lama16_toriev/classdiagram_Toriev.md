# Диаграмма классов: Интернет-магазин

## Описание предметной области
Система представляет собой веб-приложение для розничной продажи товаров.  
Покупатели могут регистрироваться, просматривать каталог, добавлять товары в корзину и оформлять заказы.  
Администраторы управляют товарами и просматривают заказы.  
Оплата происходит через внешний платёжный сервис.

### Основные классы и их назначение
- **User** – базовый класс для всех пользователей (общие данные учётной записи).
- **Customer** – покупатель, может управлять корзиной и оформлять заказы.
- **Admin** – администратор, управляет каталогом товаров.
- **Product** – товар в магазине (название, цена, остаток).
- **ShoppingCart** – корзина покупателя, временно хранит выбранные товары.
- **Order** – оформленный заказ клиента.
- **OrderItem** – позиция в заказе (товар и количество). Существует только внутри заказа.
- **PaymentService** – внешний сервис для проведения платежей (зависимость).

## Диаграмма

```mermaid
classDiagram
    class User {
        -int id
        -String name
        -String email
        -String passwordHash
        +login(email: String, password: String): boolean
        +logout(): void
    }

    class Customer {
        -String address
        +addToCart(product: Product, quantity: int): void
        +checkout(): Order
    }

    class Admin {
        -String role
        +addProduct(product: Product): void
        +removeProduct(productId: int): void
    }

    class Product {
        -int id
        -String name
        -double price
        -int stock
        +updateStock(quantity: int): void
        +getPrice(): double
    }

    class ShoppingCart {
        +addItem(product: Product, quantity: int): void
        +removeItem(product: Product): void
        +getCartTotal(): double
    }

    class Order {
        -int id
        -Date date
        -String status
        -double total
        +calculateTotal(): double
        +addOrderItem(item: OrderItem): void
    }

    class OrderItem {
        -int quantity
        -double price
        +getSubtotal(): double
    }

    class PaymentService {
        +processPayment(amount: double, cardDetails: String): boolean
    }

    Customer --|> User
    Admin --|> User
    Customer "1" -- "1" ShoppingCart : owns
    Customer "1" -- "0..*" Order : places
    ShoppingCart "1" o-- "0..*" Product : contains
    Order "1" *-- "0..*" OrderItem : consists of
    Customer ..> PaymentService : uses
