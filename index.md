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

1. DP2 is a key rehearsal release. We are best served by a DP2 that is in-between between DP1 and DR1 in size and complexity so that we can iterate on the data-release process and tooling improvements set out in detail in this document before tackling full scale. We also need a larger release still under the "Preview" caveat before going to the DR label.
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
This was compounded by the Data Services Lead failing to understand until late in the game that she needed to be exerting oversight of the entire data release "pipeline", rather that "just" leading RSP service development and data publication.

**Recommendation:**  A "shipping list" is drawn for each of the above six transfer points. The team passing the bucket has the responsibility to check that all the items in the shipment can be checked off. These shipping lists should be templated in some way (Github, JIRA) to be re-used for subsequent releases.

**Recommendation:** Future data release schedules include explicit deadlines for each of those steps, with steps 1-3 being parallelized to the extent possible. Missing these early deadlines should trigger a corresponding schedule delay.

**Recommendation:** A person who is _not_ the Data Services Lead, ideally from Pipelines or V&V should exercise oversight on steps 1-3 above and hand over to the Data Services Lead for steps 4-6.

### Gated approval from a person/body with sign-off powers

While the peer-to-peer bucket-brigade model is well suited for technical handover, there were a number of times where the Data Services Lead, specifically, had to make decisions that were not properly in her purview.
An example of this was which HiPS product to use for the coverage map, which field to center the coverage map to, etc.

Additionally, it was unclear who could be negotiated with for discussions along the lines of "do you want to go live like this, or do you want us to slip and fix this first".
The RSP product owners provided great input and context, however they did not have the authority to slip a community-promised date either.
Lines of command are further blurred by the fact that a data release crosses the boundaries of Data Management and System Performance.

**Recommendation:** A "decider of last resort" with the expertise and organisational standing to make quick authoritative decisions on appeal by the Data Services Lead should be designated.

Note that while in operations there is a plan for a Data Release board, it is still necessary to have a person (perhaps the same person would would chair this board) that can make timely decisions without a formal process.

### Insufficient internal red-team testing

One of the reasons that it was unclear how close to the deadline we were going to cut it until it was almost too late was the very late involvement of "critical" testers.
Organisationally there are two groups that can provide pre-release testing, however the V&V team focuses on pre-handover pipeline product testing, and the CST team focus on "best practice" testing for their educational materials.

The majority of late (2-3 week pre-release) serious issues was uncovered by testers with high levels of data and software domain expertise -- Product Owners, Pipeline Developers, Middleware Developers.
These had a priori knowledge of what the answer _should_ be, so they were excellent
Many of these were testing to be helpful off their own bat (and we appreciate them).
However, this activity requires more effort that can be applied on a volunteer basis or an ad-hoc timeline.

**Recommendation:** An organized red-team testing campaign should form part of the release schedule with testers with strong a priori knowledge of desired system behavior (and even some knowledge of where the bodies are buried).

Even one day of testing under such conditions, with a process for ticketing feedback, would be immensely valuable.

### Absence of a cumulative corpus for automated testing

From a technical point of view, it's oddly reassuring that our systems are mature enough for unscripted hand-offs.
The downside of this is that the lack of process means that it's harder to set up automated testing against delivered data products.
An example of this was the now infamous "where are the HiPS files" moment where it transpired it was unclear where the HiPS files were, where they should go, what the HiPS file delivery consisted of, whether it was a complete set etc.
At moments there was deja-vu with issues seen during DP0.2.

**Recommendation:** Adding to the recommendations on the "shipping list" checkpoint, at each checkpoint there should be a standardized representation of the data against which data checks can run. These tests would perform completeness checks, linting, and tests that check for errors previously seen to guard against regression.

The tests should be a collaboration of the two teams involved at each hand-off. Mobu, configured with a data curation flock, is a reasonable platform for easy test development, but other mechanisms (such as CI) is also acceptable.
The focus is on developing re-usable tests that can be run on future, including pilot, releases.

### Pre-mature external announcement of Data Release dates

Nobody wants to have to walk back a publicly announced release date, which is why it is important to hold back an announcement until all the risk has burned down.

This isn't just for the opportunity to burn down technical risk through an appropriate testing period, but also to allow for operational overrides or problems.

For DP1, we announced a release date on May 30th. At that time we had burned down the major technical risk (addressed by the TAP-Qserv bridge) but the bulk of testing was still ahead of us.
The lack of flexibility meant that we were not able to respond to the SLAC electrical work that took USDF off the air.
The unplanned (but not unpredictable) problems with the subsequent cold start inconvenienced our users and embarrassed  our technical staff.
While we discussed walking back the announced date until after the downtime, most of us did not feel comfortable advocating for this, especially given the high profile First Look exercise.

**Recommendation:** Particularly so for Data Previews, but ideally for Data Releases as well, public announcement of a date should only be made after testing, with run-up announcements of "Summer 2025" level of accuracy until the release is all-cleared.

Data Services does not have an opinion on whether it's better to have a very short (1-2 week) pre-announcement once the release is cleared or to hold the release for a longer pre-announcement period.


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

### Potential hybrid insufficiency

Shortly after DP1 release, DP0.2 data was unavailable to data.lsst.cloud users (due to an S3DF compounded outage) for 9 days (DP1 catalogs were unavailable 16 hours longer than plannedq). In this light, the decision by Data Services to host DP1 images on Google Cloud may seem a fortuitous avoidance of a black swan event.

What should not be lost is the reason for the decision to place DP1 next to the services in the first place.
In the run-up to DP1, we found total end-to-end performance from the object store at S3DF to GGP was surprisingly poor, as well as highly variable.
Additionally we had dropouts to communications to the Qserv REST API.
Because of the DP1 schedule we did not have time to properly investigate these, and concerned that slow performance would seem unreasonable (and worrying) to users for such a small volume release, Data Services decided to host the DP1 images on Google Cloud.
While this worked out reputationally, it means that the key operational model for a hybrid RSP (services on Google, data on SLAC) has not been proved by DP1, and remains a significant risk.

Moreover, any trade-off is a two-edged sword; data at USDF means more network transfers, but putting more services at USDF so that can be next to the data (eg computational services) means fewer network transfers but more exposure to S3DF reliability.

One advantage real-world DP1 usage gives us is a realistic user profile. We can use that to make decisions about whether to mitigate "pure hybrid" by having the most popular data products for the latest Data Release on GCP.

**Recommendation:** No later than following DP0.2 release (but ideally earlier) prepare for a critical internal review of the pure hybrid plan.

Data Services teams will start collecting data, conduct performance tests and calculate uptime statistics to support such a review.

### User sign-ups

The process for granting RSP accounts has the dubious merit of being simultaneously labor intensive, and insufficiently rigorous (and confusing to users but that is a more technical matter).

Ironically, the requirement for manual intervention for user creation was an asset for DP1 - 2,000 extra users you weren't expecting can't suddenly show up because it takes several days to process account creation requests.
However, even this process cannot stop account requests in bad faith.

**Recommendation:** We evaluate and a price third-party identity verification service.

### Idiosyncratic issues

The following issues arose due to issues that there is no reason to believe will arise in the future.

#### First Look

First Look was an event that required significant effort from pipelines at just the time where pipeline developer capacity would have been available for testing, sanity checking, data product iteration, etc.

#### Manic Miners

An abuse event where accounts created in bad faith were mining cryptocurrency distracted SQuaRE for a brief period while we audited the activity to ensure no systems were compromised, swept for perpetrators and de-activated and reported the relevant accounts

#### Data Engineering understaffing

Data Engineering has one full time developer FTE that has been diverted to address contingencies associated with the Prompt Products database.
The Data Engineering lead is 50% FTE with another project at a time where the other project was also preparing a Data Release.

As a result, data engineering effort was under-resourced compared to its planned and necessary effort level.

## Appendix: Technical-level recommendations

The body of this document focuses on management shortfalls and organisational issues across departments.

This appendix contains recommendations that can be addressed entirely within Data Management.

```{note}
This section is work in progress below first draft left
```
<!--
#### Disconnect between Qserv and Data Engineering

Felis, a Data Engineering product, controls schema and constitutes source of truth for Data Services.
It is not, however, source of truth for Qserv.

The Data Services Lead is taking an action to discuss with these teams improvements, ideally to have Felis be source of truth for Qserv, or as a temporary mitigation, cross-validate the Felis and Qserv schemas.

 -->

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
