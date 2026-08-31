# How an LLM Generates Text

A companion activity for the *From Words to Worlds* flipbook. Watch a real GPT-2 model compute real next-token probabilities, live, for the context "The capital of France is ___," pick a token to continue the sentence, and watch the loop repeat.

## What this actually does

This isn't a simulation with made-up numbers. It loads a real GPT-2 model (full precision, not the smaller quantized version) using Transformers.js, runs an actual forward pass on whatever text is in the context box, and shows the real top-8 next-token probabilities computed by that pass. The node-and-line diagram above the results is a stylized illustration of information flowing through the network's layers, it isn't a literal picture of GPT-2's internal structure, but every number in the candidates list below it is real.

Click any candidate token to append it to the context and generate again. There's no built-in stopping point, the model doesn't know when it's "answered the question," so left running it will keep generating past the correct answer into whatever statistically follows, capped at 25 tokens so it can't run away indefinitely.

The temperature slider re-shapes the same real probability distribution instantly, no new model call needed, higher temperature flattens it toward more variety, lower sharpens it toward the single most likely token.

## Why this is its own repository

This demo downloads full-precision GPT-2 weights, roughly 250–500MB depending on connection, considerably larger than the other NLP activities in the main activities repository. Keeping it separate means nobody browsing the lighter demos gets stuck waiting on a download they didn't ask for.

## File

| File | What it does | Needs internet on first open? |
|---|---|---|
| `index.html` | The full activity | **Yes**, first load only, and it's a large download |

After the first load, the browser caches the model and it works offline from then on, same device, same browser.

## A known limitation worth knowing

Transformers.js defaults to loading an 8-bit quantized version of a model, and testing showed that quantized GPT-2 doesn't reliably rank "Paris" among the top candidates for this exact prompt, likely because a 124-million-parameter model doesn't have much redundancy to absorb that compression cleanly. This file explicitly requests full precision (`quantized: false`) instead, at the cost of a much larger download. If you ever see it get simple factual continuations wrong again, that default is the first thing to check.

## Testing locally

Opening `index.html` directly as a `file://` path will not work; most browsers block the model download for security reasons when a page isn't served over `http://` or `https://`. Serve the folder instead:

```
cd path/to/this/folder
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser. Leave the terminal window open while testing; closing it stops the server.

## Publishing to GitHub Pages

1. Create a new, empty GitHub repository.
2. Upload this file, renamed to `index.html`, to the repository root. No other files are needed.
3. In the repository's Settings → Pages, set the source to the main branch, root folder.
4. GitHub will publish the site at `https://[your-username].github.io/[repo-name]/`. Because the file is named `index.html`, that address loads it directly, no filename needed in the URL.
5. Update the placeholder link on the "How an LLM Generates Text" page of the flipbook with that address.

## AI disclosure

This activity and this README were developed with the assistance of Claude (Anthropic) and reviewed before use.
