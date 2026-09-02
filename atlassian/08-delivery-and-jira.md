---
title: "08 Delivery and Jira"
source: confluence
space: "MS — Marketing Site"
page_id: "1093632002"
url: "https://jiracation.atlassian.net/wiki/spaces/MS/pages/1093632002"
parent: "Marketing Site"
status: "current"
version: 5
last_modified: "2026-06-12T13:42:55.189Z"
exported: "2026-09-02"
---

# 08 Delivery and Jira

# **1 About this document**

This document defines how we manage delivery for the Marketing Site project using Jira and Confluence.

Confluence is used to document why and what.

Jira is used to manage who does what and when.

The goal is to make delivery visible, prioritised and connected to the OKRs.

# **2 Delivery principles**

1. Every major task should support either qualified B2B lead generation or B2B brand building.
2. Confluence should hold the project context, decisions, page briefs and requirements.
3. Jira should hold epics, tasks, owners, status and delivery progress.
4. No major page should move into design or development without an approved page brief.
5. Tracking requirements should be defined before implementation.
6. Decisions should be documented in Confluence, not hidden in meetings or chat.
7. Done means implemented, reviewed, tested and measurable.

#  **3 Jira project type**

We will use a Kanban-style workflow.

The marketing site project contains content, design, development, tracking, QA and launch work. Kanban gives better visibility across these parallel workstreams.

# **4 Jira workflow**

We will use this workflow:

1. Backlog
2. To Do
3. In progress
4. In Review
5. Done

Use a clear **Blocked** flag or status for work that cannot move forward.

# **5 Jira epics**

## **5.1 Epic 1: Tracking and lead quality**

Purpose: Make sure the site can measure qualified B2B prospects, lead type, source and performance correctly.

Typical work:

* HubSpot field setup
* Form tracking
* Chat tracking
* AI analysis tool tracking
* Lead type classification
* Reporting dashboard
* Sales acceptance process

Related Confluence pages:

* 01 Strategy and OKRs
* 07 Tracking and Performance

## **5.2 Epic 2: Site structure and navigation**

Purpose: Create a clear information architecture based on B2B buyer needs.

Typical work:

* Sitemap
* Navigation structure
* URL structure
* Internal linking model
* Mobile navigation
* Redirect planning

Related Confluence pages:

* 05 Site Structure and User Journeys

## **5.3 Epic 3: Homepage and positioning**

Purpose: Make the homepage clearly explain who Videocation is for, what problem we solve and what outcome we create.

Typical work:

* Homepage brief
* Hero section
* Problem section
* Solution overview
* Use-case routing
* Proof sections
* Free analysis section
* Final CTA

Related Confluence pages:

* 02 Target Audience and Positioning
* 06 Page Briefs
* 04 Design System

## **5.4 Epic 4: B2B solution pages**

Purpose: Build pages for specific buying situations and competence needs.

Typical work:

* Leadership development page
* AI competence page
* HMS and compliance page
* Sales development page
* Project management page
* Onboarding page
* Team development page
* Strategic competence development page

Related Confluence pages:

* 05 Site Structure and User Journeys
* 06 Page Briefs

## **5.5 Epic 5: AI analysis tool and chat conversion**

Purpose: Implement the free analysis tool and chat as conversion paths before summer 2026.

Typical work:

* AI analysis landing page
* Organisation number lookup flow
* Preview before email capture
* Report email flow
* HubSpot integration
* Sales follow-up data
* Chat implementation
* Consent and privacy text

Related Confluence pages:

* 03 Best Practice Guide
* 07 Tracking and Performance
* 10 QA and Launch

## **5.6 Epic 6: Resources, blog and Norsk kompetansekompass**

Purpose: Build brand authority and create future demand through useful B2B content.

Typical work:

* Blog structure
* Resource hub
* Norsk kompetansekompass landing page
* Internal linking from articles to solution pages
* SEO optimisation
* Backlink support

Related Confluence pages:

* 03 Best Practice Guide
* 07 Tracking and Performance

## **5.7 Epic 7: Customer proof and case studies**

Purpose: Reduce buyer risk through customer stories, proof and relevant examples.

Typical work:

* Customer story template
* Customer logo section
* Case study pages
* Testimonials
* Proof blocks for solution pages
* Customer visuals

Related Confluence pages:

* 03 Best Practice Guide
* 06 Page Briefs

## **5.8 Epic 8: Design system implementation**

Purpose: Apply the visual identity consistently across the marketing site.

Typical work:

* Colour tokens
* Typography
* Buttons
* Navigation
* Cards
* Icons
* Image treatment
* CTA components
* Accessibility checks

Related Confluence pages:

* 04 Design System
* 10 QA and Launch

## **5.9 Epic 9: SEO and technical performance**

Purpose: Make sure the site is fast, findable and technically sound.

Typical work:

* SEO titles and meta descriptions
* Heading structure
* Internal links
* Redirects
* XML sitemap
* Image optimisation
* Core Web Vitals
* Mobile performance
* Accessibility improvements

Related Confluence pages:

* 07 Tracking and Performance
* 10 QA and Launch

## **5.10 Epic 10: QA and launch**

Purpose: Prepare the site for launch with clear quality control and go/no-go criteria.

Typical work:

* Content QA
* Design QA
* Mobile QA
* Tracking QA
* SEO QA
* Accessibility QA
* Performance QA
* HubSpot QA
* Launch checklist
* Post-launch monitoring

Related Confluence pages:

* 10 QA and Launch

# **11 Work item template**

Use this structure for Jira tasks.

## **11.1 Summary**

Clear action-oriented title.

Example:

“Create page brief for AI competence solution page”

## **11.2 Background**

Why this work is needed.

## **11.3 Deliverable**

What should be completed.

## **11.4 Related Confluence page**

Link to brief, decision or requirement.

## **11.5 Acceptance criteria**

List what must be true before the task can be moved to Done.

## **11.6 Tracking requirements**

List any events, fields or reporting needs.

## **11.7 Dependencies**

List anything blocking the work.

## **11.8 Owner**

One responsible person.

## **11.9 Reviewer**

Person responsible for approval.

# **12 Definition of Ready**

A task is ready to start when:

* The goal is clear
* The owner is assigned
* The required Confluence context exists
* Acceptance criteria are defined
* Dependencies are known
* Tracking requirements are defined, if relevant
* Design or content input is available, if required

# **13 Definition of Done**

A task is done when:

* The deliverable is completed
* Acceptance criteria are met
* Relevant review is completed
* QA is completed, if relevant
* Tracking is working, if relevant
* Documentation is updated, if relevant
* Related Jira issues are linked
* No critical defects remain

‌

# **14 Labels**

Use labels consistently.

Suggested labels:

* content
* design
* frontend
* seo
* analytics
* hubspot
* ai-analysis
* chat
* qa
* accessibility
* performance
* conversion
* brand
* blocked

# **15 Weekly delivery rhythm**

## **15.1 Weekly planning**

Purpose:

* Prioritise work for the week
* Move ready tasks into active work
* Identify blockers
* Confirm dependencies

## **15.2 Mid-week check**

Purpose:

* Review progress
* Resolve blockers
* Clarify open questions

## **15.3 Weekly review**

Purpose:

* Review completed work
* Check against OKRs and page briefs
* Decide what should be improved next

## **15.4 Monthly performance review**

Purpose:

* Review site performance
* Review lead quality
* Review brand indicators
* Prioritise improvement work

‌
