<html>
<body>
  <br>
  <center>
  <form class="gform pure-form pure-form-stacked" method="POST" data-email="support@appssky.com"
  action="https://script.google.com/macros/s/AKfycbx_PJy3qDvXPkByfPZcUQWxJMoRRs247SVU2jpp4HDQIjG_AJO6czn8MKlJIFDWlq7DtA/exec">
    <div class="form-elements"><br>
      <div style="background:none;border:8px solid green;border-radius: 30px;width: 340px;padding-top: 15px;padding-bottom: 30px;padding-left: 5px;padding-right: 5px;"><br>
        <b class="content-head" style="font-size: 2.0em;">*** สมัครสมาชิก ***</b>
        <br><br>
         <b style="margin-left: -85px;font-size: 1.3em;">ชื่อ - นามสกุล ผู้มาติดต่อ</b><br>
        <input type="text" name="name" required="" style="font-size: 1em;width: 300px;"/><br><br>

        <b style="margin-left: -255px;font-size: 1.3em;">อีเมล</b><br>
        <input type="email" name="email" required="" style="font-size: 1em;width: 300px;"/><br><br>

         <b style="margin-left: -210px;font-size: 1.3em;">เบอร์มือถือ</b><br>
         <input type="number"  name="phone" required="" style="font-size: 1em;width: 300px;"/><br><br>
      
        <b style="margin-left: -30px;font-size: 1.3em;">เลขทะเบียนนิติบุคคล 13 หลัก?</b><br>
        <textarea  name="message" rows="5" cols="22" required="" style="font-size: 1.3em;width: 300px;"></textarea><br><br>
    
        <button style="float: right;width: 100px;height: 40px;font-size: 1.5em;margin-right: 20px;">
        บันทึก</button><br>
      </div>
    </div>

    <!-- Thankyou_message --><br>
    <div class="thankyou_message" style="display:none;background:none;border:8px solid green;border-radius: 30px;width: 340px;padding-top: 15px;padding-bottom: 30px;padding-left: 5px;padding-right: 5px;"><br><br>
      <h1>ขอบคุณค่ะ!</h1>
      <h1>ยินดีให้บริการนะคะ</h1>
    </div>
  </form>
  <script data-cfasync="false" type="text/javascript">
    
   (function() {
  function validEmail(email) {
    var re = /^([\w-]+(?:\.[\w-]+)*)@((?:[\w-]+\.)*\w[\w-]{0,66})\.([a-z]{2,6}(?:\.[a-z]{2})?)$/i;
    return re.test(email);
  }

  function validateHuman(honeypot) {
    if (honeypot) {
      console.log("Robot Detected!");
      return true;
    } else {
      console.log("Welcome Human!");
    }
  }
  function getFormData(form) {
    var elements = form.elements;

    var fields = Object.keys(elements).filter(function(k) {
          return (elements[k].name !== "honeypot");
    }).map(function(k) {
      if(elements[k].name !== undefined) {
        return elements[k].name;
      }else if(elements[k].length > 0){
        return elements[k].item(0).name;
      }
    }).filter(function(item, pos, self) {
      return self.indexOf(item) == pos && item;
    });

    var formData = {};
    fields.forEach(function(name){
      var element = elements[name];
      formData[name] = element.value;
      if (element.length) {
        var data = [];
        for (var i = 0; i < element.length; i++) {
          var item = element.item(i);
          if (item.checked || item.selected) {
            data.push(item.value);
          }
        }
        formData[name] = data.join(', ');
      }
    });

    // add form-specific values into the data
    formData.formDataNameOrder = JSON.stringify(fields);
    formData.formGoogleSheetName = form.dataset.sheet || "Sheet1"; // default sheet name
    formData.formGoogleSendEmail = form.dataset.email || ""; // no email by default

    console.log(formData);
    return formData;
  }

  function handleFormSubmit(event) {  
    event.preventDefault();           
    var form = event.target;
    var data = getFormData(form);         
    if( data.email && !validEmail(data.email) ) {   
      var invalidEmail = form.querySelector(".email-invalid");
      if (invalidEmail) {
        invalidEmail.style.display = "block";
        return false;
      }
    } else {
      disableAllButtons(form);
      var url = form.action;
      var xhr = new XMLHttpRequest();
      xhr.open('POST', url);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      xhr.onreadystatechange = function() {
          console.log(xhr.status, xhr.statusText);
          console.log(xhr.responseText);
          var formElements = form.querySelector(".form-elements")
          if (formElements) {
            formElements.style.display = "none"; // hide form
          }
          var thankYouMessage = form.querySelector(".thankyou_message");
          if (thankYouMessage) {
            thankYouMessage.style.display = "block";
          }
          return;
      };
      var encoded = Object.keys(data).map(function(k) {
          return encodeURIComponent(k) + "=" + encodeURIComponent(data[k]);
      }).join('&');
      xhr.send(encoded);
    }
  }
  
  function loaded() {
    console.log("Contact form submission handler loaded successfully.");
    var forms = document.querySelectorAll("form.gform");
    for (var i = 0; i < forms.length; i++) {
      forms[i].addEventListener("submit", handleFormSubmit, false);
    }
  };
  document.addEventListener("DOMContentLoaded", loaded, false);

  function disableAllButtons(form) {
    var buttons = form.querySelectorAll("button");
    for (var i = 0; i < buttons.length; i++) {
      buttons[i].disabled = true;
    }
  }
})();


  </script>
</center>
</body>
</html>
