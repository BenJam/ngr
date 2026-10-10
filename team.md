---
layout: page
title: Team
permalink: /team/
intro: The riders. Results not included.
---

{% if site.data.riders and site.data.riders.size > 0 %}
<ul class="card-grid rider-grid" role="list">
  {% for rider in site.data.riders %}
    {% include rider-card.html rider=rider %}
  {% endfor %}
</ul>
{% else %}
Rider profiles coming soon.
{% endif %}

Want to be on here? [Here's how joining works](/join/).
