> [!NOTE]
> while this is a cleaner solution to the problem of HTML's verbosity, i think this
> is still suboptimal and as such i see this as a failed experiment. this has mostly
> to do with the fact that while the HTML part itself is now cleaner, the jinja part
> is not.
> 
> i.e. having to add `end` statements is not nice. while i am in favour of lua/julia
> style of syntax (scoping) for indentation based languages (mostly because it makes
> things non-ambigious and more accessible (unless things like LSP/editors itself
> support the syntax, the general accessibility tools fail to identify the actual
> end of scopes)) but not for brace styled language.
>
> the problems/hurdles:
> - end blocks in jinja.
> - having to support new syntax in different editors.
> - lsp/editors not being able to provide a nicer editing experience.
>
> i am always in favor of having better tools, so if you have any ideas on how to solve
> this with a better solution then please feel free to communicate. i am all ears.

---

# cuteninja

A python package that allows you to use [KDL](https://kdl.dev) as the markup
with Jinja2 syntax support.

Why? Because KDL is much more readable than HTML. While looking through
the KDL repository I found a example file that used KDL as an alternative
of HTML. That was what give me the initial idea of using KDL as the markup
with Jinja in Python for succint and maintainable template.

This packages doesn't do much, it extract the Jinja syntax from source files
and then parses the KDL via kdl-rs which generates the HTML. The previously
extracted Jinja syntax is restored and returned as valid HTML with Jinja
syntax. i.e.

```kdl
!DOCTYPE html
html lang=en {
    head {
        title "{{ page_title }}"
    }
    body {
        h1 "Hello, {{ user.name }}!"
        {% if user.is_authenticated %}
        div class=content {
            p "You are logged in."
        }
        {% else %}
        div class=login {
            a href="/login" "Please log in"
        }
        {% endif %}
    }
}
```

is turned to the following:

```jinja
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>{{ page_title }}</title>
    </head>
    <body>
        <h1>Hello, {{ user.name }}!</h1>
        {% if user.is_authenticated %}
        <div class="content">
            <p>You are logged in.</p>
        </div>
        {% else %}
        <div class="login">
            <a href="/login">Please log in</a>
        </div>
        {% endif %}
    </body>
</html>
```

## License

MIT

## Credits

- [KDL Document Language](https://kdl.dev)
- [kdl-rs](https://github.com/kdl-org/kdl-rs)
