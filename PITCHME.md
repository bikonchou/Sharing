<!-- $theme: default -->
<!-- page_number: true -->

### How To Disable Turbolinks from Rails

---

### Scenario:
*- We removed Turbolinks, because it's not compatible with LiveChat.*

---

### Remove Turbolinks entirely
*- http://codkal.com/rails-how-to-remove-turbolinks/*

---

### Looks great! But......

---

### Several bugs reported by QAs.

---

### All AJAX forms with "remote: true" are not working

---

![screenshot](https://github.com/bikonchou/Sharing/blob/how_to_disable_turbolinks/assets/images/jacky_WTF.jpg?raw=true)

---

### Log when enabled Turbolinks
```
Started POST "/writers" for ::1 at 2017-09-15 15:02:13 +0800
Processing by Writer::RegistrationsController#create as JS
Parameters: {"utf8"=>"âœ“", "writer"=>{"profile_attributes"=>{"first_name"=>"TEST", "last_name"=>"TEST"...}
Redirected to http://localhost:3000/writer/projects
Completed 200 OK in 231ms (ActiveRecord: 19.7ms)

Started GET "/writer/projects" for ::1 at 2017-09-15 15:02:13 +0800
Processing by Writer::ProjectsController#index as JS
```

---

### Log when disabled Turbolinks
```
Started POST "/writers" for ::1 at 2017-09-15 15:02:13 +0800
Processing by Writer::RegistrationsController#create as JS
Parameters: {"utf8"=>"âœ“", "writer"=>{"profile_attributes"=>{"first_name"=>"TEST", "last_name"=>"TEST"...}
Redirected to http://localhost:3000/writer/projects
Completed 302 Found in 671ms (ActiveRecord: 12.7ms)

Started GET "/writer/projects" for ::1 at 2017-09-15 15:06:49 +0800
Processing by Writer::ProjectsController#index as JS
Rendered writer/projects/index.js.erb (206.9ms)
Completed 200 OK in 312ms (Views: 207.0ms | ActiveRecord: 36.4ms)
```

---

### In Controller
```ruby
module Writer
  class RegistrationsController < Devise::RegistrationsController
    def create
      run Registration::Create do |op|
		.
       	.
        .
        return redirect_to after_registration_path(op.model)
      end
    end
  end
end
```

---

### Should revise to
```ruby
module Writer
  class RegistrationsController < Devise::RegistrationsController
    def create
      run Registration::Create do |op|
		.
       	.
        .    
        return render js: %(window.location = '#{desired_redirect_url}')
      end
    end
  end
end
```
---
