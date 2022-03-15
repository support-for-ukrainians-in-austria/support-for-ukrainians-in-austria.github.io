---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

<p lang="ua">
  🇺🇦 На цій сторінці зібрано важливу та корисну інформацію для всіх, хто приїжджає до Австрії з України та для тих, хто їм допомагає. Це спільний проект – ми раді зворотній зв’язку та допомозі.
</p>

<p lang="de">
  🇦🇹 Diese Seite sammelt wichtige und nützliche Informations für alle, die aus der Ukraine nach Österreich kommen und für jene, die ihnen helfen. Das ist ein gemeinschaftliches Projekt - wir freuens auf Hinweise und Hilfe.
</p>

<p lang="en">
  🇬🇧 This page collects important and useful information for everyone who comes to Austria from Ukraine and for those who help them. This is a collaborative project - we welcome feedback and help.
</p>

<div id="select-languages">
  <form id="select-languages-form">
    <div class="align-middle" style="font-size: 2rem">
      <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" name="language" id="language_ua" value="ua" checked>
        <label class="form-check-label" for="language_ua">
          🇺🇦 Виберіть Мови
        </label>
      </div>
      <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" name="language" id="language_de" value="de">
        <label class="form-check-label" for="language_de">
          🇦🇹 Wähle Sprachen
        </label>
      </div>
      <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" name="language" id="language_en" value="en">
        <label class="form-check-label" for="language_en">
          🇬🇧 Select Languages
        </label>
      </div>
    </div>
  </form>
</div>

<script type="text/javascript">
  var languageSelector = document.getElementById('select-languages');
  var languageForm = document.forms['select-languages-form'];

  languageSelector.addEventListener('click', handleChange);
  function handleChange(event) {
    // Collect selected.
    var allOptions = languageForm.elements['language'];
    var selectedLanguages = [];
    var selectedClasses = [];
    allOptions.forEach((element) => {
      if (element.checked) {
        selectedLanguages.push(element.value);
        selectedClasses.push('.language-content-' + element.value);
      }
    });
    console.log('Selected languages: ' + selectedLanguages.join(', '));

    // Hide all.
    document.querySelectorAll('.language-content').forEach(function(el) {
      el.classList.add("hide");
    });
    // Display selected.
    document.querySelectorAll(selectedClasses.join(', ')).forEach(function(el) {
      el.classList.remove("hide");
    });
  }

  window.addEventListener('DOMContentLoaded', (event) => {
    handleChange()
  });
</script>

<div class="sections">
  {% for section in site.data.sections.index.sections %}
    <h2 id="{{ section.id }}" class="section">
      {% if section.title.ua %}
        <span class="language-content language-content-ua" lang="ua">🇺🇦 {{ section.title.ua }}</span>
      {% endif %}
      {% if section.title.de %}
        <span class="language-content language-content-de" lang="de">🇦🇹 {{ section.title.de }}</span>
      {% endif %}
      {% if section.title.en %}
        <span class="language-content language-content-en" lang="en">🇬🇧 {{ section.title.en }}</span>
      {% endif %}
    </h2>

    {% assign entries = site.data.sections.index.emptyArray %}
    {% for entry_hash in site.data.sections[section.id] %}
      {% assign entries = entries | push: entry_hash[1] %}
    {% endfor %}
    {% assign entries = entries | sort: "order" %}

    {% for entry in entries %}
      <ul id="{{ section.id }}-{{ entry.id }}" class="list-group section-entry">
        {% for content_hash in entry.content %}
        {% assign language = content_hash[0] %}
        <li id="{{ section.id }}-{{ entry.id }}-{{ language }}" class="list-group-item language-content language-content-{{ language }}" lang="{{ language }}">
          <div class="d-flex w-100 justify-content-between">
            <h5 class="mb-1">
              {% include snippets/flag.html language=language %}
              {{ content_hash[1].title }}
            </h5>
            {% if content_hash[1].source %}
            {% assign source = content_hash[1].source %}
            <small>
              {% include snippets/source.html language=language source=source %}
            </small>
            {% endif %}
          </div>
          {% if content_hash[1].body %}
          <div class="mb-1">
            {{ content_hash[1].body | markdownify }}
          </div>
          {% endif %}
        </li>
        {% endfor %}
      </ul>
    {% endfor %}
  {% endfor %}
</div>
