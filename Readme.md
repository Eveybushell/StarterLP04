# Step 1
### Conditions
1. The reservation is between 1 and 31 days, inclusive
2. The reservation includes between 1 and 12 guests, inclusive
3. The reservation includes more than 5 guests.
4. The check-in date is before the checkout date.
5. Every night in the date range is available.

### Actions
1. `ValueError`
2. `UnavailableError`
3. There is an additional guest fee
4. Payment is calculated based on nights stayed and cleaning fee
5. The reservation is created

# Step 2

| Conditions and Actions         | Rule 1 | Rule 2 | Rule 3 | Rule 4 | Rule 5 | Rule 6 |
| ------------------------------ | :----: | :----: | :----: | :----: | :----: | :----: |
| Dates are valid                |    T    |    T    |    -    |    T    |    F    |    -    |
| Number of nights is valid      |    T    |    T    |    -    |    T    |    -    |    F    |
| Number of guests is valid      |    T    |    T    |    -    |    F    |    -    |    -    |
| Requested nights are available |    T    |    T    |    F    |    T    |    -    |    -    |
| More than five guests          |    T    |    F    |    -    |    T    |    -    |    -    |
| **Create reservation**         |    X    |    X    |        |        |        |        |
| **Calculate standard price**   |    X    |    X    |        |        |        |        |
| **Add extra guest fee**        |    X    |        |        |        |        |        |
| **Raise `ValueError`**         |        |        |        |    X    |    X    |    X    |
| **Raise `UnavailableError`**   |        |        |    X    |        |        |        |

# Step 3

### TC01: Valid Reservation With No Extra Guest Fee

- **Check-in:** October 1
- **Checkout:** October 4
- **Number of guests:** 3
- **Availability:** Every requested night is available.
- **Expected Result:** The reservation is created.
- **Expected Price:** `$650`
  - Three nights at $200: `$600`
  - One-time cleaning fee: `$50`

### TC02: Valid Reservation With Extra Guest Fee

- **Check-in:** October 1
- **Checkout:** October 4
- **Number of guests:** 8
- **Availability:** Every requested night is available.
- **Expected Result:** The reservation is created.
- **Expected Price:** `$875`
  - Three nights at $200: `$600`
  - One-time cleaning fee: `$50`
  - Extra Guest Fee (3 guests at $25 per guest for 3 nights): `$225`

### TC03: Invalid Reservation With Unavailable Dates

- **Check-in:** December 1
- **Checkout:** December 31
- **Number of guests:** 3
- **Availability:** Several requested nights are unavailable.
- **Expected Result:** Throw `UnavailableError`

### TC04: Invalid Reservation With Excessive Guests

- **Check-in:** October 1
- **Checkout:** October 4
- **Number of guests:** 25
- **Availability:** Every requested night is available.
- **Expected Result:** Throw `ValueError`

### TC05: Invalid Reservation With Excessive Nights

- **Check-in:** January 10
- **Checkout:** October 4
- **Number of guests:** 3
- **Availability:** Several requested nights are unavailable.
- **Expected Result:** Throw `ValueError`

### TC06: Invalid Reservation With Same Day Check In Check Out

- **Check-in:** December 19
- **Checkout:** December 19
- **Number of guests:** 3
- **Availability:** Every requested night is available.
- **Expected Result:** Throw `ValueError`

