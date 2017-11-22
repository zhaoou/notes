# thymeleaf

I am in the process of learning thymeleaf, so far it has been great.
Here are some of the important areas of building a form in thymeleaf:


We should provide an object in the controller, and indicate that this form is for that object:

    <form method="post" action="save.html" th:object="${customer}">

Hidden id field allows us to decide if we create a new instance or edit an existing one:

    <input type="hidden" th:field="*{id}" />

For each of the fields in the object, we can provide an html input:

    <input type="text" th:field="*{firstName}" />


**To create a radio button,** we need a model attribute with possible values. Often a collection of objects. In this case, the collection of values are genders. We need to do the following:

  * Iterate on all genders
  * Create an input field for the current gender, for the gender field of the model

Here is an example:

    <div class="radio" th:each="gender : ${genders}">
        <input type="radio" th:value="${gender}" th:field="*{gender}"/>
        <label th:for="${#ids.prev('gender')}"
               th:text="${gender.description}"> Male </label>
    </div>


**To create a select html field,** we also need a model attribute, collection of possible payment methods in this case. We need to:

  * Select command object's field: paymentMethod
  * Iterate over all possible values and add each one as an option

Here is an example:

    <select th:field="*{paymentMethod}">
        <option th:each="pm : ${paymentMethods}"
                th:value="${pm}"
                th:text="${pm.description}"> Credit card </option>
    </select>

These tricks are very useful when building a form with thymeleaf.
