---
layout: page
nav: true
nav_order: 2
permalink: /skills/
title: Skills
description: "Estas son algunas de las habilidades que he adquirido en mis años de experiencia"
tabs: false
---

<div class="key-skills mb-5">
  <h3>Competencias clave</h3>
  <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 mt-3">
    {% for skill in site.data.key_skills %}
      <div class="col mb-3">
        <div class="card h-100">
          <div class="card-body">
            <h5 class="card-title" style="font-size: 1.1rem;">{{ skill.name }}</h5>
            <div class="d-flex justify-content-between align-items-center mt-3">
              <span class="badge badge-light border text-muted"><i class="fa-regular fa-clock mr-1"></i>{{ skill.experience }}</span>
              <span class="badge badge-primary">{{ skill.level }}</span>
            </div>
          </div>
        </div>
      </div>
    {% endfor %}
  </div>
</div>

<div class="technical-skills">
  <h3>Competencias técnicas</h3>
  <p class="text-muted mb-4">Utiliza los filtros de abajo o haz clic en los encabezados para buscar y ordenar las habilidades según el campo de aplicación, grupo o tu nivel de dominio.</p>

  <div class="skills-filter row mb-3">
    <div class="col-md-3 mb-2">
      <input type="text" id="skill-search" class="form-control form-control-sm" placeholder="Buscar skill...">
    </div>
    <div class="col-md-3 mb-2">
      <select id="filter-category" class="form-control form-control-sm">
         <option value="">Todas las categorías</option>
         {% assign categories = site.data.technical_skills | map: "category" | uniq %}
         {% for cat in categories %}
           <option value="{{ cat }}">{{ cat }}</option>
         {% endfor %}
      </select>
    </div>
    <div class="col-md-3 mb-2">
      <select id="filter-group" class="form-control form-control-sm">
         <option value="">Todos los grupos</option>
         {% assign groups = site.data.technical_skills | map: "group" | uniq | sort %}
         {% for group in groups %}
           <option value="{{ group }}">{{ group }}</option>
         {% endfor %}
      </select>
    </div>
    <div class="col-md-3 mb-2">
      <select id="filter-level" class="form-control form-control-sm">
         <option value="">Todos los niveles</option>
         <option value="Experto">Experto</option>
         <option value="Avanzado">Avanzado</option>
         <option value="Intermedio">Intermedio</option>
         <option value="Básico">Básico</option>
      </select>
    </div>
  </div>

  <div class="table-responsive">
    <table class="table table-sm table-hover" id="skills-table">
      <thead class="thead-light">
        <tr>
          <th class="sortable" data-sort="name" style="cursor: pointer;">Skill <i class="fa-solid fa-sort ms-1" style="font-size: 0.8em; color: #888;"></i></th>
          <th class="sortable" data-sort="category" style="cursor: pointer;">Categoría <i class="fa-solid fa-sort ms-1" style="font-size: 0.8em; color: #888;"></i></th>
          <th class="sortable" data-sort="group" style="cursor: pointer;">Grupo <i class="fa-solid fa-sort ms-1" style="font-size: 0.8em; color: #888;"></i></th>
          <th class="sortable" data-sort="experience" style="cursor: pointer;">Experiencia <i class="fa-solid fa-sort ms-1" style="font-size: 0.8em; color: #888;"></i></th>
          <th class="sortable" data-sort="level" style="cursor: pointer;">Nivel <i class="fa-solid fa-sort ms-1" style="font-size: 0.8em; color: #888;"></i></th>
        </tr>
      </thead>
      <tbody>
        {% for item in site.data.technical_skills %}
          <tr class="skill-row" data-category="{{ item.category }}" data-group="{{ item.group }}" data-level="{{ item.level }}">
             <td class="skill-name font-weight-bold">{{ item.name }}</td>
             <td class="skill-category text-muted">{{ item.category }}</td>
             <td class="skill-group text-muted">{{ item.group }}</td>
             <td class="skill-experience">{{ item.experience }}</td>
             <td class="skill-level">
               {% if item.level == 'Experto' %}
                 <span class="badge badge-success">{{ item.level }}</span>
               {% elsif item.level == 'Avanzado' %}
                 <span class="badge badge-primary">{{ item.level }}</span>
               {% elsif item.level == 'Intermedio' %}
                 <span class="badge badge-info">{{ item.level }}</span>
               {% elsif item.level != '' and item.level != nil %}
                 <span class="badge badge-secondary">{{ item.level }}</span>
               {% else %}
                 <span class="text-muted" style="font-size: 0.85em;">-</span>
               {% endif %}
             </td>
          </tr>
        {% endfor %}
      </tbody>
    </table>
  </div>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const searchInput = document.getElementById('skill-search');
  const catFilter = document.getElementById('filter-category');
  const grpFilter = document.getElementById('filter-group');
  const lvlFilter = document.getElementById('filter-level');
  const rows = Array.from(document.querySelectorAll('.skill-row'));
  
  function filterTable() {
    const term = searchInput.value.toLowerCase();
    const cat = catFilter.value;
    const grp = grpFilter.value;
    const lvl = lvlFilter.value;
    
    rows.forEach(row => {
      const name = row.querySelector('.skill-name').innerText.toLowerCase();
      const rowCat = row.getAttribute('data-category');
      const rowGrp = row.getAttribute('data-group');
      const rowLvl = row.getAttribute('data-level');
      
      const matchSearch = name.includes(term);
      const matchCat = cat === '' || rowCat === cat;
      const matchGrp = grp === '' || rowGrp === grp;
      const matchLvl = lvl === '' || rowLvl === lvl;
      
      if (matchSearch && matchCat && matchGrp && matchLvl) {
        row.style.display = '';
      } else {
        row.style.display = 'none';
      }
    });
  }
  
  searchInput.addEventListener('input', filterTable);
  catFilter.addEventListener('change', filterTable);
  grpFilter.addEventListener('change', filterTable);
  lvlFilter.addEventListener('change', filterTable);
  
  // Sorting logic
  const table = document.getElementById('skills-table');
  const headers = table.querySelectorAll('th.sortable');
  let currentSort = { col: null, asc: true };
  
  headers.forEach((th, index) => {
    th.addEventListener('click', () => {
      const isAsc = currentSort.col === index ? !currentSort.asc : true;
      currentSort = { col: index, asc: isAsc };
      
      // Update icons
      headers.forEach(h => {
        const icon = h.querySelector('i');
        icon.className = 'fa-solid fa-sort ms-1';
        icon.style.color = '#888';
      });
      const currentIcon = th.querySelector('i');
      currentIcon.className = isAsc ? 'fa-solid fa-sort-up ms-1' : 'fa-solid fa-sort-down ms-1';
      currentIcon.style.color = '#000';
      
      // Perform sort
      const tbody = table.querySelector('tbody');
      const visibleRows = rows.filter(r => r.style.display !== 'none');
      const hiddenRows = rows.filter(r => r.style.display === 'none');
      
      visibleRows.sort((a, b) => {
        let valA = a.children[index].innerText.trim();
        let valB = b.children[index].innerText.trim();
        
        // Try numeric sort for experience if applicable (like "+10 años")
        if (index === 3) {
          const numA = parseInt(valA.replace(/[^0-9]/g, '')) || 0;
          const numB = parseInt(valB.replace(/[^0-9]/g, '')) || 0;
          if (numA !== numB) {
             return isAsc ? numA - numB : numB - numA;
          }
        }
        
        return isAsc ? valA.localeCompare(valB) : valB.localeCompare(valA);
      });
      
      // Reattach to table
      tbody.innerHTML = '';
      visibleRows.forEach(r => tbody.appendChild(r));
      hiddenRows.forEach(r => tbody.appendChild(r));
    });
  });
});
</script>