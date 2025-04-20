### Test Design Process

This document describes my approach to test design, the process of creating a solution, and all my thoughts about what is happening.

Before writing tests, I need to explore the software product and answer the following questions:
- What does the function do? (**actions**)
- What affects her work? (**parameters** of actions)
- What do I need to test? (**values** параметров, of parameters, more exactly I need a model that will appear after defining values, I will create tests based on this model)
- What is important for users? (for example, expand or reduce the model based on common sense)

Usually for research I use the following base: specifications; product; user documentation; code inspections and code coverage analysis; statistic collection; communication with users, tech support, developers; external standards and regulations; experience and exploratory testing. In the current situation I have only very basic specification, so after a brainstorm I will make some assumptions about how the program works, based on my experience, which in reality **must be officially clarified** and fixed in any way.

**Actions** that the function performs, and their **parameters**, after let’s look at each one separately:
- Adds products to the cart
  - how the product is added
  - which product is added (more exactly, how it is verified in the system)
- Determines which discount to apply
  - set number of products in settings (N) for applying each rule
  - number of products in the cart
- Calculates the total purchase sum
  - set number of products in settings (N) for applying each rule
  - number of products in the cart
  - set prices in settings for the second and third rule

The first parameter of adding products to the cart is how the product is added: by clicking a button in the interface, scanning a barcode, entering a product code, searching by name, etc. That means our function can be used in any application with similar functionality. On the other hand, I only have access to the business logic, so I will assume that the function receives a product code as input. Later, during requirements implementation or interface development, the tests can be expanded. I will not test the code verification function, because the fact of its existence is based on an assumption.

In the second parameter I have only 3 values that could belong to one equivalence class, but since they are manually set in the settings, I prefer to test all 3. On the other hand, any tests will cover these checks if each product is added at least once, so it’s enough to make sure of this fact and there’s no need to include this parameter in the model.

The values of the next two parameters (“Determines which discount to apply” and “Calculates the total purchase amount”) overlap, so I will consider them together. First, I will assume that the N values mentioned in the task description are different values for different rules (although finally they can take the same value, it would be strange to create multiple different rules that work for the same number of products), but the same value within the first rule. I will redefine the variables for clarity in the following explanation:

1) FreeRule: buy **N** --> get **N** free
2) ReducedPriceRule: buy more than **M** --> pay **X** price
3) FractionPriceRule: buy more than **K** --> pay **Y**% of the original price

This brings up a few more questions. In the first rule, it’s unclear whether the cart is always doubled (since the task description also mentions “Buy one get one free”) or if it’s necessary to reach the quantity N, and only then the cart will be doubled? I will assume the second option.

Does each rule apply to a specific product or to the total number of products in the cart? I will assume that all rules refer to the total number of products. In that case, it would also be confusing for the user to set a different percentage Y for each product, so I assume it is set as one value for all products.

Are all three rules always applied, or is there a setting to disable each rule? I will assume that all three rules are always applied. In this case, it would be possible to “disable” a rule by setting the price configuration, setting the X price to the current price and the Y price to 100%.

I don’t know anything about the verification of N, M, and K, whether it is implemented or where it might be. One could assume that the rules and products in the config are stub for testing the discount calculation function, and later the rule settings will be moved to the interface, with their verification handled by a separate function. In any case, for now, I will assume that these settings are positive integers and not equal to zero. The same applies to prices, I assume the function receives prices as positive numbers.

Another important point is how the combination of prices and rule settings affects the customer experience. It’s unlikely that customers will be happy if price X is higher than the initial price, and if percentage Y is greater than 100. On the other hand, if X is set to 0, it could be very costly for the seller. Additionally, the customer might be surprised if rule 3 applies with a larger number of items in the cart, but the discount percentage Y ends up providing less savings than price X. All of this logic is critical and needs to be clearly addressed during the requirements phase.

For now, I assume that the price X is always greater than Y, and M is always strictly less than K. Therefore, I follow this logic for applying discounts: when purchasing M items (for example, 5), the customer gets a reduced price, and then when purchasing even more items (for example, 10), the customer gets an even greater discount. Additionally, at some point (e.g., at 5, 10, 15, etc.), an equal number of free items are added. Sounds good!

The final list of all assumptions:
-	The function accepts a product code as input, considering case sensitivity;
-	All rules apply to the total number of products in the cart;
-	N represents different values for different rules (redefined as N, M, K);
-	N is the same value within one rule;
- Rule one means that upon reaching N products, the same number of free products are added;
- Free products are displayed as a separate list, thus not being counted in the discount calculation;
- N, M, K > 0, integer;
- M < K;
- All three rules are always applied, they cannot be disabled, but prices can be set as X = Price, Y = 100;
- X and Y > 0, float, X <= Price, Y <= 100, X > Price*Y/100;
-	Y is the same for all products.

Let’s return to the model of action-parameter-values, and now I will proceed to define the **values** for the parameters. I’ll start with prices X and Y. As they belong to the same equivalence class, I will check one value based on the condition X > Price * Y / 100, as well as boundary values: 

X (£):
1) `[2.15, 4.43, 10.23]`
2) `[3.11, 5, 11.23]`

Y (%):
1) `[70, 70, 70]`
2) `[100, 100, 100]`

For the settings N, M, K, it’s important to cover all cases: n > m, n < m, n = m, n > k, n < k, n = k, with m < k at all times. Otherwise, they belong to the same equivalence class, plus the boundary values:

[n, m, k]:
1) `[5, 5, 7]` (n=m, n<k)
2) `[7, 5, 7]` (n>m, n=k)
3) `[10, 5, 7]` (n>k)
4) `[5, 7, 10]` (n<m)
5) `[1, 2, 3]`
6) `[3, 1, 2]`
7) `[1, 1, 2]`

The number of products in the cart by equivalence classes and boundary values in the context of N, M, K values:

Сart size (32 values):
1) `[5, 5, 7]: 4, 5, 6, 7` (4 values)
2) `[7, 5, 7]: 4, 5, 6, 7` (4 values)
3) `[10, 5, 7]: 4, 5, 6, 7, 9, 10` (6 values)
4) `[5, 7, 10]: 4, 5, 6, 7, 9, 10` (6 values)
5) `[1, 2, 3]: 1, 2, 3, 4` (4 values)
6) `[3, 1, 2]: 1, 2, 3, 4` (4 values)
7) `[1, 1, 2]: 1, 2, 3, 4` (4 values)

That's it, now I can write test cases combining these values, I can do it using different approaches depending on the priorities on the project, time, risks, etc. Here I will compare different approaches, and implement test cases for an approach "Minimum checks". Negative tests do not participate in these combinations, since obviously every negative fact needs to be checked in isolation, so I added negative tests to the test cases separately in excess of this number.

| Characteristics | Minimum checks | Full Enumeration | Atomic checks | Pairwise |
|---|---|---|---|---|
| Number of tests | 32 | 896 | 40  | 224 |
| Coverage depth | ~70% | 100%(*) | ~70%  | ~97%  |
| Ease of creation | Easy | Easy | Easy | Medium |
| Localization of defects | Difficult | Easy | Easy | Difficult |
| Application field |  Non-priority functionality, smoke tests | Critical functionality, automation |  Medium-priority functionality, automation  | High priority, tight deadlines  |

(*) 100% these specific values, exhaustive testing is not possible.
