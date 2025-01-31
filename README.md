# Part2-Exercise4

## Classes
`Card`: Represents the user's payment card and handles balance management and ticket validity

`Ticket`: Encapsulates ticket details, including type, price, and expiration time

`ReaderDevice`: Represents the device that handles ticket purchases

`ServicePoint`: Represents locations where customers can load money onto their cards


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
 * Load a specified amount of money onto the card
 * @.pre amount > 0
 * @.post balance == OLD(balance) + amount
 */
public void loadMoney(double amount);

/**
 * Return the current balance of the card
 * @.pre true
 * @.post RESULT == balance
 */
public double getBalance();

/**
 * Return the current ticket on the card, if any
 * @.pre true
 * @.post RESULT == currentTicket
 */
public Ticket getCurrentTicket();

/**
 * Check if the card has an active ticket and return True if the current ticket is valid otherwise return false 
 * @.pre true
 * @.post RESULT == (currentTicket != null && currentTicket.isValid())
 */
public boolean hasValidTicket();

/**
 * Deduct the ticket price and set a new ticket as the current ticket
 * @param ticket The ticket to purchase
 * @.pre ticket != null && ticket.getPrice() <= balance
 * @.post balance == OLD(balance) - ticket.getPrice()
 *        && currentTicket == ticket
 */
public void purchaseTicket(Ticket ticket);

```

#### Class Invariants:

`balance >= 0:` Balance must never be negative

`currentTicket == null || currentTicket.isValid():` If a ticket exists, it must be valid




## Class: `Ticket`
#### Responsibilities:

1. Represent the ticket's type, price, and validity period
2. Provide mechanisms to check validity


#### Attributes: 
`private String type` - Ticket type: single, day, or monthly

`private double price`  - Price of the ticket

`private long expiration` - Expiration time in milliseconds 




#### Methods:
```java
/**
 * Construct a new ticket
 * @param type The type of the ticket (single, day, monthly)
 * @param price The price of the ticket
 * @param validityDuration The validity duration of the ticket in milliseconds
 * @.pre type != null && price > 0 && validityDuration > 0
 * @.post this.type == type && this.price == price
 *       && this.expiration == System.currentTimeMillis() + validityDuration
 */
public Ticket(String type, double price, long validityDuration);

/**
 * Check if the ticket is still valid and return True if the ticket is valid otherwise return false 
 * @.pre true
 * @.post RESULT == (System.currentTimeMillis() <= expiration)
 */
public boolean isValid();

/**
 * Return the price of the ticket
 * @.pre true
 * @.post RESULT == price
 */
public double getPrice();

```

#### Class Invariants:

1. `price > 0`: A ticket must always have a positive price
2. `expiration > System.currentTimeMillis()` when created, the expiration time must be in the future



## Class: ReaderDevice

#### Responsibilities:

1. Facilitate ticket purchases and balance inquiries on the bus
2. Enforce ticket purchase rules and ensure sufficient balance

#### Attributes:
```java

private double singleTicketPrice = 3.0
private double dayTicketPrice = 8.0
private double monthlyTicketPrice = 55.0

private long singleTicketValidity = 2 * 60 * 60 * 1000 // 2 hours
private long dayTicketValidity = 24 * 60 * 60 * 1000    // 24 hours
private long monthlyTicketValidity = 30 * 24 * 60 * 60 * 1000 // 30 days


```

#### Methods: 
```java
/**
 * Display the current balance of the card
 * @param card The card to check
 * @.pre card != null
 * @.post RESULT == card.getBalance()
 */
public double checkBalance(Card card);

/**
 * Facilitate ticket purchase
 * @param card The card used for the purchase
 * @param ticketType The type of ticket to purchase (single, day, monthly)
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
 * Load a specified amount of money onto the card
 * @param card The card to load money onto
 * @param amount The amount to load
 * @.pre card != null && amount > 0
 * @.post card.getBalance() == OLD(card.getBalance()) + amount
 */
public void loadMoney(Card card, double amount);

```

## Justifications
Encapsulation: Each class handles its own data and logic. For example, Card encapsulates balance and ticket validity, while Ticket encapsulates type, price, and validity duration

Tasks seperations: Card focuses on managing user data, while ReaderDevice handles interactions with the card. ServicePoint focuses on loading money

Reusability: The modular design allows Card and Ticket to be reused in other contexts or extended for additional features

Robustness: Class invariants and preconditions ensure that invalid states cannot occur

Efficiency: Using `System.currentTimeMillis()` ensures quick time calculations without requiring additional libraries

## System Flow
Customer Loads Money: At a service point, the user loads money onto their Card

Customer Boards a Bus: 
The ReaderDevice checks if the card has a valid ticket: If valid, no action is taken. However, if invalid, the user selects a ticket type and the system deducts the price if the balance is sufficient

Validity Check:
The Ticket class makes sure tickets expire at the right time, keeping them valid only for the set duration





















