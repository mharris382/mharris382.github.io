---
layout: default
---

<h1>Projects</h1>

<ul>
  {% for project in site.projects %}
    <li class="project-grid">
        <div class="proj-info">
            <h2><a href="{{ project.url }}">
                {{ project.title }} - {{ project.engine | capitalize}}
            </a></h2>
            <p>{{ project.excerpt | markdownify }}</p>
        </div>
        <img class="proj-thumb" src="assets/images/{{project.thumbnail}}" alt="thumbnail" width="100px" height="100px">
    </li>
  {% endfor %}
</ul>
