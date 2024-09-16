# Predictika.ai

## Background
[Predictika.ai](https://www.predictika.ai) has developed its own patented conversational AI Agents platform ([see link to patent announcement](https://www.prnewswire.com/news-releases/predictika-poised-to-revolutionize-customer-experience-everywhere-301735387.html)) and has been working with customers in a number of verticals such as education (e.g., website bots), restaurants (e.g., voice-based food ordering agents), hospitality (e.g., in-room customer support agents), and field service assistance agents.

## Objective

With the wide availability of LLM-based chat tools (e.g., ChatGPT, Gemini, etc) and wide interest in developing AI Agents that can automate various enterprise business processes, objective is to check how good such LLM chat tools are at 
(i) understanding arbitrary requests by users 
(ii) accurately following the business logic inherent to a business task and (iii) support the variety of conversational flows that are natural in a particular application.

## Test Business Domain
The business task we have used for our testing is ordering food, conversationally, from an Italian-style restaurant whose menu is fairly typical in its variety and complexity. 
We decided to test ChatGPT 3.5 (we used the OpenAI API calls to the **gpt-3.5-turbo-0125 model** and NOT the ChatGPT web app), treating it as a proxy for all LLM based chat tools. We did run some of our tests with other LLM-based chat tools just to see if there are significant variations in results and will report on that at the end

## Menu Structure
We took the menu from a typical Italian pizza restaurant since pizza orders have enough complexity to be a meaningful test for LLMs intelligence. 
The structure of a typical restaurant menu, breaking it down into a 4-level hierarchy, consists of:

1. **Menu Categories**: The top level includes broad categories such as Appetizers, Pizza, Calzone, Drinks, or Desserts. These categories are not directly ordered but organize the menu.
   
2. **Menu Items**: These are the actual items customers order, like specific dishes or drinks. Some items are simple, while others require further specifications, such as "Create Your Pizza" or "Salads."

3. **Modifier Groups**: For certain menu items, there are options (modifiers) that allow customization. These groups list possible choices (Modifier Items) and set rules on how many items can or must be selected, expressed through min/max restrictions.

4. **Modifier Items**: These are the specific choices within the Modifier Groups, like toppings for a pizza or salad dressings. Each modifier item may affect the final price of the order.

Finally, an order comprises one or more menu items, along with any chosen modifiers, and prices are rolled up from both menu and modifier items (not considering taxes, service charges etc).

## Testing Methodology 
While running our tests, we followed three different scenarios:
<ol type="a">
  <li>Use ChatGPT as is without giving it our menu.</li>
  <li>Use ChatGPT and pass the menu in the system prompt.</li>
  <li>Give ChatGPT additional instructions in the system prompt to contain more logic instructions.</li>
</ol>

## Summary of failures
<ol type="a">
  <li>Fails to do partial match and simply accepts the wrong item, even though it does reject items that do not match at all.</li>
  <li>While it does reject menu items that are clearly not in the menu, it is quite happy to add options to customizable items that are not in the menu.</li>
  <li>Very poor at customization. (i) Forgets to ask for options. (ii) Asks for wrong options, sometimes ones that are not even in the same category. (iii) Fails to enforce compatibility rules. (iv) It's clueless about ordering an item without one of its  ingredients even if it is given an explicit description of the ingredients of the item.</li>
  <li>It has a very hard time correctly enforcing quantity limits for options that have a max limit on how many options you can add from a group.</li>
  <li>Even though failure to do arithmetic is a known problem at least with ChatGPT 3.5, we were still surprised that even for simple total price calculations it failed in so many different ways.</li>
  <li>Very inconsistent behavior when ordering multiple items that are incomplete. It simply forgets to ask for the missing information even for the first item.</li>
  <li>ChatGPT failed in enforcing very simple constraints for half-and-half pizza, i.e., that both halves have to be the same size and have the same crust. This despite being given explicit instructions as part of its system prompt. It fails in some cases in really treating half and half as a single pizza and ordered each half as a separate pizza.</li>
  <li>Its ability to explain itself or revise its answer when challenged looks rather bogus. It simply comes up with another answer - sometimes the correct one and at other times equally wrong. It feels like it's just generating a different set of sentences without any understanding of its mistake.</li>
</ol>

## Contributors
- [webmaster@predictika.com](webmaster@predictika.com)