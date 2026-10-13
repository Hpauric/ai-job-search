# Search Queries for Job Scraper

<!-- Customized for: C#/.NET business-rules engineering, Ireland. Fill [YOUR_CITY] and the location filter. -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

Primary (Irish market):
- **irishjobs.ie** - Ireland's largest general job board
- **linkedin.com/jobs** - LinkedIn job listings (filter: Ireland / [YOUR_CITY]); also covered by the `linkedin-search` CLI with `--location "Ireland"` or `--location "Dublin, County Dublin, Ireland"`
- **jobs.ie** - another major Irish board
- **ie.indeed.com** - Indeed Ireland

The Danish portal CLIs shipped with the template are disabled (`enabled: false`). `linkedin-search` and `freehire-search` are country-agnostic and stay on. To get a real CLI for an Irish board instead of the WebSearch fallback, run `/add-portal`.

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies

## Query Categories

Queries are grouped by priority. All queries are in English. Combine each query with location terms (Ireland, Dublin, Cork, Galway, Limerick, or "remote Ireland") where the site supports it.

**Organize by function, not job title.** The same work is posted under many titles (".NET Developer", "C# Software Engineer", "Backend Engineer (.NET)"), so each category lists several title variants.

### Priority 1: C# / .NET backend software engineering

Your core skill set and the most common title family.

```
site:irishjobs.ie "C# Developer" Ireland
site:irishjobs.ie ".NET Developer" [YOUR_CITY]
site:jobs.ie "Software Engineer" C# .NET
site:ie.indeed.com "C# Software Engineer" Ireland
site:linkedin.com/jobs "Senior .NET Developer" Ireland
site:linkedin.com/jobs "Backend Engineer" C# Ireland
```

### Priority 2: Business rules / decision logic

Roles where rules and decision logic are the core work: your domain specialism.

```
site:irishjobs.ie "business rules" C# Ireland
site:linkedin.com/jobs "rules engine" .NET Ireland
site:linkedin.com/jobs "decision engine" C# Ireland
site:ie.indeed.com "business rules" developer Ireland
site:linkedin.com/jobs (InRule OR NRules OR "RulesEngine") Ireland
```

### Priority 3: Rules-heavy business domains (insurance, finance, payroll, compliance)

.NET roles in sectors where business rules dominate the codebase (rating, underwriting, claims, lending, pricing, payroll, regulatory onboarding).

```
site:linkedin.com/jobs ".NET Developer" insurance Ireland
site:irishjobs.ie "C#" fintech Dublin
site:linkedin.com/jobs "C# Developer" payments OR banking Ireland
site:irishjobs.ie ".NET" payroll OR compliance Ireland
```

### Priority 4: Broader / adjacent

Wider net: full-stack .NET, Azure, or technical-lead roles.

```
site:irishjobs.ie "Full Stack" .NET Ireland
site:linkedin.com/jobs "Azure" "C#" developer Ireland
site:linkedin.com/jobs ".NET Tech Lead" Ireland
site:linkedin.com/jobs "C#" remote Ireland
```

## Location Filter

When evaluating results, check that the job is within reasonable commute distance from your home, or is remote/hybrid within Ireland. Acceptable areas:
- [YOUR_CITY] and surrounding areas
- Remote, Ireland-based (check that "remote" doesn't mean "remote within another country")
- Hybrid with [N] office days/week in [ACCEPTABLE_AREA_1]
- [BORDERLINE_AREA] (borderline, ~X min by transit/car)
- Roles requiring relocation abroad (too far)

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
