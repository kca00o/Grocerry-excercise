## 

import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
public class Product {
    private int productId;
    private String name;
    private double price;

    public Product(int prodid, String name, double price) {
        this.productId = prodid;
        this.name = name;
        this.price = price;
    }

    public int getProductId() {
        return productId;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }
}


public class Display {
    public static void showProduct(Product prod) {
        if (prod != null) {
            System.out.println("Product Name: " + prod.getName());
            System.out.println("Product Price: $" + String.valueOf(prod.getPrice()));
            // converting float to string.
        } else {
            System.out.println("Product not found.");
        }
    }
}


public class Keyboard {
    public int getProductID() {
    Scanner scanner = new Scanner(System.in);
    System.out.print("Enter the product ID: ");
    while (true) {
        try {
            int productId = Integer.parseInt(scanner.nextLine());
            return productId;
        } catch (NumberFormatException e) {
            System.out.println("Invalid input. Please enter a valid product ID (a number).");
        }
    }
}
}

public class CashRegister {
    private List<Product> products = new ArrayList<>();

    public CashRegister(String productsFile) {

        loadProducts(productsFile);
    }

    // accessing the product file to scan for product ID and retrieve data.
    private void loadProducts(String file) {
        try (Scanner fileScanner = new Scanner(new File(file))) {
            while (fileScanner.hasNextLine()) {
                String line = fileScanner.nextLine();
                String[] parts = line.split(",");
                if (parts.length == 3) {
                    int productId = Integer.parseInt(parts[0]);
                    String name = parts[1];
                    double price = Double.parseDouble(parts[2]);
                    products.add(new Product(productId, name, price)); // Store founded product data in array
                }
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
            //Error handler
        }
    }

    public Product findProductById(int prodId) {
        for (Product prod : products) {
            if (prod.getProductId() == prodId) {
                return prod;
            }
        }
        return null;
    }
}

public class Main {
    public static void main(String[] args) {
        //replace productsFile with your current directory for products.txt
        String productsFile = "C:\\Users\\kevin\\Ex3\\src\\exercise3\\products.txt";// Create a .txt file with product information
        CashRegister cashRegister = new CashRegister(productsFile);
        Keyboard keyboard = new Keyboard();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            int productId = keyboard.getProductID();
            Product product = cashRegister.findProductById(productId);
            Display.showProduct(product);

            System.out.print("Do you wish to continue (yes/no)? ");
            String userInput = scanner.nextLine().trim().toLowerCase();

            if (userInput.equals("no")) {
                System.out.println("Exiting the program.");
                break;
            }
        }
    }
}
