# Include statements

The `include` statement includes or nests a twig template \(components, node, blocks, etc\), and returns the rendered content of that file.

An example of an typical `include` statement looks like this:

```php
{% include 'header.html' %}
```

Include statements accept a path parameter to the file you want to include.

## Includes in context of Drupal 8 components

In a Drupal 8 theme includes are mainly used to include or nest components into other components or templates. This is powerful because by including a component we avoid duplicating code. As we saw in the Hero component, we were able to include previously built components and lavarage all the work we did with those components without repeating ourselves. Because the Heading component was built in such a way, they still provide a mechanism for us to change them while being included so they meet our needs per use case.

### Let's review the Heading include in the Hero

{% tabs %}
{% tab title="hero.twig" %}
```php
{%
  include '@training_theme/heading/heading.twig' with {
    "heading": heading
  } only
%}
```
{% endtab %}
{% endtabs %}

* The include statement above uses a relative path.  This was done by creating a theme namespace \(`training_theme`\), which is mapped to `src/patterns/components/`, then we complete the path by providing the template name for the component.  More on namespaces later.
* The keyword `with` allows to pass additional variable to an include \(i.e. `heading`\)
* The keyword `only` restricts access to the context of the component.  For example, if a component has multiple variable we can opt to only pass some of them in the include.

Being able to customize components during the "include" process is a must. There are two ways we can change variables, properties, or values of a component.  Let's use the Hero as an example:

1. In `hero.json` we could change the `heading_level`, `modifier`, `title`, or `url` to the values we need. 
2. We could make the changes directly in the `include` as follows:

{% tabs %}
{% tab title="hero.twig" %}
```php
{%
  include '@training_theme/heading/heading.twig' with {
    "heading": {
      "title": "A new title here",
      "modifier": 'some-css-class',
      "url": "http://example.com",
      "heading_level": "3"
    }
  } only
%}
```
{% endtab %}
{% endtabs %}

Being able to reuse components while still having the options to change them on the fly to meet our needs is an incredible advantage of twig includes. We will use includes throughout this training to reduce code duplication.

{% hint style="info" %}
Learn more about [Twig includes](https://twig.symfony.com/doc/3.x/tags/include.html)
{% endhint %}

