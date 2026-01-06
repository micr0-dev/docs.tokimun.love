# Language Reference

tokimun <span class="font-pona">toki mun</span> its lua, but like, doesn't make you want to pull your hair out.

tokimun compiles to plain lua. that means you can use it anywhere lua runs: LuaJIT, Lua 5.1–5.4, Roblox Luau, LÖVE, whatever. write tokimun, get lua out the other end.

file extension: `.tkm`

---

## Indexing

0-based. like a normal language.
```tkm
arr = {"a", "b", "c"}
print(arr[0])  -- "a"
print(arr[1])  -- "b"
print(arr[2])  -- "c"
```

this applies everywhere: array access, `ipairs`, string functions, all of it.

---

## Variables & Scoping

variables are **local by default**.
```tkm
x = 10        -- this is local
global y = 20 -- this is global
```

if you forget `global`, it's local. this is the opposite of lua. you're welcome.

---

## Operators

### not equal

`!=` works. `~=` also still works if you're a lua purist for some reason.
```tkm
if x != 5 then
  print("not five")
end
```

### compound assignment
```tkm
x += 1
x -= 1
x *= 2
x /= 2
x %= 3
s ..= "more text"
```

these compile to `x = x + 1`, etc. nothing fancy.

### null coalescing

`??` returns the right side if the left is `nil`.
```tkm
name = input ?? "anonymous"
```

compiles to something like `local name = input ~= nil and input or "anonymous"` but handles the falsy edge cases properly.

### optional chaining

`?.` short-circuits if anything in the chain is `nil`.
```tkm
zip = user?.address?.zipcode
```

if `user` is nil, or `user.address` is nil, you get `nil` back instead of an error.

---

## Numbers

all of these work:
```tkm
dec = 42
hex = 0x2A
bin = 0b101010
oct = 0o52
```

they all equal 42. hex is already in lua, we're just adding binary and octal because it's already [insert year here].

---

## Strings & Templates

backtick strings with `${}` interpolation:
```tkm
name = "mun"
greeting = `hello ${name}!`
print(greeting)  -- "hello mun!"
```

expressions work too:
```tkm
print(`2 + 2 = ${2 + 2}`)  -- "2 + 2 = 4"
```

regular `"strings"` and `'strings'` and `[[multiline strings]]` still work the same as lua.

---

## Control Flow

### continue

wow, it exists now.
```tkm
for i = 0, 9 do
  if i % 2 == 0 then
    continue
  end
  print(i)  -- prints 1, 3, 5, 7, 9
end
```

### switch statements

these are made with implicit fallthrough prevention by default. which you can just use 'countinue' to get fallthrough if you want it.
```tkm
switch status
  case "ok":
    print("all good")
  case "error", "fail":
    print("something went wrong")
  default:
    print("unknown status")
end
```

compiles to an if/elseif chain under the hood.

---

## Tables

same as lua, but 0-indexed.
```tkm
t = {
  "first",   -- t[0]
  "second",  -- t[1]
  name = "tokimun",  -- t.name (string keys unchanged)
}
```

trailing commas are allowed everywhere:
```tkm
t = {
  "a",
  "b",
  "c",  -- no angry compiler
}
```

---

## Functions

same as lua, nothing changed here.
```tkm
function greet(name)
  return `hello ${name}`
end

-- or
greet = function(name)
  return `hello ${name}`
end
```

---

## Lua Interop

tokimun compiles to standard lua. array indices are automatically offset by 1 during compilation, so lua libraries just work.

you write `arr[0]`, it compiles to `arr[1]`. you never have to think about it.

---

## That's It

tokimun <span class="font-pona">toki mun</span> is small on purpose. its still lua. but with the bullshit removed.