# ARIA Attribute Lowdown:

- aria attributes are often used to work around non-semantic HTML. They're used like so: aria-attribute={value}
- aria attributes provide contextual information for users with accessibility needs.
- If you're using semantic HTML4 elements you shouldn't need to put aria attributes on them except in some browser-specific edge cases.
- If you're using semantic HTML5 elements sometimes you'll need to put aria attributes on them depending on browser/VO support for the element.

## aria-activedescendant

What it does: marks the active element in a widget made from different HTML elements.

Use it when: children within the widget are not natively focusable or their order cannot be inferred from the DOM.

### Notes

- For an element with DOM focus using this attribute, the following must be true: 
	- The value of aria-activedescendant refers to an element that is either a descendant of the element with DOM focus or is a logical descendant as indicated by the aria-owns attribute. 
	- The element with DOM focus is a textbox with aria-controls referring to an element that supports aria-activedescendant, and the value of aria-activedescendant specified for the textbox refers to either a descendant of the element controlled by the textbox or is a logical descendant of that controlled element as indicated by the aria-owns attribute. For example, in a combobox, focus may remain on the textbox while the value of aria-activedescendant on the textbox element refers to a descendant of a popup listbox that is controlled by the textbox.

### aria-activedescendant on an example widget

```
<div role="toolbar" tabindex="0" aria-activedescendant="button1">
<img src="/buttoncut.png" alt="cut" role="button" id="button1">
<img src="/buttoncopy.png" alt="copy" role="button" id="button2">
<img src="/buttonpaste.png" alt="paste" role="button" id="button3">
</div>
```

### aria-activedescendant and aria-owns on an example widget

```
<input type="text" aria-activedescendant="cb1-opt6" aria-readonly="true"
aria-owns="cb1-list" aria-autocomplete="list" role="combobox" id="cb1-edit">
<ul aria-expanded="true" role="listbox" id="cb1-list">
<li role="option" id="cb1-opt1">Alabama</li>
<li role="option" id="cb1-opt2">Alaska</li>
<li role="option" id="cb1-opt3">American Samoa</li>
<li role="option" id="cb1-opt4">Arizona</li>
<li role="option" id="cb1-opt5">Arkansas</li>
<li role="option" id="cb1-opt6">California</li>
<li role="option" id="cb1-opt7">Colorado</li>
</ul>
```

## aria-describedby

Use it when: you need to provide supplementary information for elements that are described by a label (or nonsemantic equivalent of one.)

### aria-describedby supplementary text associated with input field

```
<label for="donation">Donation Amount</label>
<input aria-describedby="donationDesc">
<p id="donationDesc">Amount must be at least $10.</p>
```

### aria-labelledby + aria-describedby

```
<div aria-labelledby="calendar" aria-describedby="info">
    <h1 id="calendar">Calendar</h1>
    <p id="info">
        This calendar shows the game schedule for the Boston Red Sox.
    </p>
</div>
```

## aria-disabled

Use it when: you need to announce to a screenreader that an input field is currently inactive.

### Notes:

This aria-attribute does not affect the interactivity of HTML elements; set those attributes with javascript as needed.

### aria-disabled + aria-live HTML example:

```
<form>
    <div>
        <label for="email">Your e-mail</label>
        <input type="email" id="email">
    </div>
    <div>
        <label for="terms">You agree to our terms and policy</label>
        <input type="checkbox" id="terms">
    </div>
    <button  aria-disabled="true">Submit</button>
    <p class="visually-hidden" aria-live="assertive"></p>
</form>
```

### aria-disabled + aria-live JS example:

```
<script>
(function() {
    var form = document.querySelector('form');
    var checkbox = form.querySelector('input[type="checkbox"]');
    var allowed = false;

  form.addEventListener('submit', function(event) {
        if (allowed) {
            console.log('you may pass');
            event.preventDefault(); // needs to be removed in the real world
        }

        if (!allowed) {
            console.log('you shall not pass');
            event.preventDefault();
        }
  });

    function toggleButton() {
        var nonvisualHint = form.querySelector('.visually-hidden');
      var submitButton = form.querySelector('button');
      if (allowed) {
        submitButton.setAttribute('aria-disabled', 'false');
        nonvisualHint.textContent = 'Please hit the submit button to subscribe to our newsletter.';
      } else {
        submitButton.setAttribute('aria-disabled', 'true');
        nonvisualHint.textContent = 'Please check the checkbox and agree to our terms to send this form.';
      }
    }

    toggleButton();
    checkbox.addEventListener('change', function() {
        allowed = checkbox.checked;
    toggleButton();
    });
})();
</script>
```

