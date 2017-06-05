# Basics
# =======================================================
These days, it is kind of popular to generate static assets such as JavaScript and CSS by using some preprocessors. For example instead of writing pure Javascript you can use CoffeeScript, TypeScript… Instead of CSS, you can use Sass, Less, Stylus…

# PreProcessors
# =======================================================
Various preprocessors have various advantages. Such as hiding language’s awkwards, adding missing features, shorter source codes etc.
Un/fortunately web browsers don’t understand this languages. So they have to be compiled into languages supported by web browser, in this case namely JavaScript and CSS.
This step brings one fairly big disadvantage - worse debugging.
For example when error occures, browser can display affected line, but in compiled language.
You don’t have something like mapping between source code and compiled result which is served to browser.
You need to go through your code and try to guess where the problem is. Of course it is not big problem for small code base,
but it can be painful with large projects. This problem should be eliminated by source maps.
https://www.infoq.com/news/2011/08/debug-languages-on-javascript-vm


# Debugging
# =======================================================
###### Source Map Debugging
Use rails gem to generate source maps for the preprocessors.
- http://blog.vhyza.eu/blog/2013/09/22/debugging-rails-4-coffeescript-and-sass-source-files-in-google-chrome/
- https://confluence.jetbrains.com/display/RUBYDEV/Debugging+CoffeeScript+Code

After assets regeneration, assets/source_maps directory should be created in public.
When you start webserver and reload web page in Chrome, you should see scss and coffee files in inspector.


###### Debugging using Node Inspector
coffee --nodejs --debug-brk yourscript.coffee


# Syntax
# =======================================================
###### Traversing Arrays
```
arr = [1, 2, 3, 4, 5]
for i of arr
  console.log i
```

###### Traversing Objects
```
obj = {a: 1, b: 2, c: 3, d: 4, e: 5}
for k of obj
  console.log k
  obj[k]
```

###### Creating Vars
```
months:
  '1': 'January'
  '2': 'February'
  '3': 'March'
  '4': 'April'
  '5': 'May'
  '6': 'June'
  '7': 'July'
  '8': 'August'
  '9': 'September'
  '10': 'October'
  '11': 'November'
  '12': 'December'
```

###### Creating new objects
@collection = new AmberCustomer.Collections.Inventories()

###### Creating Vars
@some = "thing"

###### Calling Vars
@renderMap

###### Calling Functions
@renderMap()

###### Functions
Here $ is the ajax var
@ refers to the object of the class

```
fetchCollection: ->
  self = @
  // fetching the list of inhouse providers
  $.ajax(
    type: 'GET'
    url: Amber.config.product_service_url_public+ "/providers.json?type=basic&provider_type=employee&status=active"
    xhrFields: withCredentials: true
    crossDomain: true).success((json) ->
    console.log 'success', json.message
    self.inhouse_providers = json.data.result
    return
  ).error (error) ->
      console.log error.responseText
      self.inhouse_providers = []
      return
  @collection.fetch
    data: $.param(@urlParams)
    reset: true
```