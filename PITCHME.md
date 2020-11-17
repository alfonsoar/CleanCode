## Clean Code 

#### **NIC** Practical Programmer Series

<br /><br /><br /><br /><br />
##### Joing our Symphony room: "Practical Programmer Series"

---

### Practical Programmer Series

- Design Patterns
- Functional Programming
- **Clean Code**

---

### Audience

- You are a programmer |
- You want to become a better programmer |
- Or ... |

---

### Why is Clean Code Important

![Graph](img/ProgrammerTime.jpg){ width=80%, height=80% }

>The ratio of time spent reading versus writing is well over 10 to 1. We are
>constantly reading old code as part of the effort to write new code

---

### What is Clean Code to you?

+++

> Clean code is **simple** and direct. Clean code reads like well-written **prose**. Clean code never obscures the designer’s intent but rather is full of crisp abstractions and straightforward lines of control.

+++

> Clean code always looks like it was written by **someone who cares**.

---

### Agenda

- Naming |
- Functions |
- Code Readability |
- Refactoring |

---

### Naming

- Intention revealing names |
- Meaningful distinctions | 
- Searchable names |
- Avoid mental mapping |
- Nouns for classes and variables and verbs for functions |

---
#### Intention revealing names

```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for (int[] x : theList)
    if (x[0] == 4)
      list1.add(x);
  return list1;
}
```

- What does this code do? |
- Why does it do it? |

+++

---

#### Compared to

```java
public List<Cell> getFlaggedCells() {
  List<Cell> flaggedCells = new ArrayList<Cell>();
  for (Cell cell : gameBoard)
    if (cell.isFlagged())
      flaggedCells.add(cell));
  return flaggedCells;
}
```

- How about now? |

---

#### Meaningful distinction

##### Function Names

```
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
```

@[1-3](Which one would you use to get the Active Account?)

+++

##### Variable Names

```
moneyAmount vs money
customerInfo vs customer
theMessage vs message
accountData vs account
```

@[1-4](Don't make variables compete for attention)

---

#### Use Problem Domain Names

 - Learn the vocabulary of your users and customers
 - Use it
 - (But don't overdo it)

---

#### Avoid Mental Mapping

```python
for i in range(34):
  s += (t[i]*4)/5;
```

@[2](Clearly everyone knows why we multiply times 4 and divide by 5)

+++ 

#### Searchable Names

```python
real_days_per_ideal_day = 4
work_days_per_week = 5
number_of_tasks = len(task_estimates)
sum = 0
for i in range(number_of_tasks):
  real_days = task_estimates[i] * real_days_per_ideal_day
  real_weeks = (real_days / work_days_per_week)
  sum += real_weeks
```
@[1-8](How about now?)
@[4](Although this name is great, it shadows a built in function)
@[3, 5](Why acces via array? Why not For In?)

---

#### Good naming can replace comments

```python
# Check to see if the employee is eligible for bonus
if (employee.rating >= 4 and employee.years > 5)

if (employee.isEligibleForBonus())
```

---

### Functions

- Functons should be small, the smaller the better |
- A function should only do one thing |
- One level of abstraction |
- Less arguments are better |
- Impure Sandwich -> No side effects* |

---

#### Building Blocks

![Blocks](img/Blocks.jpeg)

Note:
- Level Of Astraction
- Composing
- Small

---

#### Code Examples 

```java
public void Checkout()
{
  Price CurrentPrice = new Price();
  foreach(var product in CurrentShoppingCart)
    CurrentPrice.Add(product.Price);
  foreach(var coupon in CurrentDiscounts)
    CurrentPrice.Discount(coupon.Price);

  var billingInfo = BillingRepository.get(BillingId);
  if (billingInfo.CheckIfStillValid() == False)
  {
    DisplayError("Invalid Billing Information")
    return;
  }

  var paymentResult = PaymentGateway.Charge(billingInfo, CurrentPrice);
  if (paymentResult.Success == false)
  {
    DisplayError("Payment Processing Error");
    return;
  }
  DisplayMessage("Thank you for your order");
}
```
@[3-7](We first Calculate our Price by summing product prices and subtracting discount prices)
@[9-14](We then confirm if the billing info is valid)
@[16-23](Finally we charge the billingInfo card the currentPrice)
@[1-24](Think we can use our magic refactoring wand?)

+++

```java
public void Checkout()
{
  UpdateCurrentTotalPrice()
  ChargeCustomer()
  DisplayResult()
}

private void UpdateCurrentTotalPrice()
{
  Price CurrentPrice = new Price()
  SumShoppingCartProductsToCurrentPrice(CurrentPrice);
  ApplyShoppingCartDiscountsToCurrentPrice(CurrentPrice);
}

private void SumShoppingCartProductsToCurrentPrice()
{
  foreach(var product in CurrentShoppingCart)
    CurrentPrice.Add(product.Price);
}

// etc ...
```

@[1-6](High Level Abstraction)
@[8-13](Mid Level Abstraction)
@[15-19](Detail Level)

---

#### Impure Sandwich

+++

```python
# Simplified version of making a reservation at a restaurant
def makeReservation(quantity, date, restaurantId):
	restaurant = Restaurant.getRestaurant(restaurantId) # ORM 
	reservationsOnDate = filter(lambda reservation: reservation.date == date, restaurant.reservations)
	available_seats = restaurant.capacity - len(reservationsOnDate)
	if available_seats >= quantity:
		return Restaurant.reserve(data, quanitity) # Returns a reservation String from db call
	else:
		return None # Client Handles None
```

@[2-9](Code is easy to read, seems straightfoward what we are doing)
@[3](Here is our culprit - we are mixing a database call with our business logic)
@[4-6](This is our business logic)
@[7](More of this impure sandwich!)

+++

```python
# Impure Function Call
def getRestaurant(restaurantId):
	restaurant = Restaurant.getRestaurant(restaurant)


# Business Logic
@_.curry
def canMakeReservation(quantity, date, restaurant):
	reservationsOnDate = filter(lambda reservation: reservation.date == date, restaurant.reservations)
	available_seats = restaurant.capacity - len(reservationsOnDate)
	if available_seats >= quantity:
		return Some(Reservation(quantity, date)) # Creates a reservation object
	else:
		return None

# Impure Function Call
def reserve(reservation):
	return Restaurant.reserve(reservation)

# Impure Sandwich
def makeReservation(quantity, date, restaurantId):
	return _.compose(
			getRestaurant(restaurantId),
			canMakeReservation(quantity, date),
			lambda reservation: reserve(reservation) if reservation else None # Does your language have Options?
		)
```
@[1-3](First Impure DB Call)
@[6-14](Business Logic Pure Function)
@[16-18](Second Impure DB Call)
@[20-26](We build our impure sandwich via compose)
@[25](**Note** This is a Ternary Operation, Does your language support Option?)

---

![Comic Style Guide](img/StyleGuide.png)

---

### Code Readability

- Nested Code is :( |
- Looping |
- Formatting Code |

---

#### Nested Code

---

#### Looping 

```javascript
// Artists with popularity <= 3 percentage of given playlist
func getHipsterArtists(playlist) {
	const maxPopularity = 3
	let artists = []
	// Get All the artists from a playlist and filter only unpopular artists
	for(let songIndex = 0; songIndex < playlist.length; songIndex++) {
		const artistsCount = playlist[songIndex].artists.length
	    for (let artistIndex = 0; artistIndex  < artistsCount; artistIndex++) {
	        if playlist[songIndex][artistIndex].popularity <= 3 {
	        	artists.append(playlist[songIndex][artistIndex].name)
	        }
	}
	return artists
}

// Functional with ES6
const getHipsterArtists = (playlist) => {
	const maxPopularity = 3
	return playlist.
		flatMap(song => song.artists).
		filter(artists => artists.popularity <= maxPopularity).
		map(artists => artists.name)
}
```
@[2-13](Imperative Programming, you have to read deep in the code to see exactly whats happening)
@[15-22](Functional Programming, concise and clean, generally no need for comments)
@[2-13](This is a small example, but have you seen double/triple for loops with maybe 3 or 4 if statements inside?)

---

#### Formatting Code

- Horizontal - Line Length |
- Vertical - File Size |
- Indentation |
- Style Guide & Linter |
- Consistency |

---

### Refactoring

- Types of Requirements |
- Refactoring |
- Technical Debt |
- Code Smells |

---

![Stickmen](img/Refactoring.png)

---

#### Types of Requirements

- Functional |
- Operational | 
- Developmental |

Note:
- Functional: Use cases, features
- Operational: Performance, SLA
- Developmental: Developers QOL
- Raise hand if you have had a refactoring sprint or investment to just improve code readability

---

#### Why Refactor

+++

>By continuously improving the design of code, we make it easier and easier to work with.

---

#### What Is Refactoring

+++

> Code Refactoring is the process of clarifying and simplifying the design of existing code, without changing its behavior.

---

#### When?  

- Brittle Code |
- Turn around times on Functional Requirements increased |
- Debugging takes longer |
- Onboarding New Joiners takes longer |
- Code Smells |

---

#### Code Smells

+++

> A code smell is a surface indication that usually corresponds to a deeper problem in the system

---

#### Code Smell Categories

- Bloaters |
- Object Orientation Abusers |
- Change Preventers |
- Dispensables |
- Couplers |

Note:
- Gargantuan Proportions
- Incorrect Application of OO
- Makes change so expensive you dont want to do it
- Pointless/uneeded
- Excessive Coupling 

---

### How to write unmaintanable code

- Use horrible naming for variables and functions |
- Don't break up your functions into abstract levels |
- Make your code look like a maze |
- Compete to see who has the most technical debt |

--- 

### Acknowledgements

- Robert "Uncle Bob" Martin
- Enrique Padilla (Ex-GS)
- Aditya Dalal (GS)
---

## Thank you!
