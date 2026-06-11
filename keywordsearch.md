# Academic Search Algorithm & Query Crafting for Research Databases

A systematic, no-code approach to building precise, high-recall queries across Google Scholar, arXiv, Semantic Scholar, PubMed, CORE, and more.  
Use this to replicate your search, adjust scope, and ensure comprehensive literature discovery.

---

## 1. General Search Strategy (Platform‑Agnostic)

Follow these steps regardless of the database you choose.

### Step 1: Deconstruct the Research Question
Break it into **core concepts** (usually 2–4 nouns or phrases).  
*Example:* “How does federated learning improve privacy in healthcare IoT?”  
→ Concepts: **federated learning**, **privacy**, **healthcare IoT**

### Step 2: Expand Each Concept with Synonyms
For each concept, list:
- Exact phrase: `"federated learning"`
- Acronyms: `FL`
- Variant spellings: `federated machine learning`
- Broader/narrower terms: `distributed learning`, `collaborative learning`

Avoid bias by scanning known papers for terminology used in titles/abstracts.

### Step 3: Choose Operators Based on the Database
- **Boolean**: `AND`, `OR`, `NOT` (some databases use implicit `AND` or `+`/`-`)
- **Field codes**: title (`ti`, `intitle`), abstract (`abs`), author, journal, year
- **Phrases**: `"..."` to lock word order
- **Wildcards/truncation**: `*` (if supported) to capture variations (e.g., `therap*` for therapy, therapies, therapeutic)
- **Grouping**: parentheses `( )` to nest ORs

*Database‑specific syntax tables are provided below.*

### Step 4: Build the Query Structure
Combine concepts with `AND`, and synonyms within a concept with `OR`.  
Use parentheses to isolate OR groups.  
*Template:*  
`(concept1_syn1 OR concept1_syn2) AND (concept2_syn1 OR concept2_syn2) AND ...`

Add **exclusions** (`NOT` or `-`) to filter noise, e.g., `-review` to remove review articles if you want primary research.

**Example:**  
`("federated learning" OR FL) AND (privacy OR security) AND (healthcare OR IoMT OR "Internet of Medical Things")`

### Step 5: Execute and Iteratively Refine
- **Too few results** (<20):  
  - Remove field restrictions (search all fields instead of just title).  
  - Use broader synonyms.  
  - Drop less critical concepts (reduce ANDs).
- **Too many results** (>500):  
  - Restrict to title/abstract.  
  - Add date filter.  
  - Use more specific terms.  
  - Require exact phrase for key concepts.
- **Scan top 20–30 hits** for new keywords, then reformulate.

### Step 6: Validate and Saturate
- Take a known relevant paper and ensure your query retrieves it. If not, adjust.
- **Forward/backward citation tracking**:  
  - Backward: check the paper’s references.  
  - Forward: see who cited it (Google Scholar “Cited by”, Semantic Scholar “Citations”).
- When no new relevant papers appear after multiple iterations, stop.

---

## 2. Google Scholar

A free, interdisciplinary search engine. It lacks a full query language but supports powerful, simple operators.

### Available Search Operators

| Operator | Example | Effect |
|----------|---------|--------|
| `"..."` | `"deep reinforcement learning"` | Exact phrase |
| `+` (or just space) | `+neural +network` | Required term (AND) |
| `-` | `machine learning -supervised` | Exclude term (NOT) |
| `OR` / `|` | `deep learning OR neural network` | Match any (OR) – must be uppercase |
| `intitle:` | `intitle:transformer` | Word appears anywhere in the title |
| `allintitle:` | `allintitle:attention is all you need` | All words must appear in the title (phrase‑like) |
| `author:` | `author:"Y LeCun"` | Author name (use quotes for exact) |
| `source:` | `source:"Nature"` | Publication name |
| `site:` | `site:arxiv.org` | Limit to a specific website (useful to cross‑check preprints) |
| `..` | *(via Advanced Search sidebar)* | Year range (e.g., 2020..2024) |

**Important**: Google Scholar interprets a space as `AND`. Parentheses are **not** supported; use multiple `OR` terms and rely on the order of operations. The `-` exclusion works like `NOT`.

### Step‑by‑Step Example

*Goal: Graph neural networks for molecular property prediction (2020–2025), excluding patents.*

1. Enter in the search bar:  
   `allintitle:"graph neural network" "molecular property" -patent`
