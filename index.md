---
title: UCSB Library Carpentry Workshops
---


## Workshops

<!-- show workshops after 2025-01-01 -->
{% assign cutoff = '2026-02-15' | date: "%s" %}
{% assign sorted_workshops = site.categories.workshop | sort: "date" %}
{% assign lastWorkshop = sorted_workshops | last %}
{% assign lastWorkshopDate = lastWorkshop.date | date: "%s" %}


<ul>
{% if lastWorkshopDate > cutoff %}
  {% for w in sorted_workshops %}
    {% assign postDate = w.date | date: "%s" %}
    {% if postDate > cutoff %}
      <li>{{ w.human_date }}: <a href="{{ w.url }}" target="_blank">{{ w.title }}</a></li>
    {% endif %}
  {% endfor %}
{% else %}
    <li> We don’t have any more workshops scheduled this quarter. Join our <a href="https://groups.google.com/u/1/a/library.ucsb.edu/g/carpentry/about" target="_blank">mailing list</a> to hear about future workshops. </li>
    <li> Have ideas for what we should offer next? <a href="https://tinyurl.com/future-workshops-ucsbcarpentry" target="_blank">Let us know here</a>. </li>
{% endif %}
</ul>

## Related events 
<ul>
  <li>March 6: <a href="https://www.library.ucsb.edu/events-exhibitions/workshop-1-how-make-map-arcgis-online" target="_blank">How to make a map with ArcGIS Online</a></li>
  <li>Thursdays this Winter: <a href="https://bit.ly/D2D_2026Winter" target="_blank">Data to Discovery Series (30-minute sessions on data visualization and visual communication)</a></li>
</ul>

## Community Meetings

<ul>
{% assign meeting=site.categories.meeting |  where_exp:"item", "item.date > site.time" | first %}
{% if meeting %}
    <li> <a href="{{meeting.url}}"> {{  meeting.date | date: "%B %d" }}: {{ meeting.title }} (agenda)</a>
      <p>{{ meeting.short_description }}</p>
    </li>
{% else %}
    <li> The next community meeting is not yet scheduled. Check back soon! </li>
{% endif %}
</ul>


UCSB Carpentry Community meetings are a venue for open discussion on topics in computational research. Meetings take place on Zoom and in the Library, and are open to all researchers in the Santa Barbara area. Feel free to propose a topic for an upcoming meeting!

## About Us

If you want to stay in the loop for future workshops, you can join our [Carpentry mailing list](https://groups.google.com/u/1/a/library.ucsb.edu/g/carpentry/about) with an @ucsb.edu email address and/or join our [Slack channel](https://join.slack.com/t/ucsbcarpentry/shared_invite/zt-3chxqtb2b-cVc3l~yj1ghUhZLNmOLfhQ). 
If you do not have an @ucsb.edu email address, please [email the DREAM Lab](mailto:dreamlab@library.ucsb.edu) directly so we can add you.

**[The Carpentries](https://carpentries.org/)** project is an international organization of volunteers teaching foundational coding and data science skills to researchers (Check out this [map of workshops held worldwide](https://feeds.carpentries.org/plot_workshops_map.svg)). 
Carpentry Workshops at UCSB are supported and organized by our stellar volunteers and the DREAM Lab (formerly, Interdisciplinary Research Collaboratory) at UCSB Library.

We offer guided and hands-on workshops to researchers, including undergraduate, staff, graduate students, post-docs, and faculty. 
These teachings focus on beginner-level research computing, data literacy, and information management. 
Resources that persist outside of workshops are our community of data research and programming "carpenters" and the teaching materials of [previous workshops](https://ucsbcarpentry.github.io/past-workshops).

For a few of us, teaching Carpentry workshops is part of our job descriptions, but most are volunteers from around campus and the global Carpentries community- including researchers, post-docs, graduate students, faculty and staff. 
If you would like to get involved with putting on these workshops, please email us at **dreamlab@library.ucsb.edu** or visit our **[Carpentry Community Page](https://ucsbcarpentry.github.io/community/instructors)**.

*Signed,*
<br>
*Your UCSB Carpentry Team*

P.S. [See the Library calendar page for more workshops!](https://www.library.ucsb.edu/events-exhibitions?location=All&series=1225)

