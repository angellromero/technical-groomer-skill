# QA Notes Guide

This reference provides detailed guidance on generating effective QA notes. Load when creating QA notes from diffs or refining draft QA notes.

---

## QA Notes Purpose

QA notes serve two audiences:

| Audience | What They Need |
|----------|----------------|
| **QA Engineer** | Clear scenarios to test, expected outcomes, setup instructions |
| **Developer** | What to manually verify before marking complete |

Good QA notes are **testable** — each scenario has clear steps and expected outcomes.

---

## Anatomy of Effective QA Notes

### Pre-conditions

What must be true before testing begins.

**Good Pre-conditions:**
```
- User logged in with role: Admin
- At least 3 products exist in database
- Feature flag `NEW_CHECKOUT` enabled
- Test environment: Staging
```

**Bad Pre-conditions:**
```
- System should be working
- User exists
```

**Ask yourself:** Could a QA engineer start testing with only these instructions?

---

### Test Scenarios

Each scenario needs three elements:

| Element | Description | Example |
|---------|-------------|---------|
| **Context** | What situation is being tested | "User with items in cart" |
| **Steps** | Exact actions to perform | "Click checkout → Enter address → Submit" |
| **Expected** | Specific outcome to verify | "Order confirmation displays with order ID" |

**Good Scenario:**
```
**Add item to cart:**
- Context: User viewing product detail page
- Steps: 
  1. Select size "Medium"
  2. Click "Add to Cart"
  3. Observe cart icon
- Expected: 
  - Cart icon shows count "1"
  - Toast notification "Added to cart"
  - Product appears in cart drawer
```

**Bad Scenario:**
```
**Add item to cart:**
- Test adding items
- Should work
```

---

### Edge Cases

Edge cases are scenarios at the boundaries of expected behavior.

**Common Edge Case Categories:**

| Category | Examples |
|----------|----------|
| **Empty States** | No items, no results, no history |
| **Boundary Values** | Max length, zero, negative, very large numbers |
| **Invalid Input** | Wrong format, missing required fields, special characters |
| **Permission Boundaries** | User lacks access, session expired, role changed |
| **Timing Issues** | Double-click, slow network, concurrent updates |
| **State Transitions** | Cancel mid-flow, back button, refresh page |

**Format:**
```
**Edge Case: Empty cart checkout attempt**
- Steps: With empty cart, navigate to /checkout
- Expected: Redirect to cart page with message "Your cart is empty"

**Edge Case: Quantity exceeds stock**
- Steps: Set quantity to 999 when only 5 in stock
- Expected: Quantity auto-corrects to 5, warning displayed
```

---

### Error States

What happens when things go wrong.

**Common Error Categories:**

| Category | Scenarios |
|----------|-----------|
| **Network Errors** | API timeout, connection lost, server error |
| **Validation Errors** | Invalid input, missing fields, format errors |
| **Business Logic Errors** | Insufficient funds, item unavailable, limit exceeded |
| **Authentication Errors** | Session expired, invalid token, unauthorized |

**Format:**
```
**Error State: Payment API timeout**
- Trigger: Payment takes >30s (simulate with network throttling)
- Expected: 
  - Loading state shows during wait
  - After timeout: "Payment processing delayed. Check order status."
  - Order status shows "Pending"
  - No duplicate charges
```

---

### Regression Areas

What else might break because of this change.

**How to Identify Regression Areas:**

1. **Shared Components:** What other features use the modified component?
2. **Shared Data:** What other features read/write the same data?
3. **Integration Points:** What external systems are affected?
4. **User Flows:** What flows pass through the changed code?

**Format:**
```
### Regression Areas

| Area | Why Check | Quick Validation |
|------|-----------|------------------|
| Product listing page | Shares ProductCard component | Verify cards render correctly |
| Order history | Reads from same order data | Verify historical orders display |
| Email notifications | Triggered by order creation | Verify email sends with correct data |
```

---

