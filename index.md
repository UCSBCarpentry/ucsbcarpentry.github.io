---
title: UCSB Library Carpentry Workshops
---
![carpentry logo](fig/banner-carpentry.png)

## Fall 2022 Workshop Schedule

The target audience are researchers with little to no computational experience, and are looking to learn tools that enable data-driven discovery. These workshops are synchronous and hands-on. These are open to any academics in the Santa Barbara area including UCSB, Westmont, and SBCC.  

- [September 6 - 13: UC Carpentries Fall 2022 Workshop Series](https://ti.to/ucsd-carpentries/uc-carpentries-fall-workshop-2022). zoom/remote
- [October 11: Data Organization in Spreadsheets](https://ucsbcarpentry.github.io/2022-10-11-ucsb-spreadsheets/), in-person
- [October 18-20: Introduction to the Unix Shell & Version Control with Git](https://ucsbcarpentry.github.io/2022-10-18-ucsb-bash-git/), in person
- [October 25 & 27: Data Analysis and Visualization in R](https://ucsbcarpentry.github.io/2022-10-25-ucsb-intro-R/), in person
- [November 8-10: Introduction to Reproducible Publications with RStudio](https://ucsbcarpentry.github.io/2022-11-8-ucsb-reproducible-pubs/), in person

## Community Meetings

UCSB Carpentry Community meetings are a venue for open discussion on topics in computational research. Meetings take place on Zoom and are open to all researchers in the Santa Barbara area. Feel free to propose a topic for an upcoming meeting!

{% assign meeting=site.categories.meeting |  where_exp:"item", "item.date > site.time" | first %}
<ul>
<li><a href="{{meeting.url}}"> {{  meeting.date | date: "%B %d" }}: {{ meeting.title }} (agenda) </a></li>
</ul>

## About Us
**[The Carpentries](https://carpentries.org/)** project is an international organization of volunteers teaching foundational coding and data science skills to researchers. 
Carpentry Workshops at UCSB are supported and organized by our stellar volunteers and the DREAM Lab (formerly, Interdisciplinary Research Collaboratory) at UCSB Library.
<!---
Uncomment after the page is done.
[We have a few policies.](community/workshops)
-->

We offer guided and hands-on workshops to researchers, including undergraduate, staff, graduate students, post-docs, and faculty. 
These teachings focus on beginner-level research computing, data literacy, and information management. 
Resources that persist outside of workshops is our community of data research and programming "carpenters," and the teaching material of [previous workshops](https://ucsbcarpentry.github.io/past-workshops).

If you want to stay in the loop for future workshops, you can join our Carpentry mailing list with an @ucsb.edu email address and/or join our slack channel. 
If you do not have an @ucsb.edu email address, please email the DREAM Lab directly so we can add you.

For a few of us, teaching Carpentry workshops is part of our job descriptions, but most are volunteers from around campus and the global Carpentries community- including researchers, post-docs, graduate students, faculty and staff. 
If you would like to get involved with putting on these workshops, please email us at **dreamlab@library.ucsb.edu** or visit our **[Instructors Page](https://ucsbcarpentry.github.io/community/instructors)**.

*Signed,*
<br>
*Your UCSB Carpentry Team*

P.S. [See the DREAM Lab calendar page for more events!](https://www.library.ucsb.edu/events-exhibitions?location=All&series=1218)

