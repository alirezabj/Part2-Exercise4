# Part2-Exercise4

## Classes
`Card`: Represents the user's payment card and handles balance management and ticket validity.
`Ticket`: Encapsulates ticket details, including type, price, and expiration time.
`ReaderDevice`: Represents the device that handles ticket purchases, balance inquiries, and card loading.
`ServicePoint`: Represents locations where customers can load money onto their cards.


## Class: `Card`
Responsibilities:

1. Manage the card balance.
2. Track the validity of purchased tickets.
3. Check if a valid ticket exists and manage its expiration.

#### Attributes: 
`private double balance;       // Current balance on the card in euros and cents`
`private Ticket currentTicket; // The active ticket on the card, if any`

#### Methods: 
`
/**
 * Loads a specified amount of money onto the card.
 * @param amount The amount to be loaded (must be > 0).
 * @.pre amount > 0
 * @.post balance == OLD(balance) + amount
 */
public void loadMoney(double amount);

/**
 * Returns the current balance of the card.
 * @return The card's balance.
 * @.pre true
 * @.post RESULT == balance
 */
public double getBalance();

/**
 * Returns the current ticket on the card, if any.
 * @return The current ticket or null if no ticket is valid.
 * @.pre true
 * @.post RESULT == currentTicket
 */
public Ticket getCurrentTicket();

/**
 * Checks if the card has an active ticket.
 * @return True if the current ticket is valid; false otherwise.
 * @.pre true
 * @.post RESULT == (currentTicket != null && currentTicket.isValid())
 */
public boolean hasValidTicket();

/**
 * Deducts the ticket price and sets a new ticket as the current ticket.
 * @param ticket The ticket to purchase.
 * @.pre ticket != null && ticket.getPrice() <= balance
 * @.post balance == OLD(balance) - ticket.getPrice()
 *        && currentTicket == ticket
 */
public void purchaseTicket(Ticket ticket);

`