## Generating QA Notes from Diff

### Step 1: Categorize Changes

Review the diff and categorize:

| Category | What to Look For |
|----------|------------------|
| **New Functionality** | New components, new API endpoints, new features |
| **Modified Behavior** | Changed logic, updated validation, altered flows |
| **Bug Fixes** | Corrected behavior, edge case handling |
| **Refactoring** | Structure changes that shouldn't affect behavior |

### Step 2: Map to Test Scenarios

| Change Type | Test Focus |
|-------------|------------|
| New functionality | Happy path + edge cases + errors |
| Modified behavior | Verify new behavior + regression on old |
| Bug fixes | Verify fix + original bug no longer reproducible |
| Refactoring | Regression only — behavior should be unchanged |

### Step 3: Identify What NOT to Test

Equally important — what's out of scope:

```
### Not In Scope

- Styling changes (none in this PR)
- Admin functionality (not touched)
- Mobile-specific behavior (desktop only change)
```

This prevents QA from wasting time on unaffected areas.

---

## QA Notes Quality Checklist

Before sharing QA notes, verify:

### Completeness
- [ ] All new functionality has test scenarios
- [ ] Edge cases identified for each feature
- [ ] Error states documented
- [ ] Regression areas listed

### Clarity
- [ ] Each scenario has explicit steps
- [ ] Expected outcomes are specific and verifiable
- [ ] Pre-conditions are sufficient to start testing
- [ ] Technical jargon explained or avoided

### Accuracy
- [ ] Scenarios match actual implementation (not just plan)
- [ ] Expected outcomes reflect real behavior
- [ ] Pre-conditions are actually required
- [ ] Regression areas are genuinely affected

### Testability
- [ ] Scenarios can be executed independently
- [ ] Expected outcomes are observable
- [ ] Edge cases can be triggered
- [ ] Error states can be simulated

---

## Common Mistakes

### Mistake 1: Vague Expected Outcomes

**Bad:** "Page should load correctly"
**Good:** "Page displays product grid with 12 items, filter sidebar, and sort dropdown"

### Mistake 2: Missing Setup Steps

**Bad:** "Test checkout flow"
**Good:** "With 2 items in cart totaling $50, logged in as standard user, click Checkout"

### Mistake 3: Untestable Scenarios

**Bad:** "Verify performance is good"
**Good:** "Page load completes in <2s on 3G throttling (Chrome DevTools)"

### Mistake 4: Missing Negative Cases

Only testing happy path. Always include:
- What happens with invalid input?
- What happens when external services fail?
- What happens at boundary values?

### Mistake 5: QA Notes from Plan, Not Implementation

Draft QA notes from grooming are estimates. Always regenerate or refine from actual diff before sharing with QA.

---

## Template: Minimal QA Notes

For small changes:

```markdown
## QA Notes: [Brief Description]

**Change:** [One sentence summary]

**Test:**
1. [Primary scenario] → Expected: [Outcome]

**Edge Case:**
1. [Key edge case] → Expected: [Handling]

**Regression:** [One area to spot-check]
```

---

## Template: Comprehensive QA Notes

For large features:

```markdown
## QA Notes: [Feature Name]

**Ticket:** [ID]
**Summary:** [What was implemented]

---

### Pre-conditions
- [Setup requirement 1]
- [Setup requirement 2]

---

### Test Scenarios

#### Happy Path
1. **[Scenario Name]:**
   - Context: [Situation]
   - Steps: [Numbered steps]
   - Expected: [Specific outcomes]

[Additional scenarios...]

#### Edge Cases
[Numbered edge case scenarios...]

#### Error States
[Numbered error scenarios...]

---

### Regression Areas

| Area | Why | Validation |
|------|-----|------------|
| [Area] | [Reason] | [How to check] |

---

### Not In Scope
- [Explicitly excluded items]

---

### Notes for QA
- [Any additional context]
- [Known limitations]
- [Environment-specific instructions]
```
