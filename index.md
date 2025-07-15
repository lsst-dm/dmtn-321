# Process and tooling recommendations for future Data Releases drawn from the experience of Data Preview 1

```{abstract}
Data Preview 1 was the first release of Rubin acquired, processed and published data. While DP1 achieved its goals and was released on time and to great success, we identified a number concerns that should be resolved before bigger data releases.

This document is a proposal for process and tooling improvements to cover the lifecycle from after the Pipelines processing through to Rubin Science Platform publication of the data release.
```

## Context

Data Preview 1 (DP1) was the successful release of Rubin's first end-to-end data products. While it met its goals (and a number of its stretch goals), the process of getting there was error-prone, planned contingency was exhausted, and a number of Data Management staff had to go into last-minute hero-mode to pull it off.

```{important}
This document can be considered a management-level lessons-learned/post-mortem for the purposes of creating a sound basis for future data releases. No criticism is attached to Data Management staff whose expertise, high standards and extra-mile work were key to DP1's success.
```

Our aim is not just to achieve results, it's to achieve results with a process that is efficient in staff time to preserve as much development time as possible.
Sustained development is central to keeping our software and services state of the art.
Delays, miscommunication, errors, human time spent on tasks that can be machine-performed and heroism to increase number of hours available in a week are signs of a process in need of improvement.

We regard hero-mode as the aviation equivalent of a near-miss: the bad outcome did not occur, and barely anybody pays attention to it, but it's a warning sign that should not be ignored.

In some cases, gaps in our process could only be mitigated due to DP1's very small volume (allowing numerous do-overs), which will not be the case for future releases.
Complacency on the basis of DP1's apparent success is dangerous and we ignore its lessons at our peril.

## Key Project-Level Recommendations

The key project-level recommendations in this document are:

1. DP2 is a key rehearsal release. We are best served by a DP2 that is in-between between DP1 and DR1 in size and complexity so that we can iterate on the data-release process and tooling improvements set out in detail in this document before tackling full scale. We also need a larger release still under the "Preview" shield before going to the DR label.
2. Adequate time is taken between DP2 and DR1 to ensure the data release process is as turn-key as possible and allow us to get on a sound footing of sustainable annual data release schedules. While community enthusiasm for DR1 is obviously sky-high, failure to adequately prepare for DR1 risks creating a snowball effect that puts all subsequent releases at risk. Data Services is thus strongly in favor of the proposed plan for an end-of-year-1 DR1 (as opposed to a 6-month DR1)
3. No later than the month following DP2, the RSP hybrid model should be technically reviewed to determine whether it is sound in its present form and if not, any related risks should be exercised so that mitigations can be in place in time for DR1. This item also reinforces (1) and (2).
4. In the context of the scope of this document, the Prompt Products release is entirely un-rehearsed and requires significant attention from Data Management to get right. Given the obvious high interest in the community for timely transient science, this release should be given equivalent priority (if not higher) than DP2. This item also reinforces (2).
5. Agency or other sign-off should be sought to make even a ComCam-sized subset of the data "ok public" (if not actually public) to bypass restrictions that prevent us from shipping data for CI purposes to your cloud-hosted services.

For recommendations at the Data Management group levels and the reasoning for the key recommendations, see individual sections.

## Problem summary

The overall symptom of the problems identified was:

* The interval between data production of DP1 and live release on the RSP took 4 months of elapsed effort, instead of the anticipated 3 months.
* In the three month plan, there was 1 month of contingency which was fully exhausted.

The release was on time regardless due to heroic efforts, a minor fortuitous delay in First Light release, slipping some issues unlikely to be immediately impacting users till after going live, deferring non-DP1-related tasks, do-overs, and fixing bugs in the wild (mostly found by staff testing) that could have been caught ahead of time had contingency not eaten into the testing time.

A number of these interventions would have been ineffective for a DR1-scale event.

The eventual goal should be a 3-month worst-case release consisting of 2 months of release work _including_ contingency, and 1 month of testing/bugfixing prior to release. We are unlikely to reach this goal in the next two releases.

## High-level issues, and related recommendations


### Handover checkpoints

While it's not quite as simple, there are six major transfers that can serve as checkpoints after data production is complete:

1. Pipelines to Qserv (Catalogs)
2. Pipelines to Middleware (Images)
3. Pipelines to [Data Services? Middleware?] (HiPS)
4. Qserv to/from Data Engineering ()
5. Data Engineering to SQuaRE/Portal
6. Data Services to CST

