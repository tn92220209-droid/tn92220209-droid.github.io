---
layout: page
title: Proyectos
permalink: /projects/
description: Colección de proyectos personales y profesionales de todas las épocas.
avatar: assets/img/coming_soon.jpeg
nav: true
nav_order: 3
display_categories: [featured, small_things, old, opensource, fun]
horizontal: false
---

<div class="tag-category-list">
  <ul class="p-0 m-0">
  {% for category in page.display_categories %}
    <li>
      <i class="fa-solid fa-tag fa-sm"></i> <a href="{{ category | slugify | prepend: '/projects/#' | relative_url }}">{{ category }}</a>
    </li>
    {% unless forloop.last %}
      <p>&bull;</p>
    {% endunless %}
  {% endfor %}
  </ul>
</div>

<!-- pages/projects.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if category == 'small_things' %}
  <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3">
    {% for project_hash in site.data["small-projects"] %}
      {% assign project = project_hash[1] %}
      <div class="col">
        {% include projects_list_item.liquid %}
      </div>
    {% endfor %}
  </div>
  {% else %}
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% if project.category == 'small_things' %}
        {% include projects_horizontal_simple.liquid %}
      {% else %}
        {% include projects_horizontal.liquid %}
      {% endif %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% if project.category == 'small_things' %}
        {% include projects_simple.liquid %}
      {% else %}
        {% include projects.liquid %}
      {% endif %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
