Act as an expert Python developer using LangGraph.
Your task is to build a deterministic order processing system for a bakery using a LangGraph graph in a Google Colab notebook. 
CRITICAL: Do NOT use any LLMs or AI calls. Everything must be pure deterministic Python.

1. STATE DEFINITION
Define a State (TypedDict) with EXACTLY these keys:
- status: "accepted" or "rejected"
- item: the actual product sold (after any fallbacks)
- delivery: "pickup" or "delivery"
- total: the final amount to pay, including delivery
- message: the reply to the customer
- steps: a list of strings recording the steps/nodes the order went through.

2. CONSTANTS (MENU & INVENTORY)
- challah: price 18, inventory 10
- sourdough: price 25, inventory 4
- rugelach: price 30, inventory 3
- babka: price 45, inventory 0
(Ignore croissants).

3. INPUT
The graph will be invoked with a dictionary containing: 'item' (str), 'quantity' (int), 'day' (str), 'city' (str). If 'city' is empty, assume pickup.

4. NODES & LOGIC
Node 1: Validation
- Check if quantity <= 0 or if the item is not in the menu.
- If invalid: status="rejected", message="We couldn't understand the order.", append to 'steps', end graph.
- If valid: append to 'steps', continue to Inventory.

Node 2: Inventory & Fallback
- Check if the requested item has enough inventory for the requested quantity.
- If not, use this strict fallback chain: babka -> rugelach -> sourdough -> challah.
- You can offer up to 2 alternative suggestions max. The fallback must satisfy the ENTIRE requested quantity.
- If a fallback succeeds: update the 'item' to the new product, customer pays the new product's price.
- If 2 suggestions fail or inventory is insufficient: status="rejected", message="Apologies, order cancelled due to lack of inventory.", append to 'steps', end graph.
- If succeeded: append to 'steps', continue to Delivery.

Node 3: Delivery
- If city is "Jerusalem": delivery="delivery", delivery_fee = 20.
- If city is outside Jerusalem: delivery="pickup", delivery_fee = 0, add a note to the message that it was switched to pickup.
- If city is empty: delivery="pickup", delivery_fee = 0.
- Append to 'steps', continue to Pricing.

Node 4: Pricing
- Calculate baked_goods_total = item_price * quantity.
- Apply a 15% discount ONLY to baked_goods_total IF baked_goods_total > 150 AND day != "Friday".
- total = discounted_baked_goods_total + delivery_fee.
- Append to 'steps', continue to Message.

Node 5: Message
- Generate a polite deterministic string summarizing the accepted order, the final item, delivery method, and total price. 
- status="accepted".
- Append to 'steps'.

5. GRAPH CONSTRUCTION & OUTPUT
- Compile the graph.
- Draw the graph in the notebook using `draw_mermaid_png()`.
- The graph must be invocable like this: result = graph.invoke({"item": "babka", "quantity": 2, "day": "Thursday", "city": "Jerusalem"})
