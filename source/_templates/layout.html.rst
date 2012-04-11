{# layout.html ~~~~~~~~~~~~~~~~~ Custom template based on Haiku theme. #} {% extends "basic/layout.html" %} {% set script_files = script_files + ['_static/theme_extras.js'] %} {% set css_files = css_files + ['_static/print.css'] %} {# do not display relbars #} {% block relbar1 %}{% endblock %} {% block relbar2 %}{% endblock %} {% macro nav() %}
{%- block customrel1 %} {%- endblock %} {%- if prev %} «  {{ prev.title }}   ::   {%- endif %} {{ _('Contents') }} {%- if next %}   ::   {{ next.title }}  » {%- endif %} {%- block customrel2 %} {%- endblock %}
{% endmacro %} {% block content %}
{%- block customheader %} {%- if theme_full_logo != "false" %} {%- else %} {%- if logo -%} {%- endif -%}
{{ shorttitle|e }}
{{ title|striptags|e }}
{%- endif %} {%- endblock %}
{{ nav() }}
{#{%- if display_toc %}
Table Of Contents
{{ toc }}
{%- endif %}#} {% block body %}{% endblock %}
{{ nav() }}
{% endblock %} {% block footer %} {{ super() }} {%- if theme_google_analytics_code -%} {%- endif %} {% endblock %}