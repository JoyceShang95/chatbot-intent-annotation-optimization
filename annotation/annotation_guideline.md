# Overseas Customer-Service Chatbot — Annotation Guideline

## 1. Purpose
Classify each customer message into the **primary user intent** that should determine the next customer-service action.

## 2. Intent taxonomy
| Intent | Definition | Positive cues | Boundary rule |
|---|---|---|---|
| order_tracking | User asks where/how to track shipment or current shipment status | track, tracking link, where is my package, shipped yet | If an explicit overdue/late signal exists, use delivery_delay |
| delivery_delay | Delivery is later than the promised/expected time | late, overdue, past ETA, should have arrived, taking too long | Delay status dominates generic tracking wording |
| address_change | User wants to modify delivery destination | wrong address, change address, different address | Intent is destination change, even when order is already placed |
| order_cancellation | User wants to stop/cancel an order | cancel, stop order, changed my mind | Cancellation intent dominates shipment state |
| refund_status | User asks about money already expected back | refund, money back, refund processed, when will I get it | Focus is status/timing of returned funds |
| return_request | User wants to send an item back or asks about return procedure | return, send back, return label | If the item was already returned and user asks about money, use refund_status |
| payment_failed | Payment/checkout attempt did not complete | declined, failed, did not go through | Pending/double charge is not automatically duplicate_charge; read the whole message |
| duplicate_charge | User believes they were charged more than once | twice, two charges, paid twice | Needs evidence of duplicate billing, not just a pending authorization |
| wrong_item | User received a different/incorrect item or variation | wrong item, different product, wrong size | Size/variation errors are still wrong_item unless the business taxonomy says otherwise |
| damaged_item | Product arrived physically damaged | broken, cracked, damaged | Damage is the primary intent even if delivery was also delayed |

## 3. Priority rule for ambiguous messages
When multiple signals appear, annotate the intent that best maps to the **next action required**.

Example:
- “Where can I track my package?” → `order_tracking`
- “Where is my package? It should have arrived yesterday.” → `delivery_delay`
- “Can I cancel after it was shipped?” → `order_cancellation`
- “I returned it last week. When will I get my money back?” → `refund_status`

## 4. Low-confidence protocol
Flag a sample when:
- two intents are genuinely plausible;
- key context is missing;
- confidence is below 0.70;
- the message contains a new scenario not covered by the taxonomy.

## 5. Quality-control checklist
Before submission, ask:
1. What action does the customer actually want?
2. Is there a time/exception signal that changes the intent?
3. Is the message asking for a process, a status, or a complaint resolution?
4. Would another annotator likely choose a different label?
5. Does the taxonomy need a new example or a boundary rule?
