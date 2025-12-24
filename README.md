#include <stdio.h>
#include <string.h>

struct Product {
    char name[50];
    float originalPrice;
    float discountedPrice;
    float discountPercent;
};

// Function to swap two products
void swap(struct Product *a, struct Product *b) {
    struct Product temp = *a;
    *a = *b;
    *b = temp;
}

// Partition function for Quick Sort
int partition(struct Product arr[], int low, int high) {
    float pivot = arr[high].discountPercent;
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j].discountPercent > pivot) { // descending order
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}

// Quick Sort function
void quickSort(struct Product arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    int n;
    printf("Enter number of products: ");
    scanf("%d", &n);

    struct Product products[n];

    for (int i = 0; i < n; i++) {
        printf("\nEnter product name: ");
        scanf("%s", products[i].name);
        printf("Enter original price: ");
        scanf("%f", &products[i].originalPrice);
        printf("Enter discounted price: ");
        scanf("%f", &products[i].discountedPrice);

        products[i].discountPercent =
            ((products[i].originalPrice - products[i].discountedPrice) / products[i].originalPrice) * 100;
    }

    quickSort(products, 0, n - 1);

    printf("\nProducts sorted by highest discount:\n");
    printf("%-15s %-15s %-18s %-15s\n", "Name", "Original Price", "Discounted Price", "Discount (%)");
    printf("---------------------------------------------------------------\n");

    for (int i = 0; i < n; i++) {
        printf("%-15s %-15.2f %-18.2f %-15.2f\n",
               products[i].name,
               products[i].originalPrice,
               products[i].discountedPrice,
               products[i].discountPercent);
    }

    return 0;
}
