# Person Library

Just an example documentation.
This text goes in the top of the page.

## Person record

```lua
global Person = @record{
  name: string, -- Full name.
  age: integer, -- Years since birth.
}
```

## Person.create

Creates a new person with `name` and `age`

```lua
function Person.create(name: string, age: integer): *Person
```

## Person:get_name

Returns the person name

```lua
function Person:get_name(): string
```

## Person:get_age

Returns the person age

```lua
function Person:get_age(): integer
```

---

You have reached the end of the documentation!
