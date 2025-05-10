# Car Dealership CLI Application

## Overview

This is a simple CLI java application for managing cars for dealers, each entry records their values of VIN, year, make, model, type, color, mileage and price.
All enties are read from a saved to a CSV file when the dealer runs and exits the program. The built-in CSV file has existing entries for testing purpose.
The application supports several features, users can modify the car list, or search/filter by specific property values. 

## Features

- **Search/Filter**: View and filter vehicles in the following ways:
  - Price
  - Make/model
  - Year
  - Color
  - Mileage
  - Type
  - Display all
- **Modify**: change the vehicle list in the following ways:
  - Add
  - Remove
- **Auto save**: When exiting the program.

## Requirements

- Java 8 or higher

## Setup

1. Clone this repository to your local machine.

    ```bash
    git clone https://github.com/dmDaisy/workshopFour_carDealership
    cd workshopFour_carDealership
    ```

2. Compile the Java files.

    ```bash
    javac -d out src/main/java/com/ps/*.java
    ```

3. Run the application.

    ```bash
    java -cp out com.ps.Main
    ```

## Screenshots

Below are a few screenshots of the application in action:
![Screenshot 2025-05-09 at 9 44 42 PM](https://github.com/user-attachments/assets/63bc2b5b-7acb-4c16-8eb1-1c891ec64f39)

### Home Screen

![Screenshot 2025-05-09 at 9 44 42 PM](https://github.com/user-attachments/assets/63bc2b5b-7acb-4c16-8eb1-1c891ec64f39)

### Search by features example 1

![Screenshot 2025-05-09 at 9 45 21 PM](https://github.com/user-attachments/assets/8679b20c-9fbe-457d-8cbf-49162aff6dea)

### Search by features example 2

![Screenshot 2025-05-09 at 9 46 12 PM](https://github.com/user-attachments/assets/162bb91a-eed3-4425-91d7-5cb7b5ad0454)

### Search by features example 3

![Screenshot 2025-05-09 at 9 46 40 PM](https://github.com/user-attachments/assets/060ed825-f33d-4daa-9609-37e42db69a3e)


### Sample Custom Search

![Screenshot 2025-05-01 at 3 38 17 PM](https://github.com/user-attachments/assets/8fb71c64-a8bc-4e7c-89df-a976c33dd054)


### Interesting Piece of Code
**Combining code to avoid repetition**: For example, the `addDeposit()` and `addPayment()` methods are combined into one method `addTransaction(boolean valueIsPositive)` 
and a helper method `saveTransaction(Transaction t, String type, String fileName)` that does all the following repetitive steps that are needed for both methods:
  - Prompt user for transaction details
  - Determine if amount is positive/negative based on transaction type
  - Save transaction to ledger
  - Save transaction to CSV file
  - Print confirmation message

```java
// add deposit or payment
private static void addTransaction(boolean valueIsPositive){
    System.out.println("Enter description: ");
    String description = scanner.nextLine();
    System.out.println("Enter vendor: ");
    String vendor = scanner.nextLine();
    // foolproof to ensure amount >= 0 for deposit and amount < 0 for payment
    float amount = getUserFloat();
    if((valueIsPositive && amount < 0) || ( ! valueIsPositive && amount > 0)) amount *= -1;
    LocalDateTime dateTime = LocalDateTime.now();
    saveTransaction(new Transaction(dateTime, description, vendor, amount), (valueIsPositive ? "deposit" : "payment"), FILE_NAME);
}

// Helper method for saving the transaction to both the ledger and the CSV file
private static void saveTransaction(Transaction t, String transactionType, String fileName) {
    ledger.add(t);
    try (BufferedWriter writer = new BufferedWriter(new FileWriter(fileName, true))) {
        writer.write(t.toCsvEntry());
        writer.newLine(); // better than adding "\n"
        System.out.println("The following " + transactionType + " is successfully saved to your ledger file!");
        System.out.println(t);
    } catch(IOException e){
        System.out.println("Error writing to file. ");
    }
}
