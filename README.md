# constr



![](https://github.com/blessing-michael/constr/blob/main/images/Didy%20Java%20sketch%20(1).svg)

## **Describing How to Use Constraint Validation API to Validate Forms on the Frontend**

### A Beginner's Guide to Constraint Validation API.

Almost every major website require forms. There are forms for signing up, contacting, searching, and more! the browsers have built-in validation based on those HTML attributes like required and maxlength etc. We can leave everything to the browser for basic validation or we can build our own unique JS validation. But wouldn't it be nice to use our script and tap into the browser's mechanism for validation? Yes, we can, courtesy of the Constraint Validation API.
In this article, we will discuss:

* How Constraint Validation API works. 
* How we can have more control over our forms using Constraint Validation API.
* Add validation using only Javascript.

### **Prerequisites**
* Basic knowledge of HTML and CSS.
* Basic knowledge of Javascript.
* A code editor(VS code recommended).

### **How Constraint Validation API works**
The Constraint Validation API can be used on several HTML form elements.

*  button element
*  fieldset element
*  input element
*  output element
*  select element
*  textarea element

Now, we will practice using The Constraint Validation API on HTML Input Element. Below is an image that shows an input field.


![input field](Didy%20Java%20sketch%20(1).svg)

### **Creating a Basic HTML/CSS Page**
let's create a simple form with HTML.

```HTML
<body>
    <form action="" id="form" novalidate>
      <div class="form1">
        <label for="name" id="name-label" class="label">Name</label>
        <input
          required
          type="text"
          id="name"
          class="name"
          placeholder="Enter your Name"
        />
      </div>

      <div class="form1">
        <label for="email" id="email-label" class="label">Email</label>
        <input
          type="email"
          id="email"
          class="name"
          placeholder="Enter your Email"
          required
        />

        <span class="error-Message" aria-live="polite"></span>
      </div>

      <div class="country">
        <label for="country" class="label">Country</label>
        <input
          minlength="2"
          id="country"
          class="name"
          placeholder="Enter your country's code"
          required
        />

        <span class="error-Message" aria-live="polite"></span>
      </div>

      <button type="submit" id="submit">submit</button>
    </form>
    <script src="index.js"></script>
  </body>

```
* <mark style="background-color:#F9F6EE">novalidate</mark>: The novalidate attribute as seen in the form is used to disable the browser's built-in validation, giving our script control over the validation process.
* <mark style="background-color:#F9F6EE">```<span class="error-message" aria-live="polite"></span>```</mark>: The span element is for displaying the error messages.
* <mark style="background-color:#F9F6EE">aria-live</mark>: The attribute is set to the ```<span>``` to ensure that our personalized error message is displayed to everyone.

**CSS**

Now for the CSS, let's add styles to the invalid and valid fields. border of  1px solid green; if valid and  border of 1px solid rgb(99, 97, 97); if invalid. We will style our <mark style="background-color:#F9F6EE">span class</mark> which is responsible for displaying the error message with the background color of red, and display of none. Lastly, we will create a class of active with a display block for the span class.

```CSS
body {
  display: flex;
  flex-direction: column;
  background: linear-gradient(
    90deg,
    rgba(100, 100, 280, 0.5),
    rgba(107, 107, 189, 0.9)
  );
  background-size: cover;
  background-position: center;
  justify-content: center;
  align-items: center;
  font-size: 20px;
}

form {
  margin: 0 auto;
  background-color: whitesmoke;
  padding: 40px;
  width: 400px;
  height: 400px;
  border-radius: 5px;
  box-shadow: 2px 4px 4px rgba(0, 0, 0, 0.3);
}

.form1 {
  display: flex;
  flex-direction: column;
  margin-bottom: 30px;
}
.name {
  width: 95%;
  height: 30px;
  border-radius: 5px;
}
#submit {
  width: 95%;
  margin-top: 40px;
  height: 40px;
  background-color: rgb(54, 52, 108);
  border: none;
  color: white;
  text-transform: capitalize;
  font-size: 15px;
  font-weight: bold;
  font-family: sans-serif;
  border-radius: 5px;
}
.label {
  margin-bottom: 5px;
}
input {
  border-radius: 4px;
  width: 100%;
  position: relative;
}
/* This will handle the valid and invalid field */
input:valid {
  border: 1px solid green;
}
input:invalid {
  border: 1px solid rgb(99, 97, 97);
  box-shadow: 2px 2px 8px rgba(99, 97, 97, 0.3);
}

/* validation css starts here */
.error-Message {
  width: 90%;
  font-size: 80%;
  background-color: rgb(230, 63, 63);
  border-radius: 0 0 5px 5px;
  height: 30px;
  padding: 0;
  display: none;
  color: white;
}
.error-Message.active {
  padding: 10px;
  display: block;
  transition: all 0.9s ease-in;
}
/* end of validation css */
```
The result 👇

![form image](/formpixnew.jpg)

Alright, now we are done with the basic form setup, let's dig into Javascript and discuss the Constraint Validation API.

**Putting the Constraint Validation API into Practice**

* <mark style="background-color:#F9F6EE">valueMissing</mark>: Returns true if a required attribute is present, but has no value, otherwise returns false. If true, the element matches the CSS pseudo-class :invalid
* <mark style="background-color:#F9F6EE">typeMismatch</mark>: If the value is less than the minimum length defined by the minlength attribute, it returns true, or false if it exceeds or is equal to the minlength. If true, the element matches the CSS pseudo-class :invalid
  
  Now let's validate the email's input field ```<input
          type="email"
          id="email"
          class="name"
          placeholder="Enter your Email"
          required
        />``` and  the Country's input field  ``` <input
          minlength="2"
          id="country"
          class="name"
          placeholder="Enter your country's code"
          required
        />```


Notice that both has the required attribute, but the Country's input field has minlength of 2 characters.

```javascript
const form = document.getElementById("form");
const email = document.getElementById("email");
const emailErrorMes = document.querySelector("#email +span.error-Message");

const country = document.getElementById("country");
const countryerror = document.querySelector("#country +span.error-Message");

email.addEventListener("input", (event) => {
  if (email.validity.valid) {
    emailErrorMes.textContent = "";
    emailErrorMes.className = "error-Message";
  } else {
    displayError();
  }
});
country.addEventListener("input", (event) => {
  if (country.validity.valid) {
    countryerror.textContent = "";
    countryerror.className = "error-Message";
  } else {
    displayError();
  }
});
```


we added an  event listener to the input field to know when there's a value in it or not.

* <mark style="background-color:#F9F6EE">if (email.validity.valid) {
    emailErrorMes.textContent = "";
    emailErrorMes.className = "error-Message"}</mark>:
       Here, the error message is removed if the field is valid. setting the textContent to an empty string resets the content of the input field.

  * <mark style="background-color:#F9F6EE">else {
    displayError();
  }</mark>:
  This line checks for when the field is invalid. we call the displayError function here to display an error message. We will create the displayError function in a minute.

Next, we add a submit  event listener to our form element. This event will fire on submitting the form and ensures that entries are valid.

  * <mark style="background-color:#F9F6EE"> if (!email.validity.valid) {
    displayError();
    event.preventDefault()}</mark>:
     If the email field is not valid, we call the displayError function to display our custom error message.
       * <mark style="background-color:#F9F6EE">event.preventDefault()</mark>: This cancels the event and prevents the form from being submitted

Now, here comes our displayError function


```javascript

function displayError() {
  if (email.validity.valueMissing) {
    emailErrorMes.textContent = "Please enter your email address";
  }
  else if (email.validity.typeMismatch) {
    emailErrorMes.textContent = "Entered value needs to match an email address";
  }
  emailErrorMes.className = "error-Message active";

  //validity for country
  if (document.activeElement == country) {
    if (country.validity.valueMissing) {
      countryerror.textContent = "Please enter your country";
    } else if (country.validity.tooShort) {
      countryerror.textContent = `Country code too short. Code should be at least ${country.minLength} characters.`;

      //   "Country name too short";
    }
    countryerror.className = "error-Message active";
  }
}
```

* <mark style="background-color:#F9F6EE">if(email.validity.valueMissing){emailErrorMes.textContent = "Please enter your email address"}</mark>: This line takes care of when the email input field is empty. When this happens our custom error message is displayed on the screen
  
  * <mark style="background-color:#F9F6EE">else if (email.validity.typeMismatch) {
    emailErrorMes.textContent = "Entered value needs to match an email address"}</mark>: Here the entered details in the email are not in the appropriate format of type email, again we display our custom message.

* <mark style="background-color:#F9F6EE"> emailErrorMes.className = "error-Message active"</mark>: "error-Message active" is our CSS class which sets the styling properly.

That's all, our webpage should now look like this.


<video src="https://youtu.be/xpfeNKEW6X8" autoplay loop controls></video>

Repeat the same process for the country's input field

This is the complete JavaScript code that we created.

```javascript
const form = document.getElementById("form");
const email = document.getElementById("email");
const emailErrorMes = document.querySelector("#email +span.error-Message");

const country = document.getElementById("country");
const countryerror = document.querySelector("#country +span.error-Message");

email.addEventListener("input", (event) => {
  if (email.validity.valid) {
    emailErrorMes.textContent = "";
    emailErrorMes.className = "error-Message";
  } else {
    displayError();
  }
});
country.addEventListener("input", (event) => {
  if (country.validity.valid) {
    countryerror.textContent = "";
    countryerror.className = "error-Message";
  } else {
    displayError();
  }
});

form.addEventListener("submit", (event) => {
  if (!email.validity.valid) {
    displayError();

    event.preventDefault();
  }
  if (!country.validity.valid) {
    displayError();

    event.preventDefault();
  }
});

function displayError() {
  if (email.validity.valueMissing) {
    emailErrorMes.textContent = "Please enter your email address";
  } else if (email.validity.typeMismatch) {
    emailErrorMes.textContent = "Entered value needs to match an email address";
  }
  emailErrorMes.className = "error-Message active";

  //validity for country
  if (document.activeElement == country) {
    if (country.validity.valueMissing) {
      countryerror.textContent = "Please enter your country";
    } else if (country.validity.tooShort) {
      countryerror.textContent = `Country code too short. Code should be at least ${country.minLength} characters.`;
    }
    countryerror.className = "error-Message active";
  }
}
```

- [Watch Demo](https://blessing-michael.github.io/Constraint-Validation-API/?)
- [Source code](https://github.com/blessing-michael/Constraint-Validation-API)

### **Conclusion**
Congrats!!! you have successfully customized your custom messages using Constraint Validation API but we did not cover other properties like patternMismatch, tooLong, rangeOverflow, and rangeUnderflow. Remember that you can do so many things with Constraint Validation API. You could add more strict validation to your form element if you want. Stay tuned for part 2 and more content on how we could apply these properties to other form element DOM interfaces


If you have any inquiries or spot any mistakes, kindly leave some feedback as I am open to reading and learning from you too.

### **References**
 - [MDN](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#validating_forms_using_javascript)
 - [W3schools.com](https://www.w3schools.com/js/js_validation_api.asp)