2. Click **Custom range** in the left sidebar, set **2020–2025**.
3. Scan results. If too few, remove `allintitle:` and use:  
   `"graph neural network" molecular property -patent`
4. Use the **“Cited by”** link under any relevant paper to expand.

### Additional Google Scholar Tips
- There is **no wildcard** (`*`); list variations explicitly with `OR`.
- To search within a specific journal: `source:"Journal of Machine Learning Research"`
- The **Advanced Search** form (☰ → Advanced search) gives fields for author, journal, and date without manual operators.
- Because Scholar includes books, patents, and grey literature, use `-patent` if you only want articles.

---

## 3. arXiv

A preprint server for physics, mathematics, computer science, and related fields. It has a structured search syntax both for the web interface and API.

### Search Syntax (Web & API)

| Field Code | Example | Explanation |
|------------|---------|-------------|
| `ti:` | `ti:transformers` | Word in title |
| `abs:` | `abs:deep learning` | Word in abstract |
| `au:` | `au:bengio` | Author last name |
| `cat:` | `cat:cs.AI` | arXiv category (e.g., cs.LG, stat.ML) |
| `all:` | `all:federated learning` | All fields (default) |
| `jr:` | `jr:J. Mach. Learn. Res.` | Journal reference |
| `id:` | `id:2312.12345` | Specific arXiv ID |
| `"..."` | `"neural radiance field"` | Exact phrase |
| `*` | `optimiz*` | Prefix wildcard (matches optimize, optimization, etc.) |
| `AND` | `cat:cs.LG AND ti:diffusion` | Both must match |
| `OR` | `cat:cs.CV OR eess.IV` | Either matches |
| `ANDNOT` / `-` | `ti:transformer ANDNOT cat:cs.CL` | Exclude category; `-` works inside a field: `-review` |
| `(...)` | `(cat:cs.LG OR stat.ML) AND ti:"deep learning"` | Grouping |
| `+` | `+graph` (within a field) | Required term |

**Date range**: Use the **Advanced Search** web form, or add `AND submittedDate:[202001010000 TO 202412312359]` directly in the query (requires exact timestamp format).  
For simple year filtering, the advanced UI provides a date‑picker.

### Building a Query (Example)

*Goal: Large language model privacy in computational linguistics, 2022–2024.*

1. Break into concepts: LLMs, privacy, CL category.
2. Construct:  
   `all:large language model AND abs:privacy AND cat:cs.CL`
3. On the [Advanced Search page](https://arxiv.org/search/advanced):  
   - **All fields**: `large language model`  
   - **Abstract**: `privacy`  
   - **Subject**: `Computer Science - Computation and Language`  
   - **Date**: January 2022 – December 2024
4. If too few: replace `AND abs:privacy` with `AND all:privacy` or use `(privacy OR security)`.

### Tips
- Use `cat:` to narrow by subfield – find codes [here](https://arxiv.org/category_taxonomy).
- The wildcard `*` works **only as a prefix**; no infix or suffix wildcard.
- arXiv interprets multiple words as AND unless you group with `OR` or wrap in quotes.

---

## 4. Semantic Scholar

A free, AI‑powered academic search engine that provides extensive metadata and citation graphs.

### Search Operators (Web & API)

The web search box supports simple operators:
- `+term` – must include term
- `-term` – exclude term
- `"exact phrase"`
- `title:"..."` – restrict exact phrase to title

For more control, use the **Refine Results** sidebar (year, field of study, publication type, etc.).

**API** (for programmatic use, but no code here): parameter `query`, `year`, `fieldsOfStudy` can be combined. Boolean logic is limited; often require multiple queries.

### Step‑by‑Step Search

*Goal: Graph neural networks for drug discovery, 2020‑2024.*

1. Go to [semanticscholar.org](https://www.semanticscholar.org/).
2. In the search bar type:  
   `"graph neural network" drug discovery`
3. Use the sidebar to set **Year** 2020‑2024, **Fields of Study** → Medicine, Computer Science.
4. For more precision, try:  
   `title:"graph neural network" +drug +discovery`
5. Click on a relevant paper → use the **Citations** tab to find later works, and **References** tab for earlier ones.

### Tips
- Semantic Scholar’s *TLDR* feature gives a one‑sentence summary – use it for quick screening.
- Filter by **Venue** or **Author** using the left panel.
- Use the **Cite** button to export BibTeX.

---

## 5. PubMed (Biomedical & Life Sciences)

A free database with a powerful query parser that automatically maps to MeSH terms.

### Key Operators & Field Codes

| Syntax | Example | Meaning |
|--------|---------|---------|
| `[ti]` | `asthma[ti]` | Word in title |
| `[tiab]` | `children[tiab]` | Word in title or abstract |
| `[mh]` | `"Diabetes Mellitus"[mh]` | MeSH term (Medical Subject Heading) |
| `[au]` | `Smith J[au]` | Author |
| `[dp]` | `2020/01:2024/12[dp]` | Publication date range |
| `AND`, `OR`, `NOT` | `(covid[ti] OR sars-cov-2[ti]) AND vaccine[tiab]` | Boolean (uppercase) |
| `"..."` | `"acute kidney injury"` | Phrase |
| `*` | `therap*` | Truncation (unlimited characters after) |
| `(...)` | Used for grouping ORs |

### Crafting a PubMed Query

*Goal: Effects of exercise on depression in adolescents.*

1. Identify concepts: exercise, depression, adolescents.
2. Find synonyms/MeSH:  
   - Exercise: `"Physical Activity"[tiab] OR "Exercise"[mh]`  
   - Depression: `"Depressive Disorder"[mh] OR depression[tiab]`  
   - Adolescents: `adolescen*[tiab] OR "Adolescent"[mh]`
3. Combine:  
   `("Physical Activity"[tiab] OR "Exercise"[mh]) AND ("Depressive Disorder"[mh] OR depression[tiab]) AND (adolescen*[tiab] OR "Adolescent"[mh])`
4. Add date limit if needed: `AND 2018:2025[dp]`
5. Paste in the search box and execute.

### Tips
- Use the **Advanced Search Builder** to browse MeSH terms and field codes.
- The `[mh]` tag automatically includes narrower MeSH terms; to turn this off use `[mh:noexp]`.
- For systematic reviews, add the PubMed systematic review filter: `AND systematic[sb]`.

---

## 6. CORE (Open Access Aggregator)

[CORE](https://core.ac.uk/) aggregates millions of open‑access full texts.

### Search Syntax
- `AND`, `OR`, `NOT` (uppercase)
- `"phrase"`
- `*` wildcard (prefix)
- Field filters are available via the Advanced Search UI: `title`, `author`, `year`, `language`.
- Direct URL parameters can be used: `?q=title%3A"machine learning"&yearFrom=2020`.

### Example
*Open‑access papers on “climate change” and “migration” since 2018.*  
Advanced search:  
- **Title/Abstract**: `"climate change" AND migration`  
- **Year from**: 2018  
- Execute and browse.

---

## 7. Quick‑Reference Operator Crosswalk

| Feature | Google Scholar | arXiv | Semantic Scholar | PubMed |
|--------|:---:|:---:|:---:|:---:|
| Exact phrase | `"..."` | `"..."` | `"..."` | `"..."` |
| AND | (space) | `AND` | `+` (prefix) | `AND` |
| OR | `OR` or `\|` | `OR` | ❌ (merge queries) | `OR` |
| NOT / Exclude | `-` | `ANDNOT` or `-` | `-` | `NOT` |
| Title search | `intitle:` / `allintitle:` | `ti:` | `title:"..."` | `[ti]` |
| Abstract search | ❌ | `abs:` | ❌ | `[tiab]` |
| Author | `author:` | `au:` | Filter sidebar | `[au]` |
| Year range | Sidebar filter | `submittedDate` or UI | Sidebar | `[dp]` |
| Wildcard | ❌ | `*` (prefix only) | ❌ | `*` (suffix) |
| Grouping | ❌ | `(...)` | ❌ | `(...)` |

---

## 8. Final Workflow Checklist

- [ ] Research question broken into 2–4 core concepts.
- [ ] Synonyms, acronyms, and broader/narrower terms listed.
- [ ] Database‑appropriate operators and field codes applied.
- [ ] Query built with AND/OR/exclusions.
- [ ] Date range and other filters set (if needed).
- [ ] Test run – known paper appears?
- [ ] Refined based on result count and relevance.
- [ ] Forward/backward citation chasing performed.
- [ ] Search saved or documented for reproducibility.

---
