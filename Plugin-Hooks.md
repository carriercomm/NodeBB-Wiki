# Available Hooks

The following is a list of all hooks present in NodeBB. This list is intended to guide developers who are looking to write plugins for NodeBB. For more information, please consult [Writing Plugins for NodeBB](https://docs.nodebb.org/en/latest/plugins/create.html).

There are three types of hooks, **filters**, **actions**, and **static**.

* Filters take an input (provided as a single argument), parse it in some way, and return the changed value.
* Actions take multiple inputs, and execute actions based on the inputs received. Actions do not return anything.
* Static hooks take an input, execute actions based on the inputs received, and executes a callback when it is finished. Anything returned in the callback is ignored.

**Important**: This list is by no means exhaustive. Hooks are added on an as-needed basis (or if we can see a potential use case ahead of time), and all requests to add new hooks to NodeBB should be sent to us via the [issue tracker](https://github.com/NodeBB/NodeBB/issues).


## Filters


### filter:admin.header_build

Allows plugins to create new navigation links in the ACP


### filter:post.save

**Argument(s)**: A post's content (markdown text)

Executed whenever a post is created or edited, but before it is saved into the database.


### filter:post.get

**Argument(s)**: A post object (javascript Object)

Executed whenever a post is retrieved, but before being sent to the client.


### filter:header.build

**Allows plugins to add new navigation links to NodeBB**


### filter:register.build

**Argument(s)**:

 * `req` the express request object (javascript Object)
 * `res` the express response object (javascript Object)
 * `data` the data passed to the template (javascript Object)

**Allows plugins to add new elements to the registration form. At the moment, the only one supported is `data.captcha`**


### filter:post.parse

**Argument(s)**: A post or signature's raw text (String)

Executed when a post or signature needs to be parsed from raw text to HTML (for output to client). This is useful if you'd like to use a parser to prettify posts, such as [Markdown](http://daringfireball.net/projects/markdown/), or [BBCode](http://www.bbcode.org/).


### filter:posts.custom_profile_info

**Allows plugins to add custom profile information in the topic view's author post block**


### filter:posts.modifyUserInfo

This is the user object found in any post array. It's a smaller subset of the full list of user data to save on bandwidth, so if you've added custom fields in via ``user.setUserField``, add it here.


### filter:register.check

**Argument(s)**:

 * `req` the express request object (javascript Object)
 * `res` the express response object (javascript Object)
 * `userData` the user data parsed from the form

**Allows plugins to run checks on information and deny registration if necessary.**


### filter:scripts.get

**Allows to add client-side JS to the header and queue up for minification on production**


### filter:uploadImage


### filter:uploadFile


### filter:widgets.getAreas


### filter:widgets.getWidgets


### filter:search.query


### filter:post.parse


### filter:messaging.save


### filter:messaging.parse


### filter:sounds.get


### filter:post.getPosts

{posts: posts, uid: uid}

Where uid is the callee 

### filter:post.getFields


### filter:auth.init


### filter:composer.help


### filter:topic.thread_tools


### filter:topic.get


Passes in the final parsed topic data.

### filter:user.create


### filter:user.delete


### filter:user.profileLinks


### filter:user.verify.code

**Argument(s)**:

 * `confirm_code` The confirmation code (String)

Ability to modify the generated verification code (ex. for using a shorter verification code instead for SMS verification)


### filter:user.custom_fields

**Argument(s)**:

 * `userData` (javascript Object)

Allows you to append custom fields to the newly created user, ex. mobileNumber


### filter:register.complete

**Argument(s)**:
 * `uid` The new user id (javascript Number)
 * `destination` The referral url (String)

Set the post-registration destination, or do post-register tasks here.


### filter:widget.render


### filter:templates.get_virtual

Allows you to modify the `api/get_templates_listing` API call, allowing ajaxification to custom templates that are not served physically via the ``templates` parameter in ``plugin.json``


### filter:templates.get_config

Allows you to add custom ajaxification rules in the `api/get_templates_listing` API call. See https://github.com/NodeBB/nodebb-theme-vanilla/blob/master/templates/config.json for more details


### filter:sockets.sendNewPostToUids

An object consisting of a `uidsTo` array, a `uidFrom` integer, and a `type` ("newPost" or "newTopic". Useful for modifying who gets notifications of new posts (ex: ignore lists, etc).



## Actions


### action:app.load

**Argument(s)**: None

Executed when NodeBB is loaded, used to kickstart scripts in plugins (i.e. cron jobs, etc)


### action:page.load

**Argument(s)**: An object containing the following properties:

* ``template`` - The template loaded
* ``url`` - Path to the page (relative to the site's base url)


### action:plugin.activate

**Argument(s)**: A String containing the plugin's ``id`` (e.g. ``nodebb-plugin-markdown``)

Executed whenever a plugin is activated via the admin panel.

**Important**: Be sure to check the ``id`` that is sent in with this hook, otherwise your plugin will fire its registered hook method, even if your plugin was not the one that was activated.


### action:plugin.deactivate

**Argument(s)**: A String containing the plugin's ``id`` (e.g. ``nodebb-plugin-markdown``)


Executed whenever a plugin is deactivated via the admin panel.

**Important**: Be sure to check the ``id`` that is sent in with this hook, otherwise your plugin will fire its registered hook method, even if your plugin was not the one that was deactivated.


### action:post.save

**Argument(s)**: A post object (javascript Object)

Executed whenever a post is created or edited, after it is saved into the database.


### action:post.upvote

**Argument(s)**: pid, uid

Executed whenever a post is upvoted. ``uid`` is the user that has triggered the upvote.


### action:post.downvote

**Argument(s)**: pid, uid

Executed whenever a post is downvoted. ``uid`` is the user that has triggered the downvote.

### action:email.send


### action:post.setField


### action:topic.edit


### action:topic.pin

Called when toggling pinned state
Object: tid, isPinned, uid


### action:topic.lock

Called when toggling locked state
Object: tid, isLocked, uid


### action:topic.move

Called when moving a topic from one category to another
Object: tid, fromCid, toCid, uid


### action:post.edit


### action:post.delete


### action:post.restore


### action:notification.pushed

**Argument(s)**: A notification object (javascript Object)

Executed whenever a notification is pushed to a user.

### action:config.set


### action:topic.save


### action:topic.delete


### action:user.create


### action:user.verify

Parameters: uid; a hash of confirmation data (ex. confirm_link, confirm_code)
Useful for overriding the verification system. Currently if this hook is set, the email verification system is disabled outright.


### action:user.follow
Parameters: fromUid, toUid


### action:user.unfollow

Parameters: fromUid, toUid


### action:user.set

Parameters: field (str), value, type ('set', 'increment', or 'decrement')
Useful for things like awarding badges or achievements after a user has reached some value (ex. 100 posts)


### action:settings.set

Parameters: hash (str), object (obj)
Useful if your plugins want to cache settings instead of pulling from DB everytime a method is called. Listen to this and refresh accordingly.


Client Side Hooks
--------------------

### filter:categories.new_topic


### action:popstate

### action:ajaxify.start

### action:ajaxify.loadingTemplates

### action:ajaxify.loadingData

### action:ajaxify.contentLoaded

### action:ajaxify.end

### action:reconnected

### action:connected

### action:disconnected

### action:categories.loading

### action:categories.loaded

### action:categories.new_topic.loaded

### action:topic.loading

### action:topic.loaded

### action:composer.loaded

### action:composer.topics.post

### action:composer.posts.reply

### action:composer.posts.edit

### action:widgets.loaded