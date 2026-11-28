---
title: "Demo"
date: 2026-11-27
weight: 14
chapter: false
pre: " <b> 5.14. </b> "
---

#### Production link

**[https://sealaws.io.vn](https://sealaws.io.vn)**

The environment runs only during the evaluation period to control cost ([5.13](../5.13-cleanup/)). If the API answers 502/504, the runtime stack is paused; the recorded video below shows the same flow.

#### Demo video

{{< youtube nkpH7XdOSsc >}}

#### What the demo shows

The video walks through one hackathon from start to finish, from the point of view of each role:

| Role | What happens in the demo |
|---|---|
| Coordinator | Creates an event, sets up rounds, scoring rubrics and awards, opens registration |
| Team | Signs up (e-mail or Google/GitHub), forms a team, joins the event, submits a repository link before the timer runs out |
| Judge | Opens assigned submissions, scores them with the rubric, uses AI Assist for a suggested review of the repository |
| Coordinator | Collects the scores, publishes the results and the awards |

The AWS infrastructure behind each step is covered in sections [5.3](../5.3-architecture/)–[5.10](../5.10-testing-validation/).

{{% notice note %}}
Demo accounts use addresses under the reserved `.invalid` domain and have no password, so nobody can sign in with them and no e-mail reaches a real person. Real test credentials are never published on this page.
{{% /notice %}}
