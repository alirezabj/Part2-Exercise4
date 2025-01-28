# Part2-Exercise4

## Classes
`Card`: Represents the user's payment card and handles balance management and ticket validity.

`Ticket`: Encapsulates ticket details, including type, price, and expiration time.

`ReaderDevice`: Represents the device that handles ticket purchases, balance inquiries, and card loading.

`ServicePoint`: Represents locations where customers can load money onto their cards.


## Class: `Card`
#### Responsibilities:

1. Manage the card balance.
2. Track the validity of purchased tickets.
3. Check if a valid ticket exists and manage its expiration.

#### Attributes: 
`private double balance` - Current balance on the card in euros and cents

`private Ticket currentTicket` - The active ticket on the card, if any`

#### Methods: 
```java
/**
 * Loads a specified amount of money onto the card
 * @param amount The amount to be loaded (must be > 0)
 * @.pre amount > 0
 * @.post balance == OLD(balance) + amount
 */
public void loadMoney(double amount);

/**
 * Returns the current balance of the card
 * @return The card's balance
 * @.pre true
 * @.post RESULT == balance
 */
public double getBalance();

/**
 * Returns the current ticket on the card, if any
 * @return The current ticket or null if no ticket is valid
 * @.pre true
 * @.post RESULT == currentTicket
 */
public Ticket getCurrentTicket();

/**
 * Checks if the card has an active ticket
 * @return True if the current ticket is valid, otherwise return false 
 * @.pre true
 * @.post RESULT == (currentTicket != null && currentTicket.isValid())
 */
public boolean hasValidTicket();

/**
 * Deducts the ticket price and sets a new ticket as the current ticket
 * @param ticket The ticket to purchase
 * @.pre ticket != null && ticket.getPrice() <= balance
 * @.post balance == OLD(balance) - ticket.getPrice()
 *        && currentTicket == ticket
 */
public void purchaseTicket(Ticket ticket);

```

#### Class Invariants:

balance >= 0: Balance must never be negative.

currentTicket == null || currentTicket.isValid(): If a ticket exists, it must be valid.




## Class: `Ticket`
#### Responsibilities:

1. Represent the ticket's type, price, and validity period.
2. Provide mechanisms to check validity.


#### Attributes: 
```java

private String type;        // Ticket type: "single", "day", or "monthly".
private double price;       // Price of the ticket.
private long expiration;    // Expiration time in milliseconds since epoch.

```

#### Methods:
```java
/**
 * Constructs a new ticket.
 * @param type The type of the ticket ("single", "day", "monthly").
 * @param price The price of the ticket.
 * @param validityDuration The validity duration of the ticket in milliseconds.
 * @.pre type != null && price > 0 && validityDuration > 0
 * @.post this.type == type && this.price == price
 *       && this.expiration == System.currentTimeMillis() + validityDuration
 */
public Ticket(String type, double price, long validityDuration);

/**
 * Checks if the ticket is still valid.
 * @return True if the ticket is valid; false otherwise.
 * @.pre true
 * @.post RESULT == (System.currentTimeMillis() <= expiration)
 */
public boolean isValid();

/**
 * Returns the price of the ticket.
 * @return The ticket price.
 * @.pre true
 * @.post RESULT == price
 */
public double getPrice();

```

#### Class Invariants:

1. price > 0: A ticket must always have a positive price.
2. expiration > System.currentTimeMillis() when created: The expiration time must be in the future.



## Class: ReaderDevice

#### Responsibilities:

1. Facilitate ticket purchases and balance inquiries on the bus.
2. Enforce ticket purchase rules and ensure sufficient balance.

#### Attributes:
```java

private double singleTicketPrice = 3.0;
private double dayTicketPrice = 8.0;
private double monthlyTicketPrice = 55.0;

private long singleTicketValidity = 2 * 60 * 60 * 1000;  // 2 hours
private long dayTicketValidity = 24 * 60 * 60 * 1000;    // 24 hours
private long monthlyTicketValidity = 30 * 24 * 60 * 60 * 1000; // 30 days


```

#### Methods: 
```java
/**
 * Displays the current balance of the card.
 * @param card The card to check.
 * @.pre card != null
 * @.post RESULT == card.getBalance()
 */
public double checkBalance(Card card);

/**
 * Facilitates ticket purchase.
 * @param card The card used for the purchase.
 * @param ticketType The type of ticket to purchase ("single", "day", "monthly").
 * @.pre card != null && ticketType in {"single", "day", "monthly"}
 * @.post If card.hasValidTicket() == false && card.getBalance() >= ticketPrice,
 *        card.purchaseTicket(new Ticket(ticketType, ticketPrice, validityDuration))
 */
public void purchaseTicket(Card card, String ticketType);

```

## Class: ServicePoint

#### Responsibilities:

1. Provide functionality for users to load money onto their cards.

#### Methods:
```java
/**
 * Loads a specified amount of money onto the card.
 * @param card The card to load money onto.
 * @param amount The amount to load.
 * @.pre card != null && amount > 0
 * @.post card.getBalance() == OLD(card.getBalance()) + amount
 */
public void loadMoney(Card card, double amount);

```

## Justifications
Encapsulation:

1. Each class handles its own data and logic. For example, Card encapsulates balance and ticket validity, while Ticket encapsulates type, price, and validity duration.
Separation of Concerns:
2.Card focuses on managing user data, while ReaderDevice handles interactions with the card. ServicePoint focuses on loading money.

Reusability:

1. The modular design allows Card and Ticket to be reused in other contexts or extended for additional features (e.g., discounts).

Robustness:

1. Class invariants and preconditions ensure that invalid states cannot occur.

Efficiency:

1. Using `System.currentTimeMillis()` ensures quick time calculations without requiring additional libraries or system dependencies.

## System Flow
Customer Loads Money:
At a service point, the user loads money onto their Card.

Customer Boards a Bus:
The ReaderDevice checks if the card has a valid ticket:

If valid, no action is taken.
If invalid, the user selects a ticket type, and the system deducts the price if the balance is sufficient.
Validity Check:
The Ticket class ensures the expiration time is respected, ensuring accurate ticket validity.






















