---
title: "LitScout: A Simple Tool for Tracking New Literature"
date: 2026-07-20T10:00:00+07:00
lastmod: 2026-07-20T10:00:00+07:00
draft: false
url: "/technical/litscout-attention-not-acquisition/"
description: "A short introduction to LitScout, a local tool for finding recent papers, organizing useful records, and keeping literature review manageable."
summary: "LitScout checks for new literature, filters obvious noise, and sends useful records to Zotero. It helps me stay current without turning paper collection into the main task."
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

LitScout is a small local tool that checks for recent papers in my research area. It filters the results, removes duplicate records, and sends useful items to Zotero. When a legal open-access PDF is available, it can attach that file as well.

I built it because I wanted a simple way to stay aware of new work without spending too much time searching the same databases. The tool is used for a focused area of plant science, but the search terms and rules can be changed for other topics.

**Project repository:** [LitScout on GitHub](https://github.com/foreverhungerlai-hue/LitScout-Echinochloa)

## Why I Built It

The main problem was not a lack of papers. It was the time needed to find new work, remove irrelevant results, and decide what to read.

Downloading more PDFs does not solve this problem. A large folder of unread papers is still an unread folder. I needed a tool that could check what was new and prepare a short list for review.

LitScout is designed for that limited job. It supports literature discovery and organization, but it does not try to replace reading or research judgment.

## How It Works

| Step | What LitScout does |
|---|---|
| Search | Checks Crossref and, when enabled, OpenAlex for recent records |
| Clean | Normalizes DOI values and removes duplicate results |
| Filter | Uses editable keyword rules to reduce unrelated results |
| Organize | Saves selected records and basic metadata to Zotero |
| Access | Attaches a PDF only when Unpaywall reports a legal open-access copy |
| Review | Leaves the final reading decision to the researcher |

The search window, keywords, and score threshold can be changed from the local dashboard. Each run is intentionally limited so that the results remain small enough to check.

The filtering rules are visible and editable. They use information such as the title, abstract, subject, and journal metadata. These rules are not meant to decide whether a paper is scientifically important. They only help reduce obvious noise.

## Why It Does Not Bypass Paywalls

LitScout does not try to bypass publisher access controls. It checks Unpaywall for a legal open-access copy. If one is available, the PDF can be attached to the Zotero record. If not, LitScout keeps the citation and marks it as waiting for a PDF.

This is both an access boundary and a practical choice. The purpose of LitScout is to show that a relevant paper exists. It is not meant to build a complete local copy of the literature.

If a closed paper looks important, I can then look for it through an institutional subscription, a library, an author manuscript, or another appropriate route.

## Reading Still Requires a Decision

A title usually gives enough information for an initial decision, but it cannot replace the paper. I use a simple reading process:

1. Scan the title and basic information.
2. Check the abstract, figures, or methods when the paper may be useful.
3. Read closely only when it directly affects an experiment, method, interpretation, or piece of writing.
4. Keep other useful records in Zotero for later.

This matters because time is limited. Not every relevant paper needs the same level of attention. LitScout helps prepare the list; I still decide what to skim, what to read broadly, and what to study carefully.

## Local and Understandable

LitScout runs on my own computer through a small Flask dashboard. It can run a search, preview results, schedule a daily check, change search settings, import DOI lists, and remove incorrect matches created by the tool.

Credentials and optional API keys stay in a local configuration file that is excluded from Git. The dashboard is available only on the local computer.

The system is deliberately simple. Metadata can be incomplete, keyword rules can miss papers, and the rules need to change as a project develops. These limits are acceptable because I can see how the tool works and adjust it when necessary.

## What LitScout Is For

LitScout is not a systematic-review platform or an automatic reader. It has a smaller purpose:

- check for new literature regularly;
- remove duplicates and obvious noise;
- keep useful records organized;
- attach legal open-access files when available;
- leave reading priorities to the researcher.

The final goal is not to collect the largest number of papers. It is to stay current and use reading time carefully.

## Project and Services

- [LitScout on GitHub](https://github.com/foreverhungerlai-hue/LitScout-Echinochloa)
- [Crossref REST API](https://www.crossref.org/documentation/retrieve-metadata/rest-api/)
- [OpenAlex API](https://docs.openalex.org/)
- [Unpaywall](https://unpaywall.org/products/api)
- [Zotero Web API](https://www.zotero.org/support/dev/web_api/v3/start)
