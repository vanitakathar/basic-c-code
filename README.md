#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int bid;           // Book ID
    char bname[20];   // Book Name
    int price;        // Book Price
    char author[25];  // Author Name
} Book;

void displayBooks(Book* books, int count) {
    if (count == 0) {
        printf("No books available to display.\n");
        return;
    }
    
    printf("\n+-----------+----------------------+----------+-------------------------+\n");
    printf("| Book ID   | Book Name           | Price    | Author                  |\n");
    printf("+-----------+----------------------+----------+-------------------------+\n");
    
    for (int i = 0; i < count; i++) {
        printf("| %-9d  | %-20s | %-8d  | %-23s |\n", books[i].bid, books[i].bname, books[i].price, books[i].author);
    }
    
    printf("+-----------+----------------------+----------+-------------------------+\n");
}

void addBook(Book** books, int* count, int numToAdd) {
    *books = realloc(*books, (*count + numToAdd) * sizeof(Book));
    if (*books == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }

    for (int i = *count; i < *count + numToAdd; i++) {
        printf("Enter a book ID: ");
        scanf("%d", &(*books)[i].bid);
        printf("Enter a book name: ");
        scanf("%s", (*books)[i].bname);
        printf("Enter a book price: ");
        scanf("%d", &(*books)[i].price);
        printf("Enter a book author name: ");
        scanf("%s", (*books)[i].author);
    }
    *count += numToAdd; // Update the total number of books
}

void searchBookById(Book* books, int count) {
    int searchId;
    printf("Enter the Book ID to search: ");
    scanf("%d", &searchId);
    
    for (int i = 0; i < count; i++) {
        if (books[i].bid == searchId) {
            printf("\nBook found:\nID: %d\nName: %s\nPrice: %d\nAuthor: %s\n",
                   books[i].bid, books[i].bname, books[i].price, books[i].author);
            return;
        }
    }
    printf("Book with ID %d not found.\n", searchId);
}

void searchBookByName(Book* books, int count) {
    char searchName[20];
    printf("Enter book name: ");
    scanf("%s", searchName);

    for (int i = 0; i < count; i++) {
        if (strcmp(books[i].bname, searchName) == 0) {
            printf("\nBook found:\nID: %d\nName: %s\nPrice: %d\nAuthor: %s\n",
                   books[i].bid, books[i].bname, books[i].price, books[i].author);
            return;
        }
    }
    printf("Book with name %s not found.\n", searchName);
}

void searchBookByAuthor(Book* books, int count) {
    char authorName[25];
    printf("Enter author name: ");
    scanf("%s", authorName);

    for (int i = 0; i < count; i++) {
        if (strcmp(books[i].author, authorName) == 0) {
            printf("\nBook found:\nID: %d\nName: %s\nPrice: %d\nAuthor: %s\n",
                   books[i].bid, books[i].bname, books[i].price, books[i].author);
            return;
        }
    }
    printf("Book with author name %s not found.\n", authorName);
}

void removeBook(Book* books, int* count) {
    int removeId;
    printf("Enter the Book ID to remove: ");
    scanf("%d", &removeId);

    for (int i = 0; i < *count; i++) {
        if (books[i].bid == removeId) {
            memmove(&books[i], &books[i + 1], (*count - i - 1) * sizeof(Book));
            (*count)--; 
            printf("Book with ID %d has been removed.\n", removeId);
            return;
        }
    }
    printf("Book with ID %d not found.\n", removeId);
}

void updateBook(Book* books, int count) {
    int updateId;
    printf("Enter the Book ID to update: ");
    scanf("%d", &updateId);

    for (int i = 0; i < count; i++) {
        if (books[i].bid == updateId) {
            printf("Updating book with ID %d\n", books[i].bid);
            printf("Enter new book name: ");
            scanf("%s", books[i].bname);
            printf("Enter new book price: ");
            scanf("%d", &books[i].price);
            printf("Enter new author name: ");
            scanf("%s", books[i].author);
            printf("Book updated successfully.\n");
            return;
        }
    }
    printf("Book with ID %d not found.\n", updateId);
}

void main() {
    int choice, totalBooks = 0;
    Book* books = NULL;

    do {
        printf("\nMenu:\n");
        printf("1. Add Book\n2. Display Books\n3. Search for a Book\n4. Remove a Book\n5. Update a Book\n0. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: {
                int numToAdd;
                printf("How many books do you want to add? ");
                scanf("%d", &numToAdd);
                addBook(&books, &totalBooks, numToAdd);
                break;
            }
            case 2:
                displayBooks(books, totalBooks);
                break;
            case 3: {
                int searchChoice;
                printf("Search by:\n1. ID\n2. Name\n3. Author\nEnter your choice: ");
                scanf("%d", &searchChoice);
                if (searchChoice == 1) {
                    searchBookById(books, totalBooks);
                } else if (searchChoice == 2) {
                    searchBookByName(books, totalBooks);
                } else if (searchChoice == 3) {
                    searchBookByAuthor(books, totalBooks);
                } else {
                    printf("Invalid option.\n");
                }
                break;
            }
            case 4:
                removeBook(books, &totalBooks);
                break;
            case 5:
                updateBook(books, totalBooks);
                break;
            case 0:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid option. Please try again.\n");
                break;
        }
    } while (choice != 0);

    free(books);
   
}
