# UnderscoreJS
# =======================================================
Backbone.js users use bind and bindAll methods provide by underscore.js a lot.
Function bindAll internally uses bind . And bind internally uses apply.
_.bindAll() changes this in the named functions to always point to that object, so that you can use this.model.bind(). Notice that in your second example, a third argument is passed to bind(); that's why using _.bindAll() isn't necessary in that case. In general, it's a good idea to use for any methods on the model that will be used as callbacks to events so that you can refer to this more easily.

# Collections
# =======================================================
fetch()



# Models
# =======================================================
###### Defining a Model
```
Group = Backbone.Model.extend({
    urlRoot: SITE_PATH + 'group/index',
});

// And in route:

this.group = new Group({id: id});
this.group.fetch();

isNew model.isNew()
```

Has this model been saved to the server yet? If the model does not yet have an id, it is considered to be new.
And when you save a model,

if it is new, a POST request will be emitted,
if it is an update (an id has been set), a PUT request will be sent

###### Default Attributes

###### Methods
fetch()
save()
destroy()
@model.on('change', @render, this)

###### Custom URLs for a model
```
class AmberCustomer.Models.Service extends Backbone.Model
  urlRoot: Amber.config.product_service_url_public + "/services"

  getCustomUrl: (method) ->
    switch method
      when 'read'
        return @urlRoot + "/" + @id + ".json"
      when 'create'
        return @urlRoot + ".json"
      when 'update'
        return @urlRoot + "/" + @id + ".json"
      when 'delete'
        return @urlRoot + "/" + @id + ".json"
    return

  sync: (method, model, options) ->
    options or (options = {})
    options.url = @getCustomUrl(method.toLowerCase())
    # Lets notify backbone to use our URLs and do follow default course
    Backbone.sync.apply this, arguments

  parse: (response) ->
    if response != undefined
      return response.data
```