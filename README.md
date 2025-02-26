**Introduction**

For this project, we chose the card game **"War"**, a popular game, especially among children, due to its simple rules and the ability to play with a variable number of participants. The choice of this game was mainly due to the fact that it is a game of **pure chance**, meaning that players cannot influence its course in any way, being entirely constrained by the random shuffling of the deck.

This makes it ideal for a **Monte Carlo simulation**, sparing us from implementing decision-making algorithms. However, the game's flow is complex enough that calculating the time required to complete a match is a challenging exercise, justifying the use of a Monte Carlo simulation.

**Game Rules**

Like any traditional game, "War" has multiple variations of rules, but the differences are not as significant as in other card games. For this project, we decided to implement the **two-player version without Jokers** (since Jokers introduce strategic play, eliminating the pure chance aspect we aim to analyze).

The game starts with the **shuffling of the deck**, which is by far the most crucial step, as the initial deck configuration entirely determines the winner and the match duration. The deck is then **evenly divided**, with each player receiving **26 cards**, and the actual game begins with these face-down piles in front of each player.

The game is structured into rounds:

- At the beginning of each round, both players draw the **top card** from their pile and place them side by side.

- The player with the **higher value** card takes both cards and places them at the bottom of their deck.

- Face cards (Jack, Queen, King, and Ace) have assigned values of **11 (or 15), 12, 13, and 14**, depending on the chosen variation.

If the two drawn cards have **equal values**, a "war" is triggered, which is the game's defining feature:

- Players must place a number of additional cards **equal to the tied value**.

- If a player runs out of cards during the war, they stop placing them, and the war continues until all remaining players have placed the required number of cards.

- The last cards placed are compared, and the player with the **higher value** wins all the cards in the war.

- If another tie occurs, another war begins using the **new tied value**.

The game **ends** when a player runs out of cards **after a round**, making them the loser.

- However, if a player runs out of cards during a war but wins it, the game continues.

- If both players run out of cards and the last cards placed are tied, the match ends in a draw.

**Problem Statement**

Since predicting the **winner** of this game is not a very complex probability exercise, our main goal is to determine **how long a match lasts**.

To translate the problem into **easily understandable terms**, we measure the game's duration in **both rounds** and **an approximate real-time duration** based on the number of rounds played. Based on personal experience, a round (drawing cards, comparing values, and adding them to the winner's pile) takes about **10 seconds**, including time spent arranging cards between rounds. Each individual card placement in a war is also considered a separate round.

The key question we seek to answer is:

**"On average, how long does a game of War last?"**

Beyond this, we also analyze additional statistical aspects:

- **"What is the probability that a game lasts more than an hour?"**

- **"What kind of distribution can we expect for game durations?"**

- **"What is the average number of wars per game? How about in games that last over an hour?"**

- **"What is the correlation between the number of wars and game duration?"**

- **"Is there a minimum or maximum threshold for game duration?"**

**Problem Solving**

**Step 1: Preparing the Deck**

```
deck = list(range(2, 15)) * 4
np.random.shuffle(deck)
```

- We create a **complete 52-card deck** (values **2--14**, four times each).

- We shuffle the deck using **np.random.shuffle**.

**Step 2: Distributing the Cards**

```
player1 = deque(deck[:26])
player2 = deque(deck[26:])
```

- The deck is **evenly split** into **26 cards per player**.

- Each player's cards are stored in a **deque** (double-ended queue) for efficient card handling.

**Step 3: Simulating the Rounds**

```
rounds = 0
wars = 0
while player1 and player2:
```

- rounds keeps track of the **number of rounds**.

- wars counts how many **wars** occur.

- The game **continues until one player runs out of cards**.

**Monte Carlo Simulation**

Monte Carlo algorithms **simulate random events multiple times** to derive statistical insights.

In our case, we use Monte Carlo simulations to play **thousands of War games**, analyzing results to answer our key questions.

To estimate the **average game duration**, we use the rounds vector, which stores the number of rounds for each game simulation. The **mean of this vector** provides an empirical estimate.

**Estimating the Required Number of Simulations**

**Central Limit Theorem**

The **Central Limit Theorem (CLT)** states that for **n independent, identically distributed random variables** with **mean µ** and **variance σ²**, the distribution of:

approaches a standard normal distribution.

To **verify CLT** in our case:

- We **simulate multiple Monte Carlo runs**.

- We **compute the histogram** of the obtained values using the above formula.

**Chernoff-Hoeffding Inequality**

This inequality provides an upper bound on the probability that the **Monte Carlo approximation differs significantly from the true mean**.

For a **bounded** random variable:

Using this formula, we estimate the number of simulations required to achieve a **desired accuracy**.

- To ensure the **estimated mean is within ±1** with **99% confidence**, we need **370,554 simulations**.

- If we **allow a ±10 error margin**, we only need **3,500 simulations**.

**Results & Analysis**

**How long does a game of War last on average?**

The average duration is **120 rounds** (~**20 minutes**).

**What is the probability that a game lasts more than an hour?**

Only **~2%** of games last over an hour.

**What is the expected distribution of game durations?**

Using a **histogram**, we observe:

- A **minimum threshold** of **26 rounds** (best-case scenario).

- A peak near **80 rounds**.

- A **long-tail distribution** with rare extremely long games.

**How many wars occur per game?**

- The **average** game has **5.42 wars**.

- Games lasting **over an hour** average **18.76 wars**.

**Correlation between wars & game duration**

By **plotting rounds vs. wars**, we find a **strong correlation**:

- Games with **many rounds** tend to have **many wars**.

- The **longer** the game, the **more wars occur**.
