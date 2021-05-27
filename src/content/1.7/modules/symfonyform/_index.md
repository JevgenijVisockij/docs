---
title: Symfony form in module
weight: 10
aliases:
  - /module/05-CreatingAPrestaShop17Module/02-CreatingAFirstModule.html
---

# Getting started

In order to use Symfony form in your module you require:
1) A form type
2) A tempalte to display that form type
3) A way to save that form.

For your form type you can either use Symfony's form types (https://symfony.com/doc/3.4/reference/forms/types.html). Notice that currently PrestaShop uses Symfony 3.4. You can also use PrestaShops custom form types(https://devdocs.prestashop.com/1.7/development/components/form/types-reference/).

In order to compile form type into form that can be used in template you can use PrestaShop\PrestaShop\Core\Form\Handler. For form handler you require at least form type and data provider. Form type defines the way your form works and DataProvider is responsible for saving and retrieving data of that form.
  prestashop.module.demosymfonyform.form.demo_configuration_choice_form_data_handler:
    class: 'PrestaShop\PrestaShop\Core\Form\Handler'
    arguments:
      - '@form.factory'
      - '@prestashop.core.hook.dispatcher'
      - '@prestashop.module.demosymfonyform.form.demo_configuration_choice_form_data_provider'
      - 'PrestaShop\Module\DemoSymfonyForm\Form\DemoConfigurationChoiceType'
      - 'DemoConfiguration'

Which way you use depends if it's an object form or options form.

After you get form from form handler you can pass it to twig
$this->render('@Modules/demosymfonyform/views/templates/admin/form.html.twig', [
            'demoConfigurationTextForm' => $textForm->createView(),
        ]);


In twig before 1.7.8 you needed to define every field
EXAMPLE

After 1.7.8 majority of form types were simplified so instead your template can be as simple as 
{% extends '@PrestaShop/Admin/layout.html.twig' %}

{# PrestaShop made some modifications to the way forms are displayed to work well with PrestaShop in order to benefit from those you need to use ui kit as theme#}
{% form_theme demoConfigurationTextForm 'PrestaShopBundle:Admin/TwigTemplateForm:prestashop_ui_kit.html.twig' %}

{% block content %}
  {{ form_start(demoConfigurationTextForm) }}
  <div class="card">
    <h3 class="card-header">
      <i class="material-icons">settings</i> {{ 'Text form types'|trans({}, 'Modules.DemoSymfonyForm.Admin') }}
    </h3>
    <div class="card-block row">
      <div class="card-text">
        {{ form_widget(demoConfigurationTextForm) }}
      </div>
    </div>
    <div class="card-footer">
      <div class="d-flex justify-content-end">
        <button class="btn btn-primary float-right" id="save-button">
          {{ 'Save'|trans({}, 'Admin.Actions') }}
        </button>
      </div>
    </div>
  </div>
  {{ form_end(demoConfigurationTextForm) }}
{% endblock %}

Different ways to handle forms are defined here:
https://devdocs.prestashop.com/1.7/development/architecture/migration-guide/forms/crud-forms/

The same concepts that PrestaShop uses you can apply in module as well. 
```php
<?php
header('Expires: Mon, 26 Jul 1997 05:00:00 GMT');
header('Last-Modified: ' . gmdate('D, d M Y H:i:s') . ' GMT');
header('Cache-Control: no-store, no-cache, must-revalidate');
header('Cache-Control: post-check=0, pre-check=0', false);
header('Pragma: no-cache');
header('Location: ../');
exit;
```
