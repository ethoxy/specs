---
title: Deprecated
subtitle: All Ethereum Classic Improvement Proposals
layout: default
---

**Deprecated** proposals in other repositories:

<div class="small-column" markdown="1">

{% capture content %}
{% for page in site.data.org %}
{% if page.status == "Deprecated" %}
{% include toc-item.html item=page %}
{% endif %}
{% endfor %}
{% endcapture %}

{% include toc-raw.html content=content %}

</div>

