# Optimising for Generative Engine Optimization (GEO) — July 2026

*A practical report on current best practice, content and technical, for optimising a website to be discovered, cited, and recommended by AI search systems (ChatGPT, Google AI Overviews, Perplexity, Claude, Gemini, Copilot).*

---

## 1. The most important thing to understand first

There are currently two quite different bodies of opinion on GEO, and they don't fully agree. Worth being clear-eyed about both before acting on either.

**Google's own official position** (Google Search Central, last updated 29 June 2026, i.e. this month) states plainly that optimising for generative AI search is "still SEO," rooted in the same ranking and quality systems as ordinary search. Google explicitly tells site owners to **ignore**:
- Creating `llms.txt` files or other special AI markup ("Google Search itself doesn't use them")
- "Chunking" content into small pieces for AI ("no requirement to break your content into tiny pieces")
- Rewriting content specifically for AI systems rather than people
- Seeking inauthentic third-party "mentions"
- Overfocusing on structured data ("not required for generative AI search")

**The GEO industry** (SEO agencies, GEO-specific tooling vendors, and a genuine academic research paper) makes considerably stronger and more specific claims: that definition-first openings roughly double citation rates, that statistics with named sources lift citation by around 40%, that schema markup increases visibility by 30% or more, and so on. Some of this traces back to a legitimate 2024 Princeton/Georgia Tech/IIT Delhi study (presented at KDD 2024) that did find these effects in controlled tests. A lot of the rest is vendor marketing repeating those figures, or inventing similar-sounding ones, to sell GEO audit tools and agency retainers.

**The practical reading:** Google's position and the academic research aren't actually in conflict as much as they first appear. Google is saying these tactics aren't *necessary* and won't move the needle on Google's own systems specifically. The academic research is about how *other* platforms (ChatGPT, Perplexity, Claude) select and extract passages for citation, which is a different mechanism to Google's ranking system. Both can be true at once: you don't need to do GEO tricks to rank well on Google, but if you want to be the source ChatGPT or Perplexity quotes, structuring content the way their retrieval systems prefer does help, based on real (if narrow) research. Treat vendor statistics with real scepticism regardless of which side they're on.

---

## 2. Content best practice

### High confidence (backed by Google's official guidance and/or the Princeton/KDD research)

- **Write genuinely non-commodity content.** Google's strongest, most repeated point: content that restates common knowledge ("7 tips for first-time homebuyers") is explicitly deprioritised in favour of content with a real point of view or first-hand experience. This matters more than any structural trick.
- **Definition-first openings help with AI-platform extraction.** The KDD 2024 research found content whose opening sentence directly answers the implied question, rather than building up to it narratively, is retrieved and cited meaningfully more often by LLM-based systems. This is a genuine, tested effect, separate from Google's own ranking.
- **Cite sources and use specific, verifiable statistics rather than vague claims.** The same research identified this as one of the strongest levers, alongside quotations and named sources. "Reduces claims by around 20%" beats "reduces claims significantly."
- **FAQ sections with direct Q&A pairs.** Consistently supported across both camps: FAQ-structured content mirrors the exact format LLMs produce, making it easy to extract and cite as a self-contained unit.
- **Content freshness matters, genuinely.** Both Google and the AI platforms weigh recency. A "last updated" date that's actually honest, and content that's actually revisited, is a real signal, not just decoration. Stale content loses ground to newer competitors covering the same topic.
- **Author attribution and expertise signals (E-E-A-T).** Named, credentialed authorship remains a real trust signal for both traditional SEO and AI citation, especially for YMYL (Your Money or Your Life) topics like law, health, and finance, where AI systems apply stricter scrutiny.

### Lower confidence / treat with scepticism

- Specific percentage lifts quoted by GEO vendor blogs (30%, 40%, 60%, "300% more accurate") should be read as directional at best. Several conflict with each other and cite each other in circular fashion rather than primary research.
- "Chunking" content into artificially short passages, per Google directly, isn't necessary and shouldn't override writing for a human reader first.
- Rewriting content in an unnatural, keyword-stuffed, or "AI-friendly" style is actively counterproductive; both Google and the platforms are explicitly trained to detect and deprioritise content that reads as written for a machine rather than a person.

