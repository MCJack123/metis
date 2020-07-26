# [metis][github]

A collection of useful modules for ComputerCraft/CC: Tweaked. There's not much rhyme or reason to this really, so feel
free to PR useful libraries you might have.

## Usage
Paste the following code block on the top of your program, then feel free to require any of the documented modules.

```lua
local modules = {
  ["metis.argparse"] = "src/metis/argparse.lua",
  ["metis.crypto.sha1"] = "src/metis/crypto/sha1.lua",
  ["metis.timer"] = "src/metis/timer.lua",
}
package.loaders[#package.loaders + 1] = function(name)
  local path = modules[name]
  if not path then return nil, "not a metis module" end

  local local_path = "/.cache/metis/d624683f4e8bfa5da5f838fb142dd294543b1cc4/" .. path
  if not fs.exists(local_path) then
    local url = "https://raw.githubusercontent.com/SquidDev-CC/metis/d624683f4e8bfa5da5f838fb142dd294543b1cc4/" .. path
    local request, err = http.get(url)
    if not request then return nil, "Cannot download " .. url .. ": " .. err end

    local out = fs.open(local_path, "w")
    out.write(request.readAll())
    out.close()

    request.close()
  end


  local fn, err = loadfile(local_path, nil, _ENV)
  if fn then return fn, local_path else return nil, err end
end

return require
```

For other ways of using metis, see the [github repository][github].


[github]: https://github.com/SquidDev-CC/metis "metis on GitHub"
