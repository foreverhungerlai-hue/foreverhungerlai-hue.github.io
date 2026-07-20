---
title: "LitScout: A Literature Radar Built Around Attention, Not Acquisition"
date: 2026-07-20T10:00:00+07:00
lastmod: 2026-07-20T10:00:00+07:00
draft: false
url: "/technical/litscout-attention-not-acquisition/"
description: "Why I designed a local literature radar to surface new research, respect paywalls, and preserve the human decision about what deserves deep reading."
summary: "LitScout is not a paper-scraping machine. It is a local research workflow that finds recent literature, filters noise, organizes high-value records in Zotero, and protects the researcher's most limited resource: time."
tags:
  - research-software
  - literature-discovery
  - knowledge-management
  - Zotero
  - Python
categories:
  - Research Systems
ShowToc: true
TocOpen: true
author: "Tataee"
technicalPage: true
---

<div class="technical-article-marker" aria-hidden="true"></div>

The usual fantasy behind a literature tool is simple: search everything, download everything, and place thousands of papers into a perfectly organized archive. I do not think that solves the real problem.

The limiting resource in research is rarely the number of PDFs on a hard drive. It is attention. A researcher still has to notice what is new, understand roughly what each paper contains, decide which papers deserve careful reading, and connect those papers to an actual experiment or argument. A machine can reduce the cost of discovery and organization, but it cannot make those reading decisions meaningful on my behalf.

That is the idea behind **LitScout**, a local literature radar built for my work on *Echinochloa*, herbicide resistance, P450/CYP81A genes, ALS inhibitors, quinclorac, and RT-qPCR.

