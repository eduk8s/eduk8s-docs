Page Formatting
===============

Individual module files can use either [Markdown](https://github.github.com/gfm/) or [AsciiDoc](http://asciidoc.org/) markup formats. The extension used on the file should be ``.md`` or ``.adoc``, corresponding to which formatting markup style you want to use.

Annotation of executable commands
---------------------------------

In conjunction with the standard Markdown and AsciiDoc, additional annotations can be applied to code blocks. The annotations are used to indicate that a user can click on the code block and have it copied to the terminal and executed.

If using Markdown, to annotate a code block so that it will be copied to the terminal and executed, use:

~~~text
```execute
echo "Execute command."
```
~~~

When you click on the code block the command will be executed in the first terminal of the workshop dashboard.

If using AsciiDoc, you would instead use the ``role`` annotation in an existing code block:

```text
[source,bash,role=execute]
----
echo "Execute command."
----
```

When the workshop dashboard is configured to display multiple terminals, you can qualify which terminal the command should be executed in by adding a suffix to the ``execute`` annotation. For the first terminal use ``execute-1``, for the second terminal ``execute-2``, etc:

~~~text
```execute-1
echo "Execute command."
```

```execute-2
echo "Execute command."
```
~~~

If you want to be able to send a command to all terminals, you can use ``execute-all``.

~~~text
```execute-all
clear
```
~~~

In most cases, a command you execute would complete straight away. If you need to run a command that never returns, with the user needing to interrupt it to stop it, you can use the special string ``<ctrl+c>`` in a subsequent code block.

~~~text
```execute
<ctrl+c>
```
~~~

When the user clicks on this code block, the running command in the corresponding terminal will be interrupted.

Annotation of text to be copied
-------------------------------

Instead of executing a command, you wanted the content of the code block to be copied into the paste buffer, you can use:

~~~text
```copy
echo "Text to copy."
```
~~~

After clicking on this code block, you could then paste the content into another window.

If you have a situation where the text being copied should be modified before use, you can denote this special case by using ``copy-and-edit`` instead of ``copy``. The text will still be copied to the paste buffer, but will be displayed in the browser in a way to highlight that it needs to be changed before use.

~~~text
```copy-and-edit
echo "Text to copy and edit."
```
~~~

For AsciiDoc, similar to ``execute``, you can add the ``role`` of ``copy`` or ``copy-and-edit``:

~~~text
[source,bash,role=copy]
----
echo "Text to copy."
----

[source,bash,role=copy-and-edit]
----
echo "Text to copy and edit."
----
~~~

Extensible clickable actions
----------------------------

The means to annotate code blocks described above were the original methods used to indicate code blocks to be executed or copied when clicked. To support a growing number of clickable actions with different customizable purposes, annotation names were changed to being namespaced. The above annotations will still be supported, but the following are now recommended, with additional options available to customize the way the actions are presented.

For code execution, instead of:

~~~text
```execute
echo "Execute command."
```
~~~

you can use:

~~~text
```terminal:execute
command: echo "Execute command."
```
~~~

The contents of the code block is YAML. The executable command needs to be set as the ``command`` property. By default when clicked the command will be executed in terminal session 1. If you want to specify a different terminal session, you can set the ``session`` property.

~~~text
```terminal:execute
command: echo "Execute command."
session: 1
```
~~~

To define a command when clicked that will execute in all terminal sessions on the terminals tab of the dashboard, you can also use:

~~~text
```terminal:execute-all
command: echo "Execute command."
```
~~~

Using this new form of clickable actions, the preferred method for indicating that a running command in a terminal session should be interrupted is by using:

~~~text
```terminal:interrrupt
session: 1
```
~~~

You can optionally specify the ``session`` property within the code block to indicate an alternate terminal session to session 1.

To have an interrupt sent to all terminals sessions on the terminals tab of the dashboard, you can use:

~~~text
```terminal:interrrupt-all
```
~~~

Where you want to enter input into a terminal but it isn't a command, such as when a running command is prompting for input such as a password, to denote it as being input rather than a command, you can use:

~~~text
```terminal:input
text: password
```
~~~

As for executing commands or interrupting a command, you can specify the ``session`` property to indicate a specific terminal to send it to if you don't want to send it to terminal session 1.

~~~text
```terminal:input
text: password
session: 1
```
~~~

To clear all terminal sessions on the terminals tab of the dashboard, you can use:

~~~text
```terminal:clear-all
```
~~~

This works by executing the ``clear`` command in each, so the terminal sessions need to be at the shell prompt and able to accept a command.

For copying content to the paste buffer you can use:

~~~text
```workshop:copy
text: echo "Text to copy."
```
~~~

or:

~~~text
```workshop:copy-and-edit
text: echo "Text to copy and edit."
```
~~~

A benefit of using these over the original mechanism is that by using the appropriate YAML syntax, you can control whether a multi line string value is concatenated into one line, or whether line breaks are preserved, along with whether initial or terminating new lines are included. In the original mechanism the string was always trimmed before use.

By using the different forms above when appropriate, the code block when displayed can be annotated with a different message indicating what will happen.

The method for using AsciiDoc is similar, using the ``role`` for the name of the annotation and YAML as the content:

~~~text
[source,bash,role=terminal:execute]
----
command: echo "Execute command."
----
~~~

Clickable actions for the dashboard
-----------------------------------

In addition to the clickable actions related to the terminal and copying of text to the paste buffer, additional actions are available for controlling the dashboard and opening URL links.

To have the action when clicked open a URL in a new browser, you can use:

~~~text
```dashboard:open-url
url: https://www.example.com/
```
~~~

In order to allow a user to click in the workshop content to display a specific dashboard tab if hidden, you can use:

~~~text
```dashboard:open-dashboard
name: Terminals
```
~~~

To create a new dashboard tab with a specific URL, you can use:

~~~text
```dashboard:create-dashboard
name: Example
url: https://www.example.com/
```
~~~

To create a new dashboard tab with a new terminal session, you can use:

~~~text
```dashboard:create-dashboard
name: Example
url: terminal:example
```
~~~

The value should be of the form ``terminal:<session>``, where ``<session>`` is replaced with the name you want to give the terminal session. The terminal session name should be restricted to lower case letters, numbers and ‘-‘. You should avoid using numeric terminal session names such as "1", "2" and "3" as these are use for the default terminal sessions.

To reload an existing dashboard, using whatever URL it is currently targetting, you can use:

~~~text
```dashboard:reload-dashboard
name: Example
```
~~~

If the dashboard is for a terminal session there will be no effect unless the terminal session had been disconnected, in which case it will be reconnected.

To change the URL target of an existing dashboard, you can specify the new URL when reloading a dashboard:

~~~text
```dashboard:reload-dashboard
name: Example
url: https://www.example.com/
```
~~~

You cannot change the target of a dashboard which includes a terminal session.

To delete a dashboard, you can use:

~~~text
```dashboard:delete-dashboard
name: Example
```
~~~

You cannot delete dashboards corresponding to builtin applications provided by the workshop environment, such as the default terminals, console, editor or slides.

Deleting a custom dashboard including a terminal session will not destroy the underlying terminal session and it can be connected to again by creating a new custom dashboard for the same terminal session name.

Clickable actions for the editor
--------------------------------

If the embedded editor is enabled, special actions are available which control the editor.

To open an existing file you can use:

~~~text
```editor:open-file
file: ~/exercises/sample.txt
```
~~~

You can use ``~/`` prefix to indicate the path relative to the home directory of the session. On opening the file, if you want the insertion point left on a specific line, provide the ``line`` property. Lines numbers start at ``1``.

~~~text
```editor:open-file
file: ~/exercises/sample.txt
line: 1
```
~~~

To highlight certain lines of a file based on an exact string match, use:

~~~text
```editor:select-matching-text
file: ~/exercises/sample.txt
text: "int main()"
```
~~~

The region of the match will be highlighted by default. If you want to highlight any number of lines before or after the line with the match, you can specify the ``before`` and ``after`` properties.

~~~text
```editor:select-matching-text
file: ~/exercises/sample.txt
text: "int main()"
before: 1
after: 1
```
~~~

Setting both ``before`` and ``after`` to ``0`` will result in the complete line which matched being highlighted instead of any region within the line.

To match based on a regular expression, rather than an exact match, set ``isRegex`` to ``true``.

~~~text
```editor:select-matching-text
file: ~/exercises/sample.txt
text: "int main(.*)"
isRegex: true
```
~~~

For both an exact match and regular expression, the text to be matched must all be on one line. It is not possible to match on text which spans across lines.

To append lines to the end of a file, use:

~~~text
```editor:append-lines-to-file
file: ~/exercises/sample.txt
text: |
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed
    do eiusmod tempor incididunt ut labore et dolore magna aliqua.
```
~~~

If you use ``editor:append-to-lines-to-file`` and the file doesn't exist it will be created for you. You can therefore use this to create new files.

To insert lines before a specified line in the file, use:

~~~text
```editor:insert-lines-before-line
file: ~/exercises/sample.txt
line: 8
text: |
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed
    do eiusmod tempor incididunt ut labore et dolore magna aliqua.
```
~~~

To insert lines after matching a line containing a specified string, use:

~~~text
```editor:append-lines-after-match
file: ~/exercises/sample.txt
match: Lorem ipsum
text: |
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed
    do eiusmod tempor incididunt ut labore et dolore magna aliqua.
```
~~~

Where the file contains YAML, to insert a new YAML value into an existing structure, use:

~~~text
```editor:insert-value-into-yaml
file: ~/exercises/deployment.yaml
path: spec.template.spec.containers
value:
- name: nginx
    image: nginx:latest
```
~~~

To execute a registered VS code command, you can use:

~~~
```editor:execute-command
command: spring.initializr.maven-project
args:
- language: Java
    dependencies: [ "actuator", "webflux" ]
    artifactId: demo
    groupId: com.example
```
~~~

Escaping of code block content
------------------------------

Because the [Liquid](https://www.npmjs.com/package/liquidjs) template engine is applied to workshop content, it is necessary to escape content in code blocks which conflicts with the syntactic elements of the Liquid template engine. To escape such elements you will need to suspend processing by the template engine for that section of workshop content to ensure it is rendered correctly. This can be done using a Liquid ``{% raw %}...{% endraw %}`` block.

~~~
{% raw %}
```execute
echo "Execute command."
```
{% endraw %}
~~~

This will have the side effect of preventing interpolation of data variables, so restrict it to only the scope you need it.

Interpolation of data variables
-------------------------------

When creating page content, you can reference a number of pre-defined data variables. The values of the data variables will be substituted into the page when rendered in the users browser.

The workshop environment provides the following built-in data variables.

* ``workshop_name`` - The name of the workshop.
* ``workshop_namespace`` - The name of the namespace used for the workshop environment.
* ``session_namespace`` - The name of the namespace the workshop instance is linked to and into which any deployed applications will run.
* ``training_portal`` - The name of the training portal the workshop is being hosted by.
* ``ingress_domain`` - The host domain which should be used in the any generated hostname of ingress routes for exposing applications.
* ``ingress_protocol`` - The protocol (http/https) that is used for ingress routes which are created for workshops.

To use a data variable within the page content, surround it by matching pairs of brackets:

```text
{{ session_namespace }}
```

This can be done inside of code blocks, including clickable actions, as well as in URLs:

```text
http://myapp-{{ session_namespace }}.{{ ingress_domain }}
```

When the workshop environment is hosted in Kubernetes and provides access to the underlying cluster, the following additional data variables are also available.

* ``kubernetes_token`` - The Kubernetes access token of the service account that the workshop session is running as.
* ``kubernetes_ca_crt`` - The contents of the public certificate required when accessing the Kubernetes API URL.
* ``kubernetes_api_url`` - The URL for accessing the Kubernetes API. This is only valid when used from the workshop terminal.

Note that an older version of the rendering engine required that data variables be surrounded on each side with the character ``%``. This is still supported for backwards compatibility, but you should now use matched pairs of brackets instead. Support for percentage delimiters may be removed in a future version.

Adding custom data variables
----------------------------

You can introduce your own data variables by listing them in the ``workshop/modules.yaml`` file. A data variable is defined as having a default value, but where the value will be overridden if an environment variable of the same name is defined.

The field under which the data variables should be specified is ``config.vars``:

```yaml
config:
    vars:
    - name: LANGUAGE
    value: undefined
```

Where you want to use a name for a data variable which is different to the environment variable name, you can add a list of ``aliases``:

```yaml
config:
    vars:
    - name: LANGUAGE
    value: undefined
    aliases:
    - PROGRAMMING_LANGUAGE
```

The environment variables with names given in the list of aliases will be checked first, then the environment variable with the same name as the data variable. If no environment variables with those names are set, then the default value will be used.

The default value for a data variable can be overridden for a specific workshop by setting it in the corresponding workshop file. For example, ``workshop/workshop-python.yaml`` might contain:

```yaml
vars:
    LANGUAGE: python
```

If you need more control over setting the values of data variables, you can provide the file ``workshop/config.js``. The form of this file should be:

```javascript
function initialize(workshop) {
    workshop.load_workshop();

    if (process.env['WORKSHOP_FILE'] == 'workshop-python.yaml') {
        workshop.data_variable('LANGUAGE', 'python');
    }
}

exports.default = initialize;

module.exports = exports.default;
```

This Javascript code will be loaded and the ``initialize()`` function called to load the workshop configuration. You can then use the ``workshop.data_variable()`` function to set up any data variables

Because it is Javascript, you can write any code you need to query process environment variables and set data variables based on those. This might include creating composite values constructed from multiple environment variables. You could even download data variables from a remote host.

Passing of environment variables
--------------------------------

The passing of environment variables, including remapping of variable names, can be achieved by setting your own custom data variables. If you don't need to set default values, or remap the name of an environment variable, you can instead reference the name of the environment variable directly, albeit that you must prefix the name with ``ENV_`` when using it.

For example, if you wanted to display the value of the ``KUBECTL_VERSION`` environment variable in the workshop content, you can use ``ENV_KUBECTL_VERSION``, as in:

```
{{ ENV_KUBECTL_VERSION }}
```

Handling of embedded URL links
------------------------------

URLs can be included in workshop content. This can be the literal URL, or the Markdown or AsciiDoc syntax for including and labelling a URL. What happens when a user clicks on a URL, will depend on the specific URL.

In the case of the URL being an external web site, when the URL is clicked, the URL will be opened in a new browser tab or window.

When the URL is a relative page referring to another page which is a part of the workshop content, the page will replace the current workshop page.

You can define a URL where components of the URL are provided by data variables. Data variables useful in this content are ``session_namespace`` and ``ingress_domain`` as they can be used to create a URL to an application deployed from a workshop:

```text
https://myapp-{{ session_namespace }}.{{ ingress_domain }}
```

Conditional rendering of content
--------------------------------

As rendering of pages is in part handled using the [Liquid](https://www.npmjs.com/package/liquidjs) template engine, you can also use any constructs the template engine supports for conditional content.

```text
{% if LANGUAGE == 'java' %}
....
{% endif %}
{% if LANGUAGE == 'python' %}
....
{% endif %}
```