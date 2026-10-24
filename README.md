# AD361-LP04 Black Box Testing

## Step 1: Select a Website

**Website Name:** Twitch

**Website's URL:** [twitch.tv](https://www.twitch.tv/)

**One or two features you will test:** `Sign-up form` (birthday and password fields) and `Follow` Button.

## Step 2: Equivalence Partitioning

**Feature tested:** Birthday field on the Twitch sign-up page [twitch.tv/signup](https://www.twitch.tv/signup)

**Requirement:** A user must enter a real, complete date of birth and be at least 13 years old to create an account.

### Valid Partitions

- **TC1:** A complete and real date where the user's age is 13 or older.
- **TC2:** A real date where the user is under 13 years old.
- **TC3:** A date in the future.
- **TC4:** An impossible calendar date (the day doesn't exist in that month).
- **TC5:** An incomplete date (one or more parts left empty).
- **TC6:** An unrealistically old year.

| ID  | Partition                | Valid/Invalid | Representative Value       | Expected Result                                                                                 |
| --- | ------------------------ | ------------- | -------------------------- | ----------------------------------------------------------------------------------------------- |
| TC1 | Age 13 or older          | Valid         | June 10, 2003 (age 23)     | Birthday is accepted with no error, user can continue signing up                                |
| TC2 | Under 13                 | Invalid       | March 28, 2018 (age 8)     | Error message shown saying the user doesn't meet the age requirement, account cannot be created |
| TC3 | Future date              | Invalid       | January 5, 2030            | Error message shown for an invalid birthday, account cannot be created                          |
| TC4 | Impossible date          | Invalid       | February 30, 2005          | Error message shown for an invalid date, account cannot be created                              |
| TC5 | Incomplete date          | Invalid       | June 10, (year left empty) | Field is flagged as incomplete and the Sign Up button stays disabled                            |
| TC6 | Unrealistically old year | Invalid       | June 10, 1850              | Error message shown for an invalid birthday, account cannot be created                          |

## Step 3: Decision Table Testing

**Sign Up Rules:** A password is accepted only if it has at least 8 characters, at least one special character, and at least one number.

### Decision Table

|                                      | TC1 | TC2 | TC3 | TC4 |
| ------------------------------------ | --- | --- | --- | --- |
| **Condition:** Has 8+ characters     | T   | F   | T   | T   |
| **Condition:** Has special character | T   | –   | F   | T   |
| **Condition:** Has number            | T   | –   | –   | F   |
| **Action:** Password accepted        | X   |     |     |     |
| **Action:** Password rejected        |     | X   | X   | X   |

### Test Cases

| ID  | Password     | Length | Expected Result      |
| --- | ------------ | ------ | -------------------- |
| TC1 | `pokeski26!` | 10     | Password is accepted |
| TC2 | `poke26!`    | 7      | Password is rejected |
| TC3 | `pokeski26`  | 9      | Password is rejected |
| TC4 | `pokeski!!`  | 9      | Password is rejected |

## Step 4: State Transition Testing
**Requirement:** Follow button on a Twitch channel page, assume the test account is not following the channel before testing begins.

| State/Event   | FOLLOW        | UNFOLLOW      | LOG IN        | LOG OUT    |
| ------------- | ------------- | ------------- | ------------- | ---------- |
| LOGGED OUT    | LOGGED OUT    | -             | NOT FOLLOWING | -          |
| NOT FOLLOWING | FOLLOWING     | -             | -             | LOGGED OUT |
| FOLLOWING     | -             | NOT FOLLOWING | -             | LOGGED OUT |

### Test Cases

|                 | TC1                | TC2           | TC3            | TC4              | TC5           | TC6           |
| --------------- | ------------------ | ------------- | -------------- | ---------------- | ------------- | ------------- |
| **Start State** | LOGGED OUT         | LOGGED OUT    | NOT FOLLOWING  | FOLLOWING        | NOT FOLLOWING | FOLLOWING     |
| **Trigger**     | Follow             | Log In        | Follow         | Unfollow         | Log Out       | Log Out       |
| **Action**      | Show login prompt  | Sign in user  | Follow channel | Unfollow channel | Sign out user | Sign out user |
| **End State**   | LOGGED OUT         | NOT FOLLOWING | FOLLOWING      | NOT FOLLOWING    | LOGGED OUT    | LOGGED OUT    |