**Project repository:** [foreverhungerlai-hue/LitScout-Echinochloa](https://github.com/foreverhungerlai-hue/LitScout-Echinochloa)

## The Problem Is Not “How Do I Download More Papers?”

New papers continue to appear while experiments, meetings, and writing consume the same fixed number of hours. Traditional saved searches help, but they often create another inbox. Broad automated searches create even more noise. A folder containing hundreds of unread PDFs may look productive while making the next reading decision harder.

LitScout was designed around a narrower question:

> How can I remain aware of new, relevant literature without turning literature collection into a second full-time job?

This changes the optimization target. LitScout does not try to maximize the number of records collected. It tries to minimize the time between publication and awareness while keeping the review queue small enough to be useful.

## The Workflow in One View

| Stage | What LitScout does | What remains a human decision |
|---|---|---|
| Discovery | Runs focused queries against Crossref and, optionally, OpenAlex | Whether the search strategy still represents the research question |
| Deduplication | Normalizes DOI values and removes duplicate candidates | Whether two distinct records are conceptually redundant |
| Triage | Applies transparent, weighted rules to titles, abstracts, subjects, and journal metadata | Whether a paper is truly useful in context |
| Organization | Adds high-relevance records to structured Zotero collections | How the paper contributes to an experiment, review, or argument |
| Access | Downloads a PDF only when Unpaywall identifies a legitimate open-access copy | How to obtain closed literature through appropriate channels |
| Reading | Presents titles, metadata, scores, and a manageable library | Which papers to skim, read broadly, or study in depth |

The important line is the last one. Automation ends before judgment begins.

## How LitScout Finds and Filters Literature

### 1. Focused discovery instead of a universal crawler

The search profile contains a small set of queries tied directly to the project: *Echinochloa* herbicide resistance, CYP81A genes, bispyribac-sodium, quinclorac, non-target-site resistance, and related RT-qPCR work. The look-back window and query list can be changed from the local dashboard.

Crossref provides the main stream of recent journal-article metadata. OpenAlex can contribute additional DOI candidates when an API key is available. Results from both sources are normalized around DOI, deduplicated, and capped per run. This is intentionally bounded. The system should finish, produce a reviewable result, and run again later—not expand endlessly because more records exist.

### 2. Explainable relevance scoring

LitScout does not use an opaque model to declare that a paper is “important.” Its classifier is a controlled set of regular-expression rules with visible weights. Signals include:

- the organism and individual *Echinochloa* species;
- herbicide resistance, including TSR and NTSR;
- cytochrome P450 and CYP81A genes;
- ALS/AHAS, bispyribac-sodium, and quinclorac;
- RT-qPCR, dose-response experiments, and named research materials.

The rules inspect the title, subtitle, abstract, subject fields, and journal metadata. A numerical score alone is not enough: automatic saving also requires a combination of core biological signals. For example, a paper mentioning a P450 somewhere is not automatically treated as relevant to the project.

High-relevance records can be written to Zotero automatically. Borderline records remain visible in preview reports, while low-scoring records are not saved merely to make the library look busy. The threshold is configurable because the cost of a false positive changes over the life of a project.

### 3. Zotero as the long-term memory

For records that pass the threshold, LitScout creates structured Zotero items with DOI, title, authors, abstract, journal metadata, rule-derived tags, and the LitScout score. It also places them into collections such as:

- recent automatic discoveries;
- *Echinochloa*;
- ALS and bispyribac-sodium;
- quinclorac;
- P450 and CYP81A;
- RT-qPCR methods;
- dose-response studies.

DOIs already present in the Zotero library are skipped. Each run also produces a CSV report, so a search can be audited without trusting a dashboard state or trying to remember what happened weeks earlier.

## Why LitScout Does Not Bypass Paywalls

It would be easy to describe paywalls as an obstacle that a “powerful” literature tool should route around. I deliberately rejected that goal.

LitScout asks Unpaywall whether a legitimate open-access PDF is available. If one exists, the file can be downloaded and attached to the Zotero record. If no open version is available, LitScout keeps the metadata and places the item in an **Awaiting PDF** collection. It does not attempt to defeat publisher access controls.

This is partly a legal and ethical boundary, but it is also a product decision. The purpose of the system is not to manufacture a complete local mirror of the literature. Its purpose is to tell me that a potentially important paper exists and give me enough information to decide whether obtaining it is worth the effort.

Once a paper has earned that attention, I can use an institutional subscription, look for an author manuscript, request it through a library, contact the authors, or use another legitimate route. A missing PDF is therefore a queue state, not a system failure.

## Titles Are Routing Signals, Not Substitutes for Reading

A title is often enough to form a rough map of a paper: organism, treatment, mechanism, method, and sometimes the central result. That makes titles extremely valuable for triage. It does not make them reliable replacements for the paper itself.

My practical reading funnel looks like this:

1. **Title scan:** Is the paper plausibly connected to the current question?
2. **Broad reading:** Check the abstract, figures, methods, and discussion for orientation.
3. **Deep reading:** Spend serious time only when the paper affects an experiment, interpretation, method, or argument.
4. **Return later:** Keep useful context searchable in Zotero even when it does not deserve immediate deep reading.

The same paper can move between levels as the project changes. A paper that looks peripheral today may become central when an experiment fails or a new mechanism appears. This is why LitScout organizes and resurfaces literature but does not pretend to make a permanent reading decision.

The goal is not to read less carefully. It is to spend careful reading where it has the highest value.

## A Local Dashboard, Not Another Cloud Inbox

LitScout runs on the local computer through a small Flask dashboard. The interface supports English, Chinese, and Thai, and it can:

- run a search immediately or preview without saving;
- schedule one automatic run per day;
- change the look-back window, score threshold, and queries;
- enable or disable open-access PDF attachment;
- import DOI lists from spreadsheet, text, or Markdown files;
- remove incorrectly matched items that were created by LitScout.

Zotero credentials and optional API keys remain in a local configuration file excluded from Git. The dashboard listens on localhost, and state-changing requests require a local token. This keeps the tool close to the research workflow without turning it into another account, hosted service, or remote database to maintain.

## Intentional Limitations

LitScout is not a systematic-review platform, a citation-network explorer, or an autonomous reader. Its rules can miss papers that use unexpected terminology, and metadata APIs do not always provide complete abstracts or consistent publication dates. Relevance weights need to evolve with the research question.

Those limitations are acceptable because the system is small enough to understand and modify. I can inspect why a record received a score, add a missing synonym, adjust a threshold, or change a collection rule without retraining a model or reverse-engineering a recommendation engine.

The tool is useful precisely because it does not claim to solve the whole literature problem.

## What I Was Actually Building

LitScout began as a literature-fetching utility, but the more important design emerged from deciding what it should refuse to do.

It should not collect papers to demonstrate activity. It should not bypass paywalls to maximize a download count. It should not hide relevance decisions inside an unexplained score. It should not make the final choice between broad reading and deep reading.

It should quietly check what is new, remove obvious duplication and noise, place promising work where I will find it, and then return control to me.

That is enough. The machine maintains the radar. The researcher decides where to look.

## Project and Services

- [LitScout-Echinochloa on GitHub](https://github.com/foreverhungerlai-hue/LitScout-Echinochloa)
- [Crossref REST API](https://www.crossref.org/documentation/retrieve-metadata/rest-api/)
- [OpenAlex API](https://docs.openalex.org/)
- [Unpaywall](https://unpaywall.org/products/api)
- [Zotero Web API](https://www.zotero.org/support/dev/web_api/v3/start)