### References:

- [https://a11y-101.com/development/aria-disabled]

## aria-expanded={true|false}

Use it when: a menu item within an element that has `role="menu"` set can be expanded or collapsed.

### Notes:

- Set this attribute to `aria-expanded="true"` or `aria-expanded="false"` in accordance with the UI's visual state.
- Menu items that can launch another menu must include `aria-haspopup="true"`.

### aria-expanded + role="menuitem"

```
<li role="menuitem" aria-expanded="true"></li>
```

## aria-hidden={true|false}

What it does: if set to true, an element and its children aren't exposed to the accessibility API. Reversed for aria-hidden="false".

Use it when: you don't want visually hidden but still-existent UI elements to be detected by screenreaders (for example, you need to hide unfocused page elements following a popup dialog.) Conversely, use it when previously hidden UI elements are revealed.

### Notes

- Use `aria-hidden="true"` for visible elements if they are decorative or duplicative to equally accessible content.
- Don't use aria-hidden="true" or role="presentation" to hide img elements. Instead, set their alt attribute to an empty string (i.e., `<img alt="">`).

### aria-hidden="true" example

```
<p aria-hidden="true">
  Hello world.
</p>
```

### aria-hidden - HTML equivalents

```
<p hidden>Hello world.</p>
<p style="display: none;">Hello world.</p>
<p style="visibility: hidden;">Hello world.</p>
```

### aria-hidden - visible decorative element hidden (the link/text conveys context)

```
<a href="/">
  <span class="font-icon font-icon--home" aria-hidden="true"></span>
  Home
</a>
```

### References

- [https://www.scottohara.me/blog/2018/05/05/hidden-vs-none.html]

## aria-label

What it does: reads its value to a screen reader.

Use it when: you need to provide context for HTML elements (things like - navigation menus, nondecorative images, and input types that don't contain text.)

### aria-label to supply value to an empty button

```
<button id="pianoKey1" aria-label="Piano Key 1"></button>
```

## aria-labelledby

What is does: combines the values of all specified ids (starting with the first) into an aria label.

Use it when: you need to gives context to multiple, grouped inputs described by a label (or nonsemantic equivalent of one.)

### Notes

- One of the benefits of using the semantic HTML equivalent over `aria-labelledby` is clicking on the label with an id will focus on its associated control.

### aria-labelledby using a single label

```
<p id="selectSlam">Select a Slam or Jam</p>
<select aria-labelledby="selectSlam"><option>Slam</option><option>Jam</option></select>
```

### aria-labelledby using a single label (semantic HTML equivalent)

```
<label for="selectSlamSemantic">Select a Slam or Jam</label>
<select id="selectSlamSemantic"><option>Slam</option><option>Jam</option></select>
```

### aria-labelledby using multiple ids (accessible label becomes "Amount: 10 dollars Confirm")

```
<p id="l1">Amount: 10 dollars</p>
<div aria-labelledby="l1 b1">
	<button id="b1">Confirm</button>
</div>
```

## aria-invalid

What it does: reads that a form input is invalid

Use it when: you need to indicate to a screenreader than an input field contains erroneous content

### Notes

Use aria-describedby with any labels that describe the error that occured after form submits (per typical form workflows)

### aria-invalid with example form and error messages

```
<form>
<div>
<p><label for="pin">Personal Id. Number (PIN) - 4 digits: [*]</label> 
<input type="text" size="4" name="pin" id="pin" aria-describedby="err_0"><span id="err_0">Error: Input data missing</span></p>
</div>
<input type="submit" value="Login" id="login_btn" name="login_btn">
</form>
```

### References

- [https://www.w3.org/WAI/WCAG21/working-examples/aria-invalid-data-format/]

## aria-required

What it does: reads that a form field is required.

Use it when: you need to announce required form fields to a screenreader.

### aria-required example

```
<form action="#" method="post"  id="login1" onsubmit="return errorCheck1()">
  <p>Note: [*]denotes mandatory field</p>
  <p>
    <label for="usrname">Login name: </label>
    <input type="text" name="usrname" id="usrname" aria-required="true"/>[*]
  </p>
  <p>
    <label for="pwd">Password</label>
    <input type="password" name="pwd" id="pwd" size="12" aria-required="true" />[*]
  </p>
  <p>
    <input type="submit" value="Login" id="next_btn" name="next_btn"/>
  </p>

</form>
```

### References

- [https://www.w3.org/TR/WCAG20-TECHS/ARIA2.html]


