# Super Simple Doc Gen

A language agnostic documentation generator written in nelua

## Requirement
- [nelua](https://github.com/edubart/nelua-lang/)

## Quick Start

```lua
-- file-doc.nelua
local ssdg = require "path.to.ssdg"

local gen = ssdg.new({
  lang =  "nelua",
  lead =  "-",
  single_line =  "--",
  multi_line =  {
    starting = "--[[",
    ending = "]]"
  },
  code_block =  {
    starting = "`",
    ending = "`"
  },
  ignore_block =  {
    starting = "|",
    ending = "|"
  },
  sub =  {
    starting = ":",
    ending = ":"
  },
})

gen:add([[
# File

This is just dummy text

]])

gen:add_file("file.nelua")

gen:add([[
---

You have reached the end of the documentation!
]])

gen:write_file("file.md")

```

See the [example](./example) folder for an example on how to generate documentation

To regenerate `example/person.md` run the following from the project's root:

```bash
nelua example/person-docs.nelua
```

## Types

### Block record

```lua
local Block = @record{
  starting: string,
  ending: string
}
```

### Config record

```lua
local Config = @record{
  lang: string, -- required, the language that will be appended to code blocks
  lead: string, -- optional, default: '-', differentiates regular comments from doc comments
  single_line: string, -- required, defines the way singline comments should be extracted
  multi_line: Block, -- optional, default 'none', defines the way multiline comments should be extracted
  code_block: Block, -- optional, default '`', delimeters used to determine when to start or stop wrapping code in a code block
  ignore_block: Block, -- optional, default '|', delimeters used to determine when to start or stop ignoring comments
  sub: Block, -- optional, default ':', delimeters used to determine when to start or stop definining the content that will be subbed in a doc
}
```

### FileOptions record

Used to pass extra behaviours to [ssdg:add_file](#ssdgadd_file)
`subs` - Used to replace any thing that matches the inside of the [Config](#config-record).sub Block

```lua
local FileOptions = @record{
  subs: hashmap(string, string)
}
```

### ssdg record

```lua
local ssdg = @record{
  buf: stringbuilder,
  conf: Config
}
```

## API

### ssdg.new

Create a new instance of `ssdg` to be used to generate docs

```lua
function ssdg.new(conf: Config): ssdg
```

### ssdg:add

Adds `s` to the instance buffer

```lua
function ssdg:add(s: string)
```

### ssdg:add_file

Reads the content of a file at `path` adding any docs to the buffer based on the provided config

```lua
function ssdg:add_file(path: string, opts: FileOptions)
```

### ssdg:write_file

Writes the contents of the instance buffer to a file at `path`

```lua
function ssdg:write_file(path: string)
```

## Acknowledgements

This libary is heavily inspired by [fir](https://github.com/daelvn/fir) and [nldoc](https://github.com/edubart/nldoc)
