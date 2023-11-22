+++
archetype = "default"
title = "Contact"
weight = 100
+++


Contact form:

{{< rawhtml >}}
<head>
    <script src="https://www.google.com/recaptcha/api.js" async defer></script>
    <script>
      function getQueryParam(name) {
        const urlParams = new URLSearchParams(window.location.search);
        return urlParams.get(name);
      }
      function createPopup(message) {
        alert(message);
      }
    function onPageLoad() {
      const popupValue = getQueryParam('result');
      if (popupValue !== null && popupValue==="error") {
        createPopup('Something went wrong. Please try again.');
      }
      if (popupValue !== null && popupValue==="success") {
        createPopup('Message sent successfully.');
      }
    }
     window.onload = onPageLoad;
</script>
</head>

<form
	action="https://formie.io/form/1935388c-7d35-4e42-a837-badca86c8aaa"
	method="POST"
> 
    <input name="_redirect" type="hidden" value="https://hirobytes.xyz/contact?result=success">
    <input name="_redirect_error" type="hidden" value="https://hirobytes.xyz/contact?result=error">

  <div class="mb-3 pt-0">
    <input type="text" placeholder="Your name" name="name" required />
  </div>
  <div class="mb-3 pt-0">
    <input type="email" placeholder="Email" name="email" required />
  </div>
  <div class="mb-3 pt-0">
    <textarea placeholder="Your message" name="message" required></textarea>
  </div>
  <div class="g-recaptcha" data-sitekey="6LeIRRkpAAAAAELO5I7lk1Z8P1PQbryemvz_H4F4"></div>
      <br/>
  <div class="mb-3 pt-0">
    <button type="submit">Send message</button>
  </div>

</form>

{{< /rawhtml >}}


 