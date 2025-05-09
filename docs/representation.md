---
title: Community Representation
permalink: representation
redirect_from:
  - /about/representation
---

The ERA is a member of the [Federation of Cambridge Residents’ Associations](https://www.fecra.org.uk/)
and represents Eddington residents in groups and committees locally and city-wide.

{% assign now_timestamp = "now"|date:"%s"|plus:0 %}
{% assign meetings = site.meetings|sort:"-sort_order"|reverse %}

{% for meeting in meetings %}
{% assign collection = site.collections | where: "label", "meetings" | first %}
{% assign next_meeting_timestamp = meeting.next_date|first|date:"%s"|plus:0 %}
{% assign minutes_prefix = "/minutes/" | append:meeting.minutes_dir | append:"/" %}
{% assign file_path_filter = "file.path contains '" | append:minutes_prefix | append:"'" %}
{% assign minutes = collection.files | where_exp:"file", file_path_filter %}

## {{ meeting.organisation }}

{% if meeting.frequency -%}
Meets {{ meeting.frequency }}
{%- else -%}
Inactive
{%- endif %}

{%- include separator.md -%} next meeting:
{% if next_meeting_timestamp > now_timestamp -%}
  {{ meeting.next_date|date: "%A %d %B %Y"}}
{%- else -%}
  TBA
{%- endif %}
{%- if meeting.website -%}
{%- include separator.md -%}  [organisation website]({{meeting.website}})
{%- endif %}

{{ meeting.content }}

{%- if meeting.is_public_minutes -%}
Previous meetings and minutes:<br/>
{%- else -%}
Previous meetings (minutes are not yet made public):<br/>
{%- endif -%}
{% for date in meeting.prev_dates -%}
  {%- assign date_str = date|date: "%Y-%m-%d" -%}
  {%- assign file_path_filter = "file.path contains '" | append: date_str | append:"'" -%}
  {%- assign file = minutes | where_exp:"file", file_path_filter | first -%}
  {%- assign day = date|date:"%-d" -%}
  {%- if file %} <a href="{{ file.path|replace_first: "_", "" }}">{%- include date_ordinal.md day=day -%}{{- date|date: " %B %Y" -}}</a>
  {%- else -%}
  {%- include date_ordinal.md day=day %}{{- date|date: " %B %Y" -}}
  {%- endif -%}
  {%- if forloop.last == false -%}
  ,&ensp;
  {%- endif -%}
{%- endfor %}

{% endfor %}
