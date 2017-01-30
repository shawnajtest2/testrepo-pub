---
layout: post
title:  Django_bootstrap3 form
tags:
- Blog
- Post
---

I am amazed with how much code one can save using django_bootstrap3 forms. Here is a visual.

<b>Before</b>:
<pre>
<code>
<form method="post" role="form">
    {{ "{% csrf_token "}} %}
    <div class="form-group has-feedback">
      <label class="control-label" for="type">Reagent Type</label>
      <input type="text" class="form-control" name="type" placeholder="Reagent Type"
        {% if form.type.value %} value="{{ form.type.value }}" {% endif %}
          {% if form.type.errors %}
          {% for error in form.type.errors %}
            <span><p class="input-field-error">{{ error|escape }}</p></span>
            <span class="input-field-error glyphicon glyphicon-remove form-control-feedback" aria-hidden="true"></span>
          {% endfor %}
        {% endif %}
      </input>
    </div>


    <div class="form-group has-feedback">
      <label class="control-label" for="manufacturer">Reagent Manufacturer</label>
      <input type="text" class="form-control" name="manufacturer" placeholder="Reagent Manufacturer"
        {% if form.manufacturer.value %} value="{{ form.manufacturer.value }}" {% endif %}>
          {% if form.manufacturer.errors %}
            {% for error in form.manufacturer.errors %}
              <span><p class="input-field-error">{{ error|escape }}</p></span>
              <span class="input-field-error glyphicon glyphicon-remove form-control-feedback" aria-hidden="true"></span>
            {% endfor %}
          {% endif %}
    </input>
    </div>

    <div class="form-group has-feedback">
      <label class="control-label" for="lot">Reagent Lot</label>
      <input type="text" class="form-control" name="lot" placeholder="Reagent Lot"
        {% if form.lot.value %} value="{{ form.lot.value }}" {% endif %}>
          {% if form.lot.errors %}
            {% for error in form.lot.errors %}
              <span><p class="input-field-error">{{ error|escape }}</p></span>
              <span class="input-field-error glyphicon glyphicon-remove form-control-feedback" aria-hidden="true"></span>
            {% endfor %}
            {% endif %}
    </div>

    <div class="form-group has-feedback">
      <label class="control-label" for="expiry">Reagent expiry</label>
      <input type="text" class="form-control" id="expiry" name="expiry" placeholder="Reagent expiry"
      {% if form.expiry.value %} value="{{ form.expiry.value | date:"c"}}" {% endif %}>
        {% if form.expiry.errors %}
          {% for error in form.expiry.errors %}
            <span><p class="input-field-error">{{ error|escape }}</p></span>
            <span class="input-field-error glyphicon glyphicon-remove form-control-feedback" aria-hidden="true"></span>
          {% endfor %}
        {% endif %}
    </div>

    <div class="checkbox">
      <label>
          <input class="control-label" type="checkbox" name="requiresIDcard"
              {% if form.requiresIDcard.value %} checked {% endif %}>
          RequiresIDcard
      </label>
    </div>

    <div class="reagent_toolbar">
      <input type="submit" class="btn btn-primary" value="Save" />
      <a href="/reagent/" class="btn btn-default" onclick="return App.click(this);">Cancel</a>
          <div class="pull-right">
              <a href="#" onclick="clicked();" class="btn btn-danger">Delete reagent</a>
          </div>
    </div>
</form>
</code>
</pre>

<b>After using django_bootstrap3 form</b>:
<pre>
<code>

</code>
</pre>
