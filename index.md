---
---

{% capture readme %}{% include_relative README.md %}{% endcapture %}
{{ readme | markdownify | rellinks | replace: '.md', '.html' }}
