

---
layout: default
title: "Αγάλματα"
---

<h1 style="text-align:center; margin-top:30px;">Αγάλματα</h1>

<div style="display:grid; grid-template-columns:repeat(auto-fill,minmax(260px,1fr)); gap:20px; padding:20px;">
  {% for poi in site.pois %}
  <div style="border:1px solid #e5e7eb; border-radius:12px; overflow:hidden; background:#fff; box-shadow:0 6px 16px rgba(0,0,0,0.08);">
    <a href="{{ poi.url | relative_url }}" style="text-decoration:none; color:inherit; display:block;">
      <img id="img-{{ forloop.index }}" src="" style="width:100%; height:180px; object-fit:cover;">
      <div style="padding:14px;">
        <h3 style="margin:0 0 8px 0; color:#0f172a;">{{ poi.title }}</h3>
        <p id="desc-{{ forloop.index }}" style="color:#475569; font-size:.95rem; line-height:1.5; margin:0;"></p>
      </div>
    </a>
  </div>
  {% endfor %}
</div>

<script>
const pois = [
  {% for poi in site.pois %}
  {id: "{{ poi.wikidatum }}", idx: {{ forloop.index }}},
  {% endfor %}
];

pois.forEach(p => {
  fetch(`https://www.wikidata.org/wiki/Special:EntityData/${p.id}.json`)
    .then(res => res.json())
    .then(data => {
      const e = data.entities[p.id];
      const desc = e.descriptions?.el?.value || e.descriptions?.en?.value || '';
      const img = e.claims?.P18?.[0]?.mainsnak?.datavalue?.value;
      if (img) {
        document.getElementById(`img-${p.idx}`).src =
          `https://commons.wikimedia.org/wiki/Special:FilePath/${encodeURIComponent(img)}?width=480`;
      }
      document.getElementById(`desc-${p.idx}`).innerText = desc;
    });
});
</script>

---
