---
layout: post
title:      "Active Storage & AJAX: I'm Super Serial You Guys."
date:       2019-09-22 09:30:39 +0000
permalink:  active_storage_and_ajax_im_super_serial_you_guys
---


Last blog post I spoke about how to user Rails Active Storage to upload files and attach them to your models. This time I'm going to talk about how to do the same thing...but with AJAX! So basically I spent a lot of time trying to figure this out and so I hope I can save you some time by showing what I did. First thing you need to wrap the form your submitting in a FormData object. From Mozilla- "The FormData interface provides a way to easily construct a set of key/value pairs representing form fields and their values, which can then be easily sent using the XMLHttpRequest.send() method. It uses the same format a form would use if the encoding type were set to "multipart/form-data"."

Here's how I did that:
```
function postElement() {
  $("form#new_element").submit(function(e) {
    e.preventDefault();
    let formData = new FormData(document.getElementById("new_element"));
    $.ajax({
      type: "POST",
      url: `http://localhost:3000/users/${userId}/elements`,
      data: formData,
      contentType: false,
      processData: false,
      success: document.getElementById("new-element-form-div").innerHTML = 'Element Added!'
    })

  })
}
```

Great. But that's only half the battle. We still need to get our view to display these files in the same way we display other model attributes using class constructors. For that we need to serialize the data. However, you can't serialize image/ audio data. What you can serialize, are their URLs. Given the way HTML tags work all you need to do is access the URL for the blob(what active storage gives you when you ask for your attachment) and ask for its URL, and serialize that. And so:
```
class InstrumentSerializer < ActiveModel::Serializer
  include Rails.application.routes.url_helpers

  attributes......... :picture



  def picture
    if object.picture.attached?
      variant = object.picture.variant(resize: '200x200')
      rails_representation_url(variant, disposition: 'attachment', only_path: true)
    end
   end
end
```

Very nice. Stick that in an html image tag as your source like `<img src="${this.picture}"/>` and you've got yourself a nice picture of like a cat or something. Hope this saves you as much time as it took me to figure it out!
