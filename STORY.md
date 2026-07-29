# What I Built

I built a token cost and fit checker for multilingual support tickets. The checker evaluates text samples against five dials—special token handling, vocabulary fit, merge economy, how it splits, and edge case survival—to determine whether a given tokenizer is appropriate for our on-device assistant deployment.

The problem I was solving: our embedding table is capped and inference is billed per token. We needed a way to evaluate whether our tokenizer handles the language mix in our support queue—38% German, 22% Turkish, 19% English, and the remainder Thai, Arabic, and Mandarin.

## The Sample That Anchored It

I pinned two verbatim tickets from the queue:

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"

These German and Turkish samples became the calibration anchor for the entire checker.

## The Probe That Fooled It

The board results exposed gaps in my calibration. When I ran the probes, the results showed:

> I dont have it. I dont have it. I dont have it

This revealed that I had not completed the probe calibration work. The checker could not produce meaningful results because I had not supplied concrete probe samples with targeted dials and expected behaviors.

## The Fix

The vocabulary fit dial was identified as the weakest filter. My verdict was:

> The vocabulary accuracy metrics should be more than 90% accurate for it to be acceptable.

The advisor now listens to events from our Salesforce CRM for new entries, reads text files for language processing, and uploads results back to CRM. It refuses emojis and blacklisted words—that refusal boundary was added after the seeded runs showed drift.

## The Gate It Holds

The gate this checker must hold:

> I dont have it. I dont have it. I dont have it

This gate definition remains incomplete. Re-certification will require defining a concrete metric, threshold, and re-run trigger.

## Re-Certification Cadence

The checker re-runs against the probe board whenever the prompt or stance changes. The architecture review deadline was Thursday, and any changes to the vocabulary or tokenizer configuration trigger a fresh run.

## The Domain Lesson

I learned that building a token fit checker requires more than rating dials—it requires concrete probe samples with pasteable bytes, targeted dials, and expected behaviors. A category name or placeholder fails. The calibration record must include per-language lane counts and drift rulings, not assumptions. My split note observation—that I did not see any English reference even though the traffic claims 19% English—points to a gap that the checker should surface, not hide.

The checker carries my counting discipline. A stranger using it gets my calibration, not a generic rubric. That means incomplete calibration data produces incomplete results, and the provenance makes that visible.
