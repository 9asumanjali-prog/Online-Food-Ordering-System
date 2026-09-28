# Online-Food-Ordering-System
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Scanner;

public class OnlineFoodOrderingSystem {

    static Scanner sc = new Scanner(System.in);

    // Food class
    static class Food {
        int id;
        String name;
        double price;

        Food(int id, String name, double price) {
            this.id = id;
            this.name = name;
            this.price = price;
        }
    }

    // Customer class
    static class Customer {
        String name;
        String phone;
        String address;

        Customer(String name, String phone, String address) {
            this.name = name;
            this.phone = phone;
            this.address = address;
        }
    }

    static ArrayList<Food> menu = new ArrayList<>();
    static ArrayList<Food> cart = new ArrayList<>();

    static Customer customer;
    static int orderId = 1001;

    public static void main(String[] args) {

        loadMenu();

        System.out.println("======================================");
        System.out.println("     ONLINE FOOD ORDERING SYSTEM");
        System.out.println("======================================");

        // 1. Customer Registration
        registerCustomer();

        // 2. View Food Menu
        displayMenu();

        // 3. Search Food
        searchFood();

        // 4. Add Food to Cart
        addFoodToCart();

        // 5. View Cart
        viewCart();

        // 6. Add More Items
        while (true) {

            System.out.print("\nDo you want to add more items? (yes/no): ");
            String choice = sc.nextLine();

            if (choice.equalsIgnoreCase("yes")) {

                displayMenu();
                addFoodToCart();
                viewCart();

            } else if (choice.equalsIgnoreCase("no")) {

                break;

            } else {

                System.out.println("Please enter yes or no.");
            }
        }

        // 7. Select Payment
        String payment = selectPayment();

        // 8. Make Payment
        makePayment(payment);

        // 9. Order Confirmation
        confirmOrder(payment);

        // 10. Save Order
        saveOrder(payment);

        System.out.println("\n======================================");
        System.out.println("          ORDER COMPLETED");
        System.out.println("======================================");

        sc.close();
    }

    // Load food menu
    static void loadMenu() {

        menu.add(new Food(1, "Chicken Biryani", 220));
        menu.add(new Food(2, "Veg Biryani", 180));
        menu.add(new Food(3, "Chicken Burger", 160));
        menu.add(new Food(4, "Veg Burger", 120));
        menu.add(new Food(5, "Pizza", 250));
        menu.add(new Food(6, "French Fries", 90));
        menu.add(new Food(7, "Ice Cream", 80));
        menu.add(new Food(8, "Cold Coffee", 100));
    }

    // Customer Registration
    static void registerCustomer() {

        System.out.println("\n========== CUSTOMER REGISTRATION ==========");

        System.out.print("Enter Name: ");
        String name = sc.nextLine();

        System.out.print("Enter Phone: ");
        String phone = sc.nextLine();

        System.out.print("Enter Address: ");
        String address = sc.nextLine();

        customer = new Customer(name, phone, address);

        System.out.println("\nRegistration Successful!");
    }

    // Display Menu
    static void displayMenu() {

        System.out.println("\n============== FOOD MENU ==============");

        for (Food food : menu) {

            System.out.println(
                    food.id + ". " +
                    food.name + " - ₹" +
                    food.price
            );
        }
    }

    // Search Food
    static void searchFood() {

        System.out.println("\n========== SEARCH FOOD ==========");

        System.out.print(
                "Enter food name to search (press Enter to skip): "
        );

        String search = sc.nextLine();

        if (search.isEmpty()) {
            return;
        }

        boolean found = false;

        for (Food food : menu) {

            if (food.name.toLowerCase()
                    .contains(search.toLowerCase())) {

                System.out.println(
                        food.id + ". " +
                        food.name + " - ₹" +
                        food.price
                );

                found = true;
            }
        }

        if (!found) {
            System.out.println("Food not found.");
        }
    }