---

## 3. Technical best practice

### AI crawler access (robots.txt)

This is the area with the most genuinely new, concrete technical detail in 2026. Major AI providers now run **separate bots for training versus search/citation**, and the distinction matters enormously:

| Provider | Training bot (feeds model weights) | Search/citation bot (powers live AI answers) |
|---|---|---|
| OpenAI | `GPTBot` | `OAI-SearchBot`, `ChatGPT-User` |
| Anthropic | `ClaudeBot` | `Claude-SearchBot`, `Claude-User` |
| Perplexity | — | `PerplexityBot`, `Perplexity-User` |
| Google | `Google-Extended` (opts out of Gemini training only, doesn't affect Google Search ranking) | Googlebot (unchanged) |
| Apple | `Applebot-Extended` | Applebot |

**Blocking the wrong bot is the single most common, highest-impact mistake.** Blocking `GPTBot` does not remove you from ChatGPT's live search results, that requires blocking `OAI-SearchBot` specifically, and vice versa. A defensible default for a business that wants AI visibility: allow all search/citation bots, decide separately (as a content-policy question, not an SEO one) whether to allow or block the pure training crawlers.

**A caveat worth taking seriously:** not every bot honours robots.txt. Cloudflare published evidence (August 2025) that Perplexity has run undeclared crawlers that rotate user-agents and IPs specifically to evade "do not crawl" directives. Bytespider (TikTok/ByteDance) has a similar documented history of ignoring robots.txt. For these, robots.txt is a polite request, not enforcement, actual blocking requires server or CDN/WAF-level rules. Also worth checking directly: CDN and firewall products (Cloudflare, Sucuri, etc.) sometimes silently override an intended robots.txt "allow" with a default AI-bot block at the infrastructure level. Worth verifying both layers agree.

### Schema / structured data

Google's official position is that structured data is **not required** for generative AI search specifically, though it remains part of good general SEO practice (rich results eligibility). Independent of Google's stance, there's a reasonably consistent, plausible mechanism (though vendor-inflated on exact numbers) for why schema helps with the *other* platforms: JSON-LD gives an AI crawler explicit, unambiguous facts rather than requiring inference from prose, which reduces the chance of misreading or hallucinating details about your business.

If implementing schema, in rough priority order: **Organization** (entity identity, `sameAs` links to verified profiles), **Article** (with real author and `dateModified`), **FAQPage** (only on genuine Q&A content, matching what's visibly on the page), **BreadcrumbList**, and **LocalBusiness** where relevant. A few practical failure modes worth avoiding specifically: schema describing content that isn't actually visible on the page (against Google's guidelines and a real trust risk), schema that's gone stale relative to the live page ("schema drift"), and self-referential review schema without genuine underlying reviews.

### Core Web Vitals / site performance

Genuinely still relevant, and if anything the bar has risen. As of 2026:
- **LCP** (loading): under 2.5 seconds
- **INP** (responsiveness, replaced FID in 2024): under 200ms, with the most competitive sites now targeting closer to 150ms
- **CLS** (visual stability): under 0.1

Google measures these using real-world Chrome user data (CrUX), not lab scores, at the 75th percentile, meaning your slowest quarter of visitors determines your result, not your average visitor. Google's own official generative-AI guidance explicitly lists "good page experience" as one of the foundational technical factors still worth prioritising. There's also a reasonably-sourced claim worth taking seriously (rather than as vendor hype): a slow site can be filtered out of AI Overview citation consideration even when its content is otherwise strong, since the AI grounding process still depends on Google's core retrieval and quality systems.

### llms.txt — the genuinely contested one

This deserves direct treatment since it's the most talked-about "GEO technical" tactic and the industry is split:

- **Google's official position:** ignore it. Google Search doesn't use it, full stop.
- **Independent, non-vendor technical analysis** (reviewed for this report) broadly agrees it isn't a working access-control or ranking mechanism for any major platform currently, and describes adoption as low (roughly 10% of sites that have implemented one), with no confirmed material effect on retrieval in 2026.
- **GEO vendors** are split, some recommend it as a low-cost hedge, others admit it's unproven but "can't hurt."

**Recommendation: low priority.** It costs very little to add and won't hurt anything, but it should not be mistaken for a genuine lever, and definitely shouldn't be prioritised ahead of the content and technical fundamentals above. If it's on the to-do list, it belongs near the bottom.

---

## 4. Specific considerations for a law firm site (YMYL content)

Legal content sits in Google's "Your Money or Your Life" category, meaning both Google's ranking systems and AI platforms apply more scrutiny to expertise and trust signals than they would for, say, a recipe blog. Practically, this means the E-E-A-T-related items above matter more here than average:

- Named, credentialed author bylines on substantive legal content (already standard on well-optimised competitor sites, and something this project's content already does)
- Genuine, verifiable regulatory facts (SRA number, real complaints procedure, real fee structure) function as trust signals for AI systems in the same way they do for a human reader
- Avoiding definitive, certainty-implying claims (a pattern already enforced throughout this project's content for SRA compliance reasons) likely also reads well to AI systems, which are documented to weigh hedged, appropriately-caveated claims as more trustworthy than absolute ones in sensitive categories
- A publicly documented AI crawler policy (a short, plain-English page describing which bots are allowed and why) is emerging practice among service businesses that want AI visibility on informational content while keeping certain data (e.g. case results, client-specific detail) out of training corpora

---

## 5. Practical checklist

**Content**
- [ ] Every substantive page has a genuine point of view or firm-specific detail, not just restated common knowledge
- [ ] Opening sentence of each page/section directly answers the implied question
- [ ] FAQ sections present on informational content, phrased as real questions
- [ ] Named author + honest "last updated" date on anything substantive
- [ ] Content revisited and refreshed on a real cadence, not just published once

**Technical**
- [ ] robots.txt explicitly addresses search/citation bots (`OAI-SearchBot`, `ChatGPT-User`, `Claude-SearchBot`, `Claude-User`, `PerplexityBot`) as allowed
- [ ] A considered, documented decision made on training bots (`GPTBot`, `ClaudeBot`, `Google-Extended`, `CCBot`) rather than a default block-everything policy
- [ ] CDN/WAF settings checked separately, confirmed not silently overriding robots.txt
- [ ] Organization + Article schema implemented site-wide; FAQPage added only where genuine Q&A content exists
- [ ] Schema audited periodically against live page content to catch drift
- [ ] LCP, INP, CLS measured via real field data (Search Console / CrUX), not just lab scores, on priority pages
- [ ] `llms.txt` treated as optional and low-priority, not a substitute for the above

---

## 6. Sources

- Google Search Central, *Optimizing your website for generative AI features on Google Search* (official documentation, last updated 29 June 2026) — developers.google.com/search/docs/fundamentals/ai-optimization-guide
- Search Engine Land, *Mastering generative engine optimization in 2026: Full guide* — searchengineland.com
- Digital Applied, *GEO Guide 2026* and *AI Crawler Access Control: The 2026 Decision Matrix* — digitalapplied.com
- No Hacks, *The AI User-Agent Landscape in 2026: A Complete Reference* — nohacks.co
- Contently, Soar Agency, Anagram, Verlua, Capston.ai, Pixis, Mersel AI, DataImpulse — various 2026 robots.txt / AI crawler technical guides, cross-referenced for consistency
- Discoverability Company, Averi, WPRiders, DerivateX, Walker Sands, LLM Pulse, AISOS — schema markup / structured data guides, cross-referenced; specific percentage claims treated as directional, not verified
- ALM Corp, Mewa Studio, White Label Coders, Sky SEO Digital, Technology Aloha, Digital ByteTeck, TechLooker, MorningRank — Core Web Vitals 2026 technical guides
- Princeton / Georgia Tech / IIT Delhi, *GEO: Generative Engine Optimization* (KDD 2024) — the primary academic source underlying most credible content-structure claims in this report

*Report compiled 10 July 2026. GEO is a fast-moving, commercially-driven field; treat vendor-published statistics with proportionate scepticism and re-check Google's official guidance periodically, as it is the only source here with no commercial incentive to overstate GEO's importance.*
