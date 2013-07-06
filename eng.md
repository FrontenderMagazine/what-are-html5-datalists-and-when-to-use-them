# What are HTML5 Datalists and When to Use Them

Autocompletion is a pattern that all Web users are familiar with. When you do a search, your search engine suggests terms. When you type a new e-mail message, your mail client suggest recipients. This functionality, however, has not been available to Web developers without a nontrivial amount of JavaScript. One of the new HTML5 elements, the `<datalist>`, brings this autocomplete functionality to the Web natively.

In this article, I’ll describe what datalists are, when it’s appropriate to use them, their limitations and what to do for browsers that don’t support them. Let’s get started.

## Creating Datalists

To show how a datalist works, let’s start with a traditional text input:

    <label for="favorite_team">Favorite Team:</label>
    <input type="text" name="team" id="favorite_team">

This field asks a user to provide his or her favorite sports team. By default, the user will be given no additional help to complete the field. But by using a datalist, you can provide a list of options the user can select from to complete the field. To do this, define a datalist with an option element for each suggestion:

    <datalist>
      <option>Detroit Lions</option>
      <option>Detroit Pistons</option>
      <option>Detroit Red Wings</option>
      <option>Detroit Tigers</option>
      <!-- etc... -->
    </datalist>

To tie a datalist to an input element, give the input element a list attribute and the datalist an id attribute that match. Here’s an example:

    <label for="favorite_team">Favorite Team:</label>
    <input type="text" name="team" id="favorite_team" list="team_list">
    <datalist id="team_list">
      <option>Detroit Lions</option>
      <option>Detroit Pistons</option>
      <option>Detroit Red Wings</option>
      <option>Detroit Tigers</option>
      <!-- etc... -->
    </datalist>

Notice that the list attribute of the input and the id attribute of the datalist contain the same value, “team_list”. This provides the link between the two.

That’s it. No JavaScript is required to make a datalist work. Figure 1 shows what the user will see in supporting browsers after typing a D.

![Figure1][Datalist Display]

Figure 1. Datalist Display (top left: Internet Explorer 10; top right: Firefox 18; bottom left: Chrome 24; bottom right: Opera 12)

Note: Internet Explorer 10 and Opera do not require the user to type a character before seeing suggestions, whereas Firefox and Chrome do.

Option elements can also have a value attribute. This is useful when a user might not know the code associated with a given option. Consider the following U.S. state input:

    <label for="state">State:</label>
    <input type="text" name="state" id="state" list="state_list">
    <datalist id="state_list">
      <option value="AL">Alabama</option>
      <option value="AK">Alaska</option>
      <option value="AZ">Arizona</option>
      <option value="AR">Arkansas</option>
      <!-- etc -->
    </datalist>

Here, the user will see a list of full state names, but when the user makes a selection, the text input will be filled with the state’s code rather than the full name. An example is shown in Figure 2.

![Figure2][Selecting a Datalist Option with a Corresponding Value]

Figure 2. Selecting a Datalist Option with a Corresponding Value (Firefox 18)

## The autocomplete Attribute

If the datalists in Figure 1 and Figure 2 look familiar, it’s because browsers have had autocompletion implemented for a long time. All browsers have some mechanism of storing a user’s inputs to be used for autocompletion later.

Authors can use the autocomplete attribute to control whether the browser should attempt to automatically complete the user’s data. The possible values are shown in the following example:

    <form>
      <!--
      If the autocomplete attribute is not specified, it inherits the value
      of its parent form element. If not specified, the default value of the
      form's autocomplete attribute is "on".  Since neither this input nor its
      form have the attribute specified, the "on" state will be used.
      -->
      <input type="text" name="firstName">
      <!--
     The "on" state indicates that the browser can remember the value for future
      use as well as suggest previously stored values.
      -->
      <input type="text" name="address" autocomplete="on">
      <!--
      The "off" state tells the browser neither to store the value entered for
      this input nor to suggest previously entered values. This should be
      used if the data is sensitive or the value will never be reused.
      -->
      <input type="text" name="secret" autocomplete="off">
    </form>

So what’s the difference between the autocomplete attribute and datalists? The autocomplete attribute tells the browser whether to give a user options for completion based on previous input and whether to store the entered value for future completion. Datalists are author-provided lists of suggestions that are always displayed regardless of previous input.