In the data previews so far this has been done "bucket-brigade" style with teams leads at each transfer point co-ordinating the bucket-passing between themselves, as opposed to the Czar model of a single person managing all the steps.

The bucket-brigade model is defensible, as the people in the observatory of the caliber and technical expertise to serve as a Czar are already over-subscribed, and many of these transfers require deep domain knowledge of the two systems involved.
The cracks in DP1 were less due to the bucket-brigade model itself and more to the ad-hoc definition of who was passing the bucket when, to whom, what was in the bucket anyway, and whose role it was to check that.

**Recommendation:**  A "shipping list" is drawn for each of the above six transfer points. The team passing the bucket has the responsibility to check that all the items in the shipment can be checked of.

**Recommendation:** Future data release schedules include explicit deadlines for each of those steps, with steps 1-3 being parallelized to the extent possible.

### Gated approval from a person/body with sign-off powers

While the peer-to-peer bucket-brigade model is well suited for technical handover, there were a number of times where the Data Services Lead, specifically, had to make decisions that were not properly in her purview.
An example of this was which HiPS product to use for the coverage map, which field to center the coverage map to, etc.

Additionally, it was unclear who could be negotiated with for discussions along the lines of "do you want to go live like this, or do you want us to slip and fix this first".
The RSP product owners provided great input and context, however they did not have the authority to slip a community-promised date either.
Lines of command are further blurred by the fact that a data release crosses the boundaries of Data Management and System Performance.

**Recommendation:** A "decider of last resort" with the expertise and organisational standing to make quick authoritative decisions on appeal by the Data Services Lead should be designated.

Note that while in operations there is a plan for a Data Release board, it is still necessary to have a person (perhaps the same person would would chair this board) that can make timely decisions without a formal process.


### Poorly anticipated user behavior

Here are some sobering statistics on the comparison between DR1 and (estimated) DP1

| Product                     | DP1          | DP0.2          | factor  | Notes |
|-----------------------------|--------------|--------------|---------|-------|
| Unique patches covered      |          638 |          7,693 |     x12 |          |
| Visit-detector images       |       15,972 |      2,805,017 |    x176 |          |
| DiaObject Rows              |    1,089,818 |     41,301,558 |     x38 |          |
| DiaSource Rows              |    3,086,404 |    162,448,407 |     x54 |          |
| SSource                     |        5,988 |    653,005,444 | x109052 | (cg DP0.3 10-year)|
| ForcedSourceOnDiaObject     |  196,911,566 | 16,817,191,952 |     x86 |          |

One of the key observations that has arisen _since_ the launch of DP1 is that the DP0.2 userbase (and our internal scale testing campaign that drew for it) were a very poor predictor of actual user behavior with real data (for example types, rates and complexity of searches).

Even within the Data Management teams who were already certain DP0.2 had instilled a false sense of confidence in a number of areas, it was an unpleasant surprise how little DP0.2 had prepared us for DP1.
As an example, real-world users uncovered more issues than scale testing with 10x more users.
Real users do wrong things, things you didn't expect, and generally don't behave like unit tests - which software teams are well aware of, but with the level of flexibility provided by the RSP, user-introduced entropy escalates much faster.
There's a massive difference between "what if the user presses the wrong button" and "what if the user writes bad code", as there are only so many buttons but an infinite ways to  get code wrong.

As a result, we have a critical need for a DP0.2-sized (minimum ~300 square degrees) data preview with real users working on LSSTCam data as an intermediate step before jumping to full DR1 size.

**Recommendation:** A substantial DP2 release is needed to give us user behavior data on a genuine DR1 precursor while we still have the protection of the "preview" label so that we can explain to the community we are still learning. It should not be ruled out that an addition "preview" release will be required before we can stand behind something with the DR1 label.


## What a complete process would look like

##

## Appendix: Technical-level recommendations


<!-- we need to be previewing substantial bigger stuff
    bulk cutout
    forced photomerty
    how long do we keep visits in the cache
we are getting hammered every day with people doing genuine things
    more users
    more data
    more queries
    more results from each query

static obscore table in Qserv, many manual steps

glue tooling is lacking


- alpha version of the catalog schema really is needed (ucds, description)
- Jim sat down with the object table and immediately found issues
- lame excuse for a billion data project because we can't run CI due to data
    we need a small public dataset
    WE HAVE TO HAVE CI WITH REAL DATA IN IT
    Nate still on FL
    HIPS production pipelines was rewritten and the data flow is different (way data was aggregated was different) as w

 -->
