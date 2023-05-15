#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_PRODUCTS 100
#define MAX_NAME_LENGTH 50

typedef struct {
    char name[MAX_NAME_LENGTH];
    int stock;
    float price;
    int sales;
} Product;

Product products[MAX_PRODUCTS];
int num_products = 0;

int add_product(char name[], int stock, float price) {
    if (num_products == MAX_PRODUCTS) {
        return 0;
    }

    Product p;
    strcpy(p.name, name);
    p.stock = stock;
    p.price = price;
    p.sales = 0;

    products[num_products++] = p;
    return 1;
}

void sell_product(int index, int quantity) {
    if (index < 0 || index >= num_products) {
        printf("Invalid product index.\n");
        return;
    }

    Product *p = &products[index];
    if (p->stock < quantity) {
        printf("Not enough stock.\n");
        return;
    }

    p->stock -= quantity;
    p->sales += quantity;
    printf("%d units of %s sold.\n", quantity, p->name);
}

void display_report() {
    printf("PRODUCT\t\tSTOCK\tPRICE\tSALES\n");

    for (int i = 0; i < num_products; i++) {
        Product p = products[i];
        printf("%s\t\t%d\t%.2f\t%d\n", p.name, p.stock, p.price, p.sales);
    }
}

int main() {
    int option = 0;

    do {
        printf("\n1. Add product\n");
        printf("2. Sell product\n");
        printf("3. Display report\n");
        printf("4. Exit\n");
        printf("Select an option: ");
        scanf("%d", &option);

        switch (option) {
            case 1: {
                char name[MAX_NAME_LENGTH];
                int stock;
                float price;

                printf("\nEnter product name: ");
                scanf("%s", name);
                printf("Enter product stock: ");
                scanf("%d", &stock);
                printf("Enter product price: ");
                scanf("%f", &price);

                if (add_product(name, stock, price)) {
                    printf("Product added.\n");
                } else {
                    printf("Could not add product. Maximum number of products reached.\n");
                }

                break;
            }
            case 2: {
                int index;
                int quantity;

                printf("\nEnter product index: ");
                scanf("%d", &index);
                printf("Enter quantity to sell: ");
                scanf("%d", &quantity);

                sell_product(index, quantity);
                break;
            }
            case 3: {
                display_report();
                break;
            }
            case 4: {
                printf("Goodbye!\n");
                break;
            }
            default: {
                printf("Invalid option. Try again.\n");
                break;
            }
        }
    } while (option != 4);

    return 0;
}