    // Add Food to Cart
    static void addFoodToCart() {

        System.out.println("\n========== ADD TO CART ==========");

        System.out.print("Enter Food ID: ");
        int id = sc.nextInt();

        System.out.print("Enter Quantity: ");
        int quantity = sc.nextInt();

        sc.nextLine();

        Food selectedFood = findFood(id);

        if (selectedFood == null) {

            System.out.println("Invalid Food ID.");
            return;
        }

        if (quantity <= 0) {

            System.out.println(
                    "Quantity must be greater than 0."
            );

            return;
        }

        for (int i = 0; i < quantity; i++) {

            cart.add(selectedFood);
        }

        System.out.println(
                quantity + " x " +
                selectedFood.name +
                " added to cart."
        );
    }

    // Find Food
    static Food findFood(int id) {

        for (Food food : menu) {

            if (food.id == id) {
                return food;
            }
        }

        return null;
    }

    // View Cart
    static void viewCart() {

        System.out.println("\n============== YOUR CART ==============");

        if (cart.isEmpty()) {

            System.out.println("Cart is empty.");
            return;
        }

        double total = 0;

        for (Food food : cart) {

            System.out.println(
                    food.name +
                    " - ₹" +
                    food.price
            );

            total = total + food.price;
        }

        System.out.println("----------------------------------------");
        System.out.println("Total Amount: ₹" + total);
    }

    // Calculate Total
    static double calculateTotal() {

        double total = 0;

        for (Food food : cart) {

            total = total + food.price;
        }

        return total;
    }

    // Select Payment
    static String selectPayment() {

        System.out.println("\n========== PAYMENT METHOD ==========");

        System.out.println("1. UPI");
        System.out.println("2. Card");
        System.out.println("3. Cash on Delivery");

        System.out.print("Enter choice: ");

        int choice = sc.nextInt();

        sc.nextLine();

        if (choice == 1) {

            return "UPI";

        } else if (choice == 2) {

            return "Card";

        } else if (choice == 3) {

            return "Cash on Delivery";

        } else {

            System.out.println(
                    "Invalid choice. UPI selected."
            );

            return "UPI";
        }
    }

    // Make Payment
    static void makePayment(String payment) {

        double total = calculateTotal();

        System.out.println("\n========== MAKE PAYMENT ==========");

        System.out.println("Payment Method: " + payment);
        System.out.println("Amount: ₹" + total);

        System.out.println("Processing payment...");

        System.out.println("Payment Successful!");
    }

    // Order Confirmation
    static void confirmOrder(String payment) {

        double total = calculateTotal();

        System.out.println("\n========== ORDER CONFIRMATION ==========");

        System.out.println("Order ID: ORD" + orderId);
        System.out.println("Customer: " + customer.name);
        System.out.println("Phone: " + customer.phone);
        System.out.println("Address: " + customer.address);
        System.out.println("Payment: " + payment);
        System.out.println("Total: ₹" + total);
        System.out.println("Status: CONFIRMED");
    }

    // Save Order
    static void saveOrder(String payment) {

        try {

            FileWriter writer =
                    new FileWriter("orders.txt", true);

            writer.write("--------------------------------------\n");
            writer.write("Order ID: ORD" + orderId + "\n");
            writer.write("Customer: " + customer.name + "\n");
            writer.write("Phone: " + customer.phone + "\n");
            writer.write("Address: " + customer.address + "\n");

            writer.write("Items:\n");

            for (Food food : cart) {

                writer.write(
                        food.name +
                        " - ₹" +
                        food.price +
                        "\n"
                );
            }

            writer.write(
                    "Total: ₹" +
                    calculateTotal() +
                    "\n"
            );

            writer.write(
                    "Payment: " +
                    payment +
                    "\n"
            );

            writer.write("Status: CONFIRMED\n");
            writer.write("--------------------------------------\n");

            writer.close();

            System.out.println(
                    "\nOrder saved successfully in orders.txt"
            );

            orderId++;

        } catch (IOException e) {

            System.out.println(
                    "Error saving order."
            );
        }
    }
}
