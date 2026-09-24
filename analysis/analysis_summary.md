# Error Analysis Summary

## Snapshot
- Samples: 50
- Correct model predictions: 37
- Accuracy: 74.0%
- Low-confidence samples (<0.70): 13
- Dominant error pattern: `order_tracking` vs `delivery_delay`

## Main confusion clusters
### 1. order_tracking ↔ delivery_delay
The model tends to overweight words such as **track / package / where is** and underweight temporal evidence such as **yesterday / late / overdue / past ETA**.

Recommended action:
- Add more hard-negative pairs.
- Add a temporal-signal rule to the annotation guideline.
- Add training examples where a tracking verb appears together with an overdue signal.

### 2. return_request ↔ refund_status
The model sometimes sees **return** and stops before reading the user's actual requested outcome.

Recommended action:
- Use action-outcome labeling: “send back” → return; “when will I get money back?” → refund status.
- Add post-return refund examples.

### 3. payment_failed ↔ duplicate_charge
A payment failure can coexist with a pending/duplicate-looking authorization.

Recommended action:
- Add examples distinguishing failed payment from confirmed duplicate billing.
- Include “pending charge” as a separate cue requiring context, not an automatic duplicate-charge label.

### 4. wrong_item ↔ return_request
Users may describe an incorrect variation and then ask about returning it.

Recommended action:
- Decide whether the business wants the **root issue** (`wrong_item`) or the **requested action** (`return_request`) and document it consistently.
- For this demo, root issue = `wrong_item` when the customer specifically received the wrong product/variation.

## Suggested monitoring metrics
- Intent accuracy
- Per-intent precision/recall
- Confusion matrix
- Low-confidence rate
- Annotator agreement
- New/uncovered scenario rate
- Knowledge-base gap count
