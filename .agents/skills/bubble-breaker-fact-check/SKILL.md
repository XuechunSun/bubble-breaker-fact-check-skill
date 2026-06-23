---
name: bubble-breaker-fact-check
description: Fact-check Bubble Breaker content cards, classify evidence confidence, and rewrite claims into fun but honest Chinese card copy.
---

# Bubble Breaker Fact Check Skill

Use this skill whenever the user asks to verify, fact-check, clean, review, or rewrite Bubble Breaker cards, especially cards about history, science, animals, psychology, culture, internet myths, or surprising facts.

## Goal

Turn a potentially viral sentence into a card that is:

1. interesting enough for Bubble Breaker,
2. honest about uncertainty,
3. not overclaimed,
4. backed by sources,
5. ready to store in a database or content table.

## Core rule

Do not treat every card as needing to be a hard, confirmed fact. Bubble Breaker may include anecdotes, legends, debated claims, and strange historical stories, but the wording must clearly signal the confidence level.

Use wording such as:

- confirmed: “研究显示 / 官方记录显示 / 考古证据表明”
- well_attested: “多份资料记载 / 较多资料支持”
- anecdotal: “据回忆录记载 / 一则轶事称 / 传说”
- disputed: “这个说法有争议 / 目前没有定论”
- likely_false: “这更像是误传 / 没有找到可靠证据支持”

## Confidence labels

Assign exactly one confidence label:

- `confirmed`: Strong evidence from primary sources, official records, peer-reviewed papers, or multiple highly reliable sources.
- `well_attested`: Strong secondary-source support, but not as definitive as confirmed.
- `anecdotal`: Based on memoirs, oral history, later retellings, single-source stories, or plausible but not fully verifiable accounts.
- `disputed`: Credible sources disagree, or the claim is partially true but commonly exaggerated.
- `likely_false`: The claim is unsupported, contradicted by reliable sources, or appears to be a common myth.

## Source quality labels

Assign one source quality label:

- `high`: primary sources, official institutions, peer-reviewed journals, university presses, reputable encyclopedias, museums, government agencies.
- `medium`: reputable magazines, major news outlets, serious educational sites, cited history/science blogs.
- `low`: listicles without citations, social media posts, short videos, SEO reposts, content farms.

## Workflow

For each input card:

1. Extract the original sentence.
2. Split it into atomic fact claims.
3. Search for sources or inspect provided sources.
4. Check each claim separately.
5. Identify the weakest claim. The final confidence label should not be stronger than the weakest important claim.
6. Look for common exaggerations, incorrect dates, wrong names, or context loss.
7. Decide whether the card should be kept, rewritten, marked for review, or rejected.
8. Rewrite the card in Chinese so it stays fun but does not overclaim.
9. Provide source notes.
10. Return the result in the required output format.

## Required output format

Return the answer in this structure:

```md
## Fact-check result

**Original card**
> {original_card}

**Verdict**
{keep / rewrite / review / reject}

**Confidence**
{confirmed / well_attested / anecdotal / disputed / likely_false}

**Source quality**
{high / medium / low}

### Claim breakdown

| # | Claim | Assessment | Notes |
|---|---|---|---|
| 1 | ... | supported / partially supported / unsupported / unclear | ... |

### Main issue

{Explain the biggest accuracy risk in 1-3 sentences.}

### Recommended Bubble Breaker version

> {fun but accurate Chinese rewrite}

### Optional short version

> {one-sentence compact card version}

### Source notes

1. {source name}: {why useful / limitation}
2. {source name}: {why useful / limitation}
3. {source name}: {why useful / limitation}

### Database fields

```json
{
  "verification_status": "keep|rewrite|review|reject",
  "confidence_level": "confirmed|well_attested|anecdotal|disputed|likely_false",
  "source_quality": "high|medium|low",
  "needs_human_review": true,
  "recommended_wording": "...",
  "accuracy_note": "..."
}
```
```

## Rewrite style for Bubble Breaker

Good card copy should be:

- short,
- surprising,
- conversational,
- honest about uncertainty,
- suitable for Xiaohongshu-style sharing,
- not clickbait if the evidence is weak.

Prefer:

> “传说拿破仑有次兔子狩猎翻车：兔子没逃跑，反而冲向他，因为它们可能是家养兔，以为人类来送饭。”

Avoid:

> “1807年，拿破仑被兔子军团袭击，这是历史上真实发生的事。”

## Rejection rules

Reject or mark `likely_false` when:

- no credible source can be found,
- all sources copy each other without origin,
- the claim depends on a fake quote,
- the specific number/date/name is unsupported,
- the story is framed as confirmed but only exists as a meme.

## Batch mode

When given a CSV, Markdown table, JSON array, or list of cards:

1. Process each card independently.
2. Add the database fields for each card.
3. Sort the output into:
   - ready to publish,
   - rewrite needed,
   - human review needed,
   - reject.
4. For large batches, start with the riskiest cards: history, medicine, law, finance, psychology, science, and named-person quotes.

## Human review triggers

Always mark `needs_human_review: true` for:

- medical or health advice,
- legal or tax advice,
- investment or financial claims,
- claims about living people,
- politically sensitive claims,
- claims likely to trigger misinformation concerns,
- claims whose sources are not accessible.
