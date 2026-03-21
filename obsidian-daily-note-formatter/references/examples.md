# Examples

Use these patterns as anchors when the input is messy.

## Rapid daily note with mixed content
Input:
```text
Devotional thought>
• i read psalm 37 as hunny gave me the thought yesterday
• what caught my attention was 37:4 and 5
• commit your ways the the Lord, Delight in Him
• God is first seek first His kingdom (Matt 6:33)
---
Food>
185grams salas/chicken for lunch for me
---
Hunny food>
115 grams salad/chicken for hunny lunch
---
Task TickTick>
• Reach out to Luis, to see how hes doing.
```

Assume note date is 2026-03-21.

Suggested output:
```markdown
# Note | Hunny | Devotional Reflection on Psalm 37
- [[Hunny]] mentioned [[Psalm 37]] yesterday.
- What stood out was Psalm 37:4-5.
- Commit your way to the Lord and delight in Him.
- God comes first; seek first His kingdom ([[Matthew 6:33]]).

# Note | Lunch Log
- 185 grams salad and chicken for lunch.

# Note | Hunny | Hunny Lunch Log
- [[Hunny]] lunch: 115 grams salad and chicken.

# Task | Luis | Check in with Luis
- [ ] Reach out to [[Luis]] to see how he's doing. 📅 2026-03-22
```

## Free-form note without hard delimiters
Input:
```text
meeting with ryan about website copy need home page clearer maybe ask sales for examples todo followup tomorrow
```

Suggested output:
```markdown
# Meeting | Ryan | Website Copy Review
- Met with [[Ryan]] about website copy.
- The home page needs clearer messaging.
- Ask sales for example language.

# Todo | Website Copy Follow-up
- [ ] Follow up on website copy. 📅 <next-day-date>
```
