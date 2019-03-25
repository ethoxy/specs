---
title: Draft
subtitle: All Ethereum Classic Improvement Proposals
layout: default
---

**Draft** proposals in That World and Ethoxy:

{% capture content %}
{% for page in site.specs %}
{% if page.status == "Draft" %}
{% include toc-item.html item=page %}
{% endif %}
{% endfor %}
{% endcapture %}

{% include toc-raw.html content=content %}

**Draft** proposals in other repositories:

<div class="small-column" markdown="1">

{% capture content %}
{% for page in site.data.org %}
{% if page.status == "Draft" %}
{% include toc-item.html item=page %}
{% endif %}
{% endfor %}
{% endcapture %}

{% include toc-raw.html content=content %}

</div>

