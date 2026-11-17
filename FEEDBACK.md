# Client review: Larkspur disruption-care agent

**To:** Room 19  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-17 09:30

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md leaves Number, Safety check, Still broken, Costs, Wrong, Runs it, and Left out as blank templates.**

The line reads "Number: Token usage changed from .. to .. across five tickets" with the placeholders still unfilled, and "Safety check:" and "Still broken: observed issue ---" carry no content. The four question fields under "Priya asked" (Costs, Wrong, Runs it, Left out) are empty.

Fill PITCH.md with the actual before/after token numbers from a bench.py run and paste the completed file.

**2. search_alternatives in agent.py grew from a 6-character description to 696 characters in this pod's diff.**

The diff replaces the placeholder string "search" with a 696-character description covering when to call it, what it returns (option_id, fares, delay deltas), and sequencing with hold_seat. This is the only tool description this team substantially rewrote; get_flight_status only gained a date-format clarification.

Run python3 run.py --tool-tax and check whether the added description length changed input token cost per call.

**3. run_agent's loop still discards no state, but the only committed trace shows 3 tool calls, not the 8-call cap.**

readout-trace.json records 4 API turns and tool calls lookup_booking, get_flight_status, check_policy in order, never reaching MAX_TOOL_CALLS = 8. There is no trace showing what happens when a booking needs search_alternatives, hold_seat, or confirm_rebooking, so the fix in this diff (passing response.content instead of text_of(response) into messages) is untested against a multi-option rebooking path.

Run python3 run.py --all --trace and paste the per-PNR tool sequences to confirm the fixed message history survives a rebooking flow.

**4. TONE_ADDENDUM is 0 characters and EXTRA_TOOLS is empty; no model swap changes what the agent is told to say or do.**

The static scan confirms TONE_ADDENDUM still empty (0) characters and EXTRA_TOOLS declared: 0, with no LOCAL_TOOLS executors. Whatever model answers these calls, it answers using the same nine stock tool schemas and an empty tone layer, so any behavior gap traced to tone or missing tools is not a model-capability question.

Run python3 verify.py 4.1 to check whether the intelligence-goal gate depends on TONE_ADDENDUM content that does not yet exist.

**5. readout-trace.json shows 0 cache tokens read or written against 13608 input tokens on a single run.**

The trace reports tokens: 13608 in, 1104 out and prompt caching: 0 read, 0 written, hit ratio None, with no cache_control found anywhere in agent.py by the static scan. A system prompt plus nine tool schemas repeated on every one of the 4 API turns is exactly the kind of repeated prefix caching exists for, and none of it is being captured here regardless of which model is called.

Run python3 bench.py --compare baseline cached after adding cache_control to confirm what fraction of the 13608 input tokens is cacheable.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (236 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-latest`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
