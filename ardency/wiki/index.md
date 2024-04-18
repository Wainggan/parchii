---
title: ardency wiki
---

# wiki

an official, discontinuous wiki, which houses an incomplete subsection of the world.
more info will be released when the time comes.

## featured

- [-](./world/) ~ the planet.
- [Ardency](./world/ardency) ~ the place, not this project.


## index

- [general](./general/)
- [world](./world/)
- [characters](./character/)
- [events](./event/)

{% assign folder = site.pages | where_exp: "page", "page.url contains '/ardency/wiki' " %}
{% for page in folder %}{% if page.name != "index.md" %}
- [{{ page.name }}]({{ page.url }})
{%- endif -%}
{%- endfor %}


## note

a lot of this is incomplete, some of it by design, but mostly because I haven't finalized a lot of important details, like names or events. there's also some details that I'd like to add only *after* I incorporate it in a game (not [ardent shot](/ardency/ardentshot/)). unfortunately that might take a while.

all info in this wiki is canon, but incomplete. for canon but likely innaccurate information from *inside* the world, check out the [leaks index](/ardency/leaks/).