One caveat is that setting the autocomplete attribute to “off” prevents datalists from displaying in Opera. Here’s an example:

    <!--
    This datalist will never display in Opera because the autocomplete attribute
    is set to "off". It will display in all other supporting browsers.
    -->
    <input type="text" list="pets" autocomplete="off">
    <datalist id="pets">
      <option>Cat</option>
      <option>Dog</option>
      <option>Hamster</option>
      <option>Turtle</option>
    </datalist>

## Other Input Types

While autocompletion is traditionally associated with textual input, datalists can also be used on a number of the new HTML5 input types. Consider the range input type, which allows for the creation of a slider form element. By combining this with a datalist, you can suggest points on the range to the user.

For example, the following input asks the user to provide a donation between $5 and $200 dollars.

    <label for="donation">Donation Amount (USD):</label>
    <input type="range" name="donation" id="donation" list="donation_list"
      step="5" min="5" max="200">
    <datalist id="donation_list">
      <option>25</option>
      <option>50</option>
      <option>100</option>
      <option>200</option>
    </datalist>

Figure 3 and Figure 4 show the display of a range input in Chrome 24 and Internet Explorer 10, respectively.

![Figure3][Range Input with Datalist in Chrome 24]

Figure 3. Range Input with Datalist (Chrome 24)

![Figure4][Range Input with Datalist in Internet Explorer 10]

Figure 4. Range Input with Datalist (Internet Explorer 10)

You can see that each browser displays a tick mark for each datalist option provided. Additionally, Chrome will snap the slider to these predefined values as the user moves the slider near them.

Unfortunately, Internet Explorer and Chrome are the only browsers to support datalists on range inputs at this time. Figure 5 shows support of datalists on common input types in modern browsers.

![Figure5][Browser Support of Datalists on Form Input Types]

Figure 5. Browser Support of Datalists on Form Input Types

## When to Use a Datalist

Since datalists have no built-in mechanism to require that a user select a provided option, they are well suited for inputs that can accept any value. The earlier example of providing a sports team fits here. While the datalist suggested teams, the user was free to input any value.

Conversely, the U.S. state example fails this test because there is a limited set of valid values that the user must provide. Such a use case is better handled by the select element because it forces a selection.

One exception to this is inputs with a large number of valid selections. For example, consider the traditional country drop-down list shown in Figure 6.

![Figure6][Standard Country Drop-Down]

Figure 6. Standard Country Drop-Down

This list impedes users by forcing them to sift through hundreds of options to find the one they’re looking for. An autocomplete feature fits this use case well because a user can quickly filter the list by typing one or two characters.

Here’s how country selection can be implemented with a datalist:

    <label for="country">Country:</label>
    <input type="text" id="country" list="country_list">
    <datalist id="country_list">
      <option value="AF">Afghanistan</option>
      <option value="AX">Åland Islands</option>
      <option value="AL">Albania</option>
      <option value="DZ">Algeria</option>
      <!-- more -->
    </datalist>

Figure 7 shows what the user will see after typing a U.

![Figure7][A Datalist Approach to the Country Form Field]

Figure 7. A Datalist Approach to the Country Form Field

## Enforcing a Value

While datalists do not natively allow you to require that an option be selected, you can easily add validation that does so. For example, the following code makes use of the [HTML5 constraint validation API][1] to add such validation:

    // Find all inputs on the DOM which are bound to a datalist via their list attribute.
    var inputs = document.querySelectorAll('input[list]');
    for (var i = 0; i < inputs.length; i++) {
      // When the value of the input changes...
      inputs[i].addEventListener('change', function() {
        var optionFound = false,
          datalist = this.list;
        // Determine whether an option exists with the current value of the input.
        for (var j = 0; j < datalist.options.length; j++) {
            if (this.value == datalist.options[j].value) {
                optionFound = true;
                break;
            }
        }
        // use the setCustomValidity function of the Validation API
        // to provide an user feedback if the value does not exist in the datalist
        if (optionFound) {
          this.setCustomValidity('');
        } else {
          this.setCustomValidity('Please select a valid value.');
        }
      });
    }

The constraint validation API is implemented in all browsers that support datalists, so if the datalist works, the validation should work as well. Now, when the user attempts to submit a form with an input that has a datalist (and they have not selected an option), they will see the error shown in Figure 8.

![Figure8][Constraint Validation API Error]

Figure 8. Constraint Validation API Error (Internet Explorer 10)

It’s important to note that the constraint validation API does not remove the need for server-side validation. Users working with older browsers will not have the constraint validation API available, and malicious users can easily subvert client-side scripts.

