Alright — let’s go question by question and I’ll tell you exactly:
1️⃣ which files you must edit, and
2️⃣ the updated code for each step.

⸻

Q1 – Customer class

File to update: Customer.java
Add variables & getters.

// Name: Your Name
// Student Number: Your Student Number

package YOUR_PACKAGE_NAME;

import java.util.List;

public class Customer {
    private int id;
    private String firstName;
    private String lastName;
    private String phone;
    private List<Product> purchasedProducts;

    public int getId() {
        return id;
    }
    public String getFirstName() {
        return firstName;
    }
    public String getLastName() {
        return lastName;
    }
    public String getPhone() {
        return phone;
    }
    public List<Product> getPurchasedProducts() {
        return purchasedProducts;
    }
}

✅ Commit: "Question 1 complete"

⸻

Q2 – Product class

File to update: Product.java
Add variables, getters, toString().

// Name: Your Name
// Student Number: Your Student Number

package YOUR_PACKAGE_NAME;

public class Product {
    private String sku;
    private String name;
    private double salePrice;
    private double regularPrice;
    private String image;

    public String getSku() {
        return sku;
    }
    public String getName() {
        return name;
    }
    public double getSalePrice() {
        return salePrice;
    }
    public double getRegularPrice() {
        return regularPrice;
    }
    public String getImage() {
        return image;
    }

    @Override
    public String toString() {
        return name + "-$" + String.format("%.2f", salePrice);
    }
}

✅ Commit: "Question 2 complete"

⸻

Q3 – Reading JSON

New files to create:
	•	ApiResponse.java
	•	JsonReaderUtil.java

ApiResponse.java:

package YOUR_PACKAGE_NAME;

import java.util.List;

public class ApiResponse {
    private List<Customer> customers;

    public List<Customer> getCustomers() {
        return customers;
    }
}

JsonReaderUtil.java:

package YOUR_PACKAGE_NAME;

import com.google.gson.Gson;
import com.google.gson.stream.JsonReader;

import java.io.FileReader;
import java.io.IOException;
import java.util.List;

public class JsonReaderUtil {
    public static List<Customer> readCustomers(String filePath) {
        try (JsonReader reader = new JsonReader(new FileReader(filePath))) {
            Gson gson = new Gson();
            ApiResponse response = gson.fromJson(reader, ApiResponse.class);
            return response.getCustomers();
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }
    }
}

✅ Commit: "Question 3 complete"

⸻

Q4 – Special methods

File to update: Customer.java
Add the three methods.

public double getTotalPurchases() {
    return purchasedProducts.stream()
            .mapToDouble(Product::getSalePrice)
            .sum();
}

public double getTotalSaved() {
    return purchasedProducts.stream()
            .mapToDouble(p -> p.getRegularPrice() - p.getSalePrice())
            .sum();
}

public boolean hasSavedFiveOrMore() {
    return getTotalSaved() >= 5.0;
}

✅ Commits:
	•	"Question 4.1 complete"
	•	"Question 4.2 complete"
	•	"Question 4.3 complete"

⸻

Q5 – Name & student number

Files to update:
	•	Customer.java
	•	Product.java
	•	ApiResponse.java
	•	JsonReaderUtil.java
	•	TableViewController.java

At the very top of each file:

// Name: Your Name
// Student Number: Your Student Number

✅ Commit: "Question 5 complete"

⸻

Q6 – TableView setup

File to update: TableViewController.java

Inside:

@FXML
public void initialize() {
    idColumn.setCellValueFactory(new PropertyValueFactory<>("id"));
    firstNameColumn.setCellValueFactory(new PropertyValueFactory<>("firstName"));
    lastNameColumn.setCellValueFactory(new PropertyValueFactory<>("lastName"));
    phoneColumn.setCellValueFactory(new PropertyValueFactory<>("phone"));
    totalPurchaseColumn.setCellValueFactory(c ->
        new SimpleStringProperty("$" + String.format("%.2f", c.getValue().getTotalPurchases()))
    );

    List<Customer> customers = JsonReaderUtil.readCustomers("customers.json");
    tableView.getItems().setAll(customers);
    rowsInTableLabel.setText("Rows returned: " + tableView.getItems().size());
}

✅ Commit: "Question 6 complete"

⸻

Q7 – Dynamic row label

File to update: TableViewController.java
Add method:

private void updateRowCount() {
    rowsInTableLabel.setText("Rows returned: " + tableView.getItems().size());
}

Call updateRowCount() after loading or filtering data.
✅ Commit: "Question 7 complete"

⸻

Q8 – Customer selection listener

File to update: TableViewController.java
Inside initialize():

tableView.getSelectionModel().selectedItemProperty().addListener((obs, oldVal, newVal) -> {
    if (newVal != null) {
        purchaseListView.getItems().setAll(newVal.getPurchasedProducts());
    }
});

✅ Commit: "Question 8 complete"

⸻

Q9 – Update purchase labels

Still in initialize() listener:

double totalMsrp = newVal.getPurchasedProducts().stream()
        .mapToDouble(Product::getRegularPrice).sum();
double totalSale = newVal.getPurchasedProducts().stream()
        .mapToDouble(Product::getSalePrice).sum();
double totalSavings = totalMsrp - totalSale;

msrpLabel.setText("$" + String.format("%.2f", totalMsrp));
saleLabel.setText("$" + String.format("%.2f", totalSale));
savingsLabel.setText("$" + String.format("%.2f", totalSavings));

✅ Commit: "Question 9 complete"

⸻

Q10 – Filter customers saved $5+

File to update: TableViewController.java

@FXML
private void filterSavedFive() {
    List<Customer> filtered = JsonReaderUtil.readCustomers("customers.json")
            .stream()
            .filter(Customer::hasSavedFiveOrMore)
            .toList();
    tableView.getItems().setAll(filtered);
    updateRowCount();
}

✅ Commit: "Question 10 complete"

⸻

Q11 – Load all customers button

File to update: TableViewController.java

@FXML
private void loadAllCustomers() {
    List<Customer> customers = JsonReaderUtil.readCustomers("customers.json");
    tableView.getItems().setAll(customers);
    updateRowCount();
}

✅ Commit: "Question 11 complete"

⸻

Q12 – Display image

File to update: TableViewController.java
Inside initialize():

purchaseListView.getSelectionModel().selectedItemProperty().addListener((obs, oldVal, newVal) -> {
    if (newVal != null && newVal.getImage() != null && !newVal.getImage().isEmpty()) {
        imageView.setImage(new Image(newVal.getImage()));
        imageView.setVisible(true);
    }
});

✅ Commit: "Question 12 complete"

⸻

If you want, I can now send you a ready-to-use full code bundle for Customer.java, Product.java, ApiResponse.java, JsonReaderUtil.java, and the final TableViewController.java with all questions implemented in order, so you can commit step-by-step without guessing. That would make this test much smoother for you.
