# Pipeline Logic

The exact specifications used to build the scoring and personalization columns in Clay.

## Lead Temperature (Formula column, rule-based, no AI credits)

Built using Clay's formula generator. Instruction given:

> If [Size] is 501-1,000 employees, output Hot. If [Size] is 201-500 employees, output Warm. Otherwise output Cold.

Deliberately rule-based rather than AI-driven: this signal (company size as a proxy for facility complexity and maintenance budget) doesn't need judgment, just a threshold, so a formula is faster, free, and fully explainable.

## Outreach Hook (Claygent / AI column)

Built using Clay's AI column generator. Instruction given:

> Using [Description], write one natural first-line email opener that mentions something specific from it, like what the company makes or a notable detail. No generic compliments, no "I hope this finds you well." If the description has nothing specific to reference, output exactly NO_SIGNAL instead of writing a generic line.

The explicit NO_SIGNAL fallback matters: it stops the system from inventing a personalized-sounding line when there's nothing real behind it. 1 of 16 rows correctly returned NO_SIGNAL rather than a fabricated opener.