While this approach works in modern browsers, it presents an unusable UI to users running browsers that lack support. Users are told that they must select a valid value, but if their browser doesn’t support datalists, they cannot see the options. Therefore, if you plan on using this approach, it’s essential to provide a UI that works in all browsers. This can be accomplished by detecting whether the browser supports datalists and then polyfilling appropriately.

## Unsupporting Browsers

At the time of this writing, datalists for text inputs are supported in Internet Explorer 10, Firefox 4+, Chrome 20+, and Opera, which unfortunately leaves out a large number of users.

Unlike with many new HTML5 features, for most use cases, no extra work needs to be done in browsers that do not support datalists. By default, the list of options you provide are merely suggestions; therefore, users with browsers that lack support will simply need to fill in the text field without any suggestions.

However, some fallback options can be used to provide a fuller experience to users running unsupporting browsers.

## Fallback to Alternate HTML Content

One option, [popularized by Jeremy Keith][2], is to take advantage of the fact that browsers that do not support the datalist element will still display child elements to the user. The following shows how the country datalist example can be altered and fallback to using `<select>`

    <label for="country">Country:</label>
    <datalist id="country_list">
      <select name="country">
        <option value="AF">Afghanistan</option>
        <option value="AX">Åland Islands</option>
        <option value="AL">Albania</option>
        <option value="DZ">Algeria</option>
        <option value="AS">American Samoa</option>
        <!-- more -->
      </select>
      If other, please specify:
    </datalist>
    <input type="text" name=”country” id=”country” list="country_list">

The UI presented to users in browsers that support datalists will not change, but users working with browsers without support now see a select element with the country options and a text box they can use to enter any value. This is shown in Figure 9.

![Figure9][A Select Fallback for Datalists]

Figure 9. A Select Fallback for Datalists (Internet Explorer 9)

## Polyfilling

One feature the select fallback doesn’t provide is the autocomplete behavior that datalists offer natively. If that is important for the datalists you’re adding, a second fallback option is to polyfill a datalist implementation.

To start, you first need to determine whether the user’s browser supports datalists. The popular [feature detection][3] library [Modernizr][4] provides such a test, as shown here:

    if (Modernizr.input.list) {
        //The browser supports the list attribute and datalists.
      } else {
        //The browser supports neither the list attribute nor datalists.
      }
    }

Using this approach, you can now polyfill a datalist implementation for users in unsupporting browsers. While [several polyfills][5] are available for datalists, I prefer using jQuery UI’s autocomplete widget. The following code shows a polyfill implementation:

    var datalist,
      listAttribute,
      options = [];
    // If the browser doesn't support the list attribute...
    if (!Modernizr.input.list) {
      // For each text input with a list attribute...
      $('input[type="text"][list]').each(function() {
        // Find the id of the datalist on the input
        // Using this, find the datalist that corresponds to the input.
        listAttribute = $(this).attr('list');
        datalist = $('#' + listAttribute);
        // If the input has a corresponding datalist element...
        if (datalist.length > 0) {
          options = [];
          // Build up the list of options to pass to the autocomplete widget.
          datalist.find('option').each(function() {
            options.push({ label: this.innerHTML, value: this.value });
          });
          // Transform the input into a jQuery UI autocomplete widget.
          $(this).autocomplete({ source: options });
        }
      });
      // Remove all datalists from the DOM so they do not display.
      $('datalist').remove();
    }

Figure 10 shows the display of the country list example in Safari with the jQuery UI autocomplete polyfill.

![Figure10][Country Datalist Polyfilled Using jQuery UI’s Autocomplete Widget]

Figure 10. Country Datalist Polyfilled Using jQuery UI’s Autocomplete Widget(Safari)

You may have noticed that by default, jQuery UI’s autocomplete widget matches characters anywhere in the options, whereas datalists match options only at the beginning of the string. Unlike the native datalist, the autocomplete widget allows you to customize this to match options however you’d like.

