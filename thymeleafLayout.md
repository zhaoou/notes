# thymeleaf layouts


Two things that I found useful with thymeleaf:

1) You can include any template you want using `th:include`

    <div th:include="_style"> style </div>

`_style` is a relative path to the template to be included here.

2) To use decorator approach (where you provide a template and indicate which 'wrapper' should wrap it) we can use the layout dialect.

To achieve that, we need to **add the following to the decorator:**

    <div layout:fragment="content"></div>
This marks this location as point of insertion for a fragment.

And **wrap the fragment that we want to decorate** (insert into the layout) in some other template:

    <html layout:decorator="producer/base">
      <section layout:fragment="content">
        This will go inside of the decorator

But the best thing about thymeleaf, is that we can use both of these layout approaches.

[Here is a great resource!](http://www.thymeleaf.org/doc/articles/layouts.html)
