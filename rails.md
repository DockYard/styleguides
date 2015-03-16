## Views

* If in need of a view layer, use ERB templating over HAML.

* Use double quotes when dealing with HTML or string interpolation.

* Never make complex formatting in the views, export the formatting to a
  method in the view helper or the model.

* Mitigate code duplication by using partial templates and layouts.