The following example shows how you can build an autocomplete feature that matches options only at the beginning of the string:

    <input type="text" id="autocomplete">
    <script>
    var options = ['Badger', 'Bat', 'Cat', 'Cheetah', 'Crocodile', 'Deer', 'Donkey',
      'Elephant', 'Fish', 'Frog', 'Giraffe', 'Gorilla'];
    $('#autocomplete').autocomplete({
      // req will contain an object with a "term" property that contains the value
      // currently in the text input.  responseFn should be invoked with the options
      // to display to the user.
      source: function (req, responseFn) {
        // Escape any RegExp meaningful characters such as ".", or "^" from the
        // keyed term.
        var term = $.ui.autocomplete.escapeRegex(req.term),
          // '^' is the RegExp character to match at the beginning of the string.
          // 'i' tells the RegExp to do a case insensitive match.
          matcher = new RegExp('^' + term, 'i'),
          // Loop over the options and selects only the ones that match the RegExp.
          matches = $.grep(options, function (item) {
            return matcher.test(item);
          });
        // Return the matched options.
        responseFn(matches);
      }
    });
    </script>

## Older Versions of Internet Explorer

To make any polyfill of the datalist element work in older versions of Internet Explorer, you need to take two extra steps.

## Option Elements

The first is that Internet Explorer version 9 and earlier all require that option elements be immediate children of select elements. If they are not, these versions do not recognize them, and they will not be visible to the polyfill.

Luckily, this is easy to work around. By using conditional comments, you can place a select element around the options only for these versions of Internet Explorer. Refer to this example:

    <datalist>
      <!--[if IE]><select><!--<![endif]-->
      <option>Apple</option>
      <option>Banana</option>
      <option>Cherry</option>
      <!- etc -->
      <!--[if IE]><select><!--<![endif]-->
    </datalist>

Internet Explorer version 9 and earlier will now detect the options appropriately. Internet Explorer 10 will not be affected since conditional comments were removed from the parser in Internet Explorer 10. All other browsers will also ignore the comments.

## HTML5 Shiv

In Internet Explorer version 8 and earlier, unknown elements cannot contain children. Therefore, even if a datalist’s option elements are within select elements, they will still not be recognized.

Fortunately, there’s an easy fix for this issue as well. The popular [HTML5 Shiv][6] library allows elements unknown to Internet Explorer 6–8 to have children. This shiv is included by default in Modernizr; just be sure the “html5shiv” check box is selected when you configure your download.

## Limitations of Datalists

Although datalists are perfect for adding suggestions to text inputs, they suffer from a lack of customization capabilities that many modern Web applications require. For example, you cannot do the following with datalists:

Use CSS to change its display or the display of its options.
Control the positioning of the list.
Control the number of characters typed before the browser displays results to the user.
Control what is considered a match (case sensitivity, match at beginning of string vs. anywhere, and so on).
Tie the input to a server-side datasource.

This means that if you need any of this functionality, you need to look into a more robust autocomplete solution. The jQuery UI autocomplete widget provides the ability to do all of this and more. The autocomplete widget also supports all browsers back to Internet Explorer 7 without the need of a polyfill. For more information on the autocomplete widget, check out its [demos][7] and API documentation. (The autocomplete widget handles only text-based inputs, therefore it won’t be able to help you for some of the more specialized input types like range and color.)

## Wrapping Up

Datalists provide a quick, native means of displaying input suggestions to the user. Since the options are merely suggestions, for many situations it’s not necessary to provide a fallback for unsupporting browsers; users of these browsers simply will not see the suggestions.

For situations when you do want to provide a functional datalist to all users, you can detect support and polyfill the functionality for browsers that lack support.

While datalists are great for offering suggestions, they are limited in the functionality they provide. If you need a more robust autocomplete solution, jQuery UI’s autocomplete widget is a good place to get started.

[1]: http://msdn.microsoft.com/en-us/library/ie/hh673544%28v=vs.85%29.aspx#dom_methods_input_val
[2]: http://adactio.com/journal/4272/
[3]: http://msdn.microsoft.com/en-us/magazine/hh394148.aspx
[4]: http://modernizr.com/
[5]: https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills
[6]: http://modernizr.com/docs/#html5inie
[7]: http://jqueryui.com/autocomplete/

[Datalist Display]: img/figure1.jpg
[Selecting a Datalist Option with a Corresponding Value]: img/figure2.png
[Range Input with Datalist in Chrome 24]: img/figure3.png
[Range Input with Datalist in Internet Explorer 10]: img/figure4.png
[Browser Support of Datalists on Form Input Types]: img/figure5.png
[Standard Country Drop-Down]: img/figure6.png
[A Datalist Approach to the Country Form Field]: img/figure7.png
[Constraint Validation API Error]: img/figure8.png
[A Select Fallback for Datalists]: img/figure9.png
[Country Datalist Polyfilled Using jQuery UI’s Autocomplete Widget]: img/figure10.png