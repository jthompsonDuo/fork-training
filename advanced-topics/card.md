# Card Component

A component that will introduce us to a more useful technique for working with components is the image you see below. A Card component is a great way to display all sorts of content \(news, blog posts, events, etc.\), and you will see it in many websites nowadays.  The card is not typically displayed on its own, although sometimes it is, but its most common use is as a collection of content.  For example, we could use a collection of cards to display latest blog posts or upcoming events.  In this training we will use the card component to build two content lists; Featured Content, and Blog Content.

![Example of a Card component](../.gitbook/assets/card.png)

Although we could build the content list components already as a collection of content, a better approach is to first build a single instance of a card component that then we can reuse over and over.  Having a single card component available makes it possible to even build other types of content collections.  

### Exercise:  Card component

As we've did with the Hero component, one of the first things we need to define are the data fields that makeup the component.  If you look at the image above, a single Card has the following data fields:

* image
* title \(link to full article\)
* date
* body
* & tags

Now that we have identified the fields our card component needs, let's start building it.

#### Component's stock content

1. Inside `src/patterns/components` create a new directory called **card**
2. Inside the _card_ directory create a new file called **card.json**
3. Inside _card.json_ add the following code:

{% tabs %}
{% tab title="card.json" %}
```yaml
{
  "image": "<img src='https://source.unsplash.com/BJrgqUKYx8M/640x360' alt='Women running' />",
  "title": {
    "heading_level": "2",
    "modifier": "card__title",
    "title": "Level up your game",
    "url": "#"
  },
  "date": "March 16 2020",
  "body_text": "Curabitur blandit tempus porttitor. Vestibulum id ligula porta felis euismod semper. Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Aenean lacinia bibendum nulla sed consectetur.",
  "tags": [
    {
      "text": "Photography",
      "url": "#"
    },
    {
      "text": "Sports",
      "url": "#"
    },
    {
      "text": "Outdors",
      "url": "#"
    }
  ],
  "modifier": ""
}
```
{% endtab %}
{% endtabs %}

By now we should be well familiar with the fields structure above.  The one field type we probably have not seen until now is an array.  In the code above we are declaring the tags as an array of items.  Each tag item inside the array has a `text` and `url` keys so they can become links to tag-driven pages on our site.

#### Component's markup

1. Inside the _card_ directory create a new file called **card.twig**
2. Inside _card.twig_ add the following code:

{% tabs %}
{% tab title="card.twig" %}
```php
{{ attach_library('training_theme/card') }}

<article class="card{{ modifier ? ' ' ~ modifier }}{{- attributes ? attributes.class -}}"
  {{- attributes ? attributes|without(class) -}}>
  {%  if image %}
    <div class="card__media">
      {{ image }}
    </div>
  {% endif %}
  {% if title or date or body_text or tags %}
    <div class="card__content">
      {% if title %}
        {%
          include '@training_theme/heading/heading.twig' with {
            heading: title
          } only
        %}
      {% endif %}
      {% if date %}
       <div class="card__date">
          {{ date }}
        </div>
      {% endif %}
      {% if body_text %}
        <p class="card__body">
          {{ body_text }}
        </p>
      {% endif %}
      {% if tags %}
        <ul class="card__tags">
          {% for item in tags %}
            <li class="card__tag--item">
              <a href="{{ item.url }}" class="card__tag--link">
                {{ item.text }}
              </a>
            </li>
          {% endfor %}
        </ul>
      {% endif %}
    </div>
  {% endif %}
</article>
```
{% endtab %}
{% endtabs %}

* Most things look pretty straight forward in the code above.  With the tags we loop through the `tags` array and then add each  tag item as a list item in the unordered list.

{% hint style="warning" %}
Don't forget to create the Card's library.
{% endhint %}

#### Component styles

1. Inside the _card_ directory create a new file called **card.scss**
2. Inside _card.scss_ add the following code:

{% tabs %}
{% tab title="card.scss" %}
```css
// Import site utilities
@import '../../global/utils/init';

.card {
  display: flex;
  flex-direction: column;
  position: relative;
  max-width: 420px;
  margin: 50px auto 20px;
  border-radius: 4px;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);

  img {
    display: block;
  }

  // ========== Card wide styles=========
  &.card--wide {
    box-shadow: none;
    border: 1px solid #ddd;
    flex-direction: column;

    .card__body-text {
      margin-bottom: 40px;
    }

    // Changes card layout on larger screens.
    @media screen and (min-width: 640px) {
      flex-direction: row;
      max-width: 720px;

      .card__media,
      .card__content {
        flex: 0 0 50%;
      }

      .card__content {
        padding: 40px 20px 20px 40px;
      }
    }
  }
}

.card__content {
  padding: 20px;
}

.card__title {
  margin-bottom: 8px;
  margin-top: 0;
  font-size: 24px;
  font-weight: 600;
}

.card__date {
  text-transform: uppercase;
  display: block;
  border-bottom: 1px solid #ccc;
  padding-bottom: 8px;
  letter-spacing: 2px;
}

.card__tags {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.card__tag--item {
  background-color: #edf2f7;
  border-radius: 99999px;
  color: #4a5568;
  display: inline-block;
  padding: 4px 10px;
  margin-right: 10px;
}

.card__tag--link {
  text-decoration: none;
  color: #1a202c;

  &:hover,
  &:focus {
    color: lighten(#1a202c, 25%);
  }
}

```
{% endtab %}
{% endtabs %}

#### Compiling the code

Now that the card component is done, let's compile the code so we can see it in Pattern Lab.  If the watch task is running you should be able to see the card component in Pattern Lab, otherwise follow these steps:

While in your theme's root directory, run the following commands in your command line and press **Return**

`npm run build`

`npm run watch`

In your browser of choice open the following URL: [http://localhost:3000](http://localhost:3000). This will open Pattern Lab where you can find the Card component under components.

#### Accessing Pattern Lab from within Drupal

When working with Pattern Lab locally it makes sense to use http://localhost:3000 to view your work. However, if you are viewing your project from a server during development, or want to show your work and progress to a stakeholder or client, this approach will not work.  Luckily for us, we can access Pattern Lab using Drupal's URL as follows:

`/themes/custom/training_theme/patternlab/index.html`

Two things to keep in mind with the path above:

1. The path above is appended to your Drupal's base URL.  For example, if your Drupal's address is **https://mc-training.dev.pantheon.io**, the full URL would become `https://mc-training.dev.pantheon.io/themes/custom/training_theme/patternlab/index.html`
2. Replace `training_theme` with your project's theme name if your theme name is different.

\`\`

