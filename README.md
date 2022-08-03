



# Textbook Exploiting
Guide to standard Roblox exploiting/pen-testing, in ordered levels of intricacy.
> **Other recommended content (less boring than those below):**
> [wyn#0001 "Guide to Exploiting"](https://www.youtube.com/watch?v=dCx_bVm9x88) can help you start from the bottom up.
> [Synapse X Documentation](https://x.synapse.to/docs/development/objects_mts.html) decent guide on function hooks
> [DevForum "All you need to know about Metatables/Metamethods"](https://devforum.roblox.com/t/all-you-need-to-know-about-metatables-and-metamethods/503259) section 1-3
### Table of Contents
(pending)

# 0) Pre-K: Basic Lua

Lua is syntactically very straightforward. This section, like Pre-K, is unnecessary and skippable; just read section **0.2**.

**Contents:**
: **0.1** Basic logic (variables, if/else, loops, tables, functions)
: **0.2** Auspicious softwares

## 0.1 Basic logic
### Variables
Variables save a value for repeated use, such as a:
**string** - text encased in quotes `"Hello there"` `"24"` `"false"`
**boolean** - a true or false value `true` `false`
**number** - a correctly formed number `142041.4` `math.pi`
**function** - a function, or the value returned from it `function(a,b)` `something()`
**nil** - This Lua's special "nothing" value; it's like undeclaring a variable

**Global** variables are accessible anywhere; **local** variables have limited scope (inside one script, or inside one function, etc.)

You declare a variable, or edit an existing one, with an equals sign.
```
local hello
function a()
  local n = true
end
print(n)
hello = true
print(hello)
```
`nil`
`true`

### If/else
Conditional statements verify conditions using operators, such as `>` or `==`. Parentheses can group sets of checks together.
`if a then` continues if x is not false or nil
`if not b then` continues if y is false or nil
`if c == 5 + 5 then` continues if c is equal to 10
`if tostring(d) and e*f >= 50` continues if d is a string, and e times f is greater than or equal to 50. Note how there are *two* equals signs, not one.
`if (g or h) and i then` continues if either g or h exists, and i exists.
`if somefunction(j) then` continues if the function doesn't "return" false or nil.

### Loops and tables
**Tables** - Store pairs. Use curly brackets/commas. There are 2 kinds.
**Array** - A table where values are paired with numerically sequenced indexes/indices.
```
local array1 = {"stuff","goes","in","here",true,40}
local array2 = {
  [1] = "this is also",
  [2] = "an array."
}
print(array1[3], array2[5])
```
`in nil`

**Dictionary** - A table where values are paired with keys (strings or numbers).
```
local dict1 = {Spaces = "or", ["numbers"] = "need", [5] = "brackets"}
local dict2 = {
  ayo = function(n) print(n) end
}
print(dict1.Spaces, dict2.ayo(4), dict1[5])
```
`or 4 brackets`

**Generic for loop** - Repeats for each pair of values stored in a table.
```
for i,v in pairs(dict1) do -- i,v is short for index,value
  print(i,v)
end
```
```
Spaces or
numbers need
5 brackets
```
**Numeric for loop** - Repeats for a certain amount of times.
```
local x = 1
for i=1,5 do
x = x + 3 * 3
end
print(x)
```
`46`

**While loop** - Repeats while a condition is true.
```
while x > 45 do
  print("always remember to include a wait(). Don't crash yourself!")
  wait()
end
while wait() do
  print("many people also use syntax like this.")
  if x <= 45 then break end    -- breaks the loop
end
```
### Functions
Functions "abstract" ideas into workable, reusable code. They take at least 0 arguments, sometimes return values.
```
local function factorial(num)
  local n = 1
  for i = 1, num do
    n = n * i
  end
  return n
end
local function gaming()
  print(9+10, ":(")
end
print(factorial(factorial(3)))
gaming()
```
`720`



---
---
---


### Aim:
- **Get a text editor**. Don't save it all in your executor.
- **Learn variables**. global `getgenv()` local `local` 
- **Conditional loops** `while x do` `task.spawn(function()` `if x then break`
- **Remote events** `FireServer` Use ;rspy on [Infinite Yield](https://github.com/EdgeIY/infiniteyield) for RemoteSpy

###  A) Editors; file organization

#### ⬛ Get a good editor.
- [Visual Studio Code Insiders](https://code.visualstudio.com/insiders/) 
	- capable of installing many add-ons such as "Execute to Synapse" and GitHub synchronization (Don't worry about GitHub until later).
	- lower lag than normal VSCode
- [Notepad++](https://notepad-plus-plus.org/downloads/)
	- very lightweight and simple. Add-ons are less useful
- [Sublime Text](https://www.sublimetext.com/download)

#### ⬛ Manage your work.
Put all your scripts in categorized folders (mine, not mine, important...), and open them as a "workspace". Or just dump everything in one lol.
![See the workspace on the left?](https://www.tech-recipes.com/wp-content/uploads/2020/07/Notepad-Tips-Tricks-Folder-as-Workspace.jpg)

### B) Variables; if/else; conditional while loops; spawnfunction
**<u>Variables</u> - saves a value (true/false, number, string, function) for later.**
`local IQ = 30` A locally declared variable is within the current script only.
`getgenv().ScriptEnabled = "susan"` Global variables are accessible at any time.
**<u>Loops</u> - run "while" a condition is true, or repeat "for" elements in a table.**
```
while ScriptEnabled do  -- this is already declared as a getgenv() variable
  print(IQ)             -- prints 30
  wait()                -- always have wait() or task.wait() in loops
end
```
```
local skinstable = {               -- tables are enclosed in brackets.
  Dragonlore = true                -- this "dictionary" stores "pairs" of
  Knife = "Karambit"               -- string/number "keys", and "values"
  ["Your IQ"] = 0                  -- numbers or spaces require brackets
}
for k, v in pairs(skinstable) do   -- repeats "for" key, value in table
  print(i, v)
end
```
**<u>Spawn function</u> - begin another simultaneous "thread".**
```

```


Sometimes games control your WalkSpeed and JumpPower by loop-setting it every frame. We can counter this with the video material.
```
local LocalPlayer = game:GetService("Players").LocalPlayer -- defines LocalPlayer. GetService is better than game.Players (game.Players can be renamed; services can't)
local char = LocalPlayer.Character                         -- our workspace model
getgenv().StuffEnabled = true                              -- getgenv(). is better than _G since you have to type out _G whenever you reference a global. Plus, less detectable.
task.spawn(function()                                      -- task.spawn(function() is the newer, less laggy version of spawn. Older stuff fine as long as performance isn't screwy.
  while task.wait() do                                     -- task.wait() is accurate to the frame, and is faster with no arguments than wait().
    if char and char.HumanoidRootPart then                 -- our HRP is an invisible part that usually governs our physics. If it's not there, we're probably dead.
                                                           -- Since it's parented to something that's not guaranteed to exist (our workspace model), we check if that exists first.
      char.Humanoid.WalkSpeed = 32                         -- normal is usually 16. | Our Humanoid holds all of our body parts, and also governs some properties.
      char.Humanoid.JumpPower = 100                        -- normal is usually 50. | Most of LuaU is case sensitive like this!
    end
    if not StuffEnabled then break end                     -- breaks loop if StuffEnabled is set to false or nil.
  end
end
```
`while wait() do` and `break` is common because it's harder to forget to put a `wait()`in your loops to prevent crashes.


## 1.2 Functions; `CFrame` tp; `for` loops
Skip to 8:00 if you don't care to relearn about how `_G` sucks.
> [wyn#0001 Exploiting #2 - Globals, functions, teleporting your player](https://youtu.be/SSqteROpMCw?t=482)

### Review
The video covered:
- Why `_G` sucks (detectable; inefficient), why `getgenv()` is better, and what they actually are (global environment tables)
- Creating functions `function FuncName(arg1, arg2)`. He says that it's for 'cleanliness', but it's also actually for ***"abstraction"*** - you turn an idea into a modular *building block* of code, then forget about its inner workings now that it's 1:1 with some idea.
- Numeric for loops `for i=1,10  do` to iterate something *n* times.
	- You can also use `for i,v in pairs(table)` to iterate over a table.
- Humanoid teleporting `CFrame`
- Basic DarkDex for looking for objects.

### A) Abstraction; `getgenv()` globals; `CFrame` teleporting; `for` loops
Let's make a togglable function that teleports to all players in order, using `getgenv()`, `CFrame`, and functions.
```
getgenv().TeleportToEveryone = true

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

function getHRP(plr)
  if plr and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
    return plr.Character.HumanoidRootPart          -- if the HumanoidRootPart exists, return it and stop.
  end
  return nil                                       -- if it doesn't exist, return nil.
end                                                -- "else" works too, but return stops the function too.

function TeleportToAllPlayers()
  while task.wait() do
    if not TeleportToEveryone then break end
    for i,v in pairs(Players:GetPlayers()) do           -- pairs iterates over the table of all players.
      local hrp1, hrp2 = getHRP(LocalPlayer), getHRP(v) -- Use this other way to declare locals sparingly.
      if hrp1 and hrp2 then                             -- Since the functions return the HRP if it exists,
        hrp1.CFrame = hrp2.CFrame                       -- we don't have to think or type as much.
      end
      task.wait()                                       -- Added so entire for loops don't occur instantly.
    end
  end
end

if TeleportToEveryone then
  TeleportToAllPlayers()
end
```
Since we remembered to check parentage in order to avoid checking the children of something that doesn't exist, we can use getHRP anywhere, since we *abstracted* our code into an idea/building block.

### B) Hold on, what are tables?

Just to clear up any confusion:
#### ⬛These are arrays:
```
local array1 = {"John", "Smith"}
local args = {
  [1] = "ReplaceGear",
  [2] = true,
  [3] = "294281081"
}
```
#### ⬛ Arrays use a *numeric index* to refer to an element of the table.
`print(array1[2])` prints the string `Smith` at index 2 of table `array1`

#### ⬛ You can iterate over an array in order with `ipairs`, `pairs` when order is unimportant, or even a numeric for loop if you need to decrease lag at the expense of simplicity.
```
for i,v in ipairs(args) do   -- from the table "args" above.
  print(v)
end

for i=1, #args do            -- #arrayname returns the total amount of elements in an array (3 here.)
  print(args[i])             -- This kind of lag optimization method is used for enormous tables or
end                          -- extremely quickly-iterating functions.
```
Output: (`table[i]` indexes a table value; `i` is a variable.)
```
ReplaceGear
true
294281081
ReplaceGear
true
294281081
```

#### ⬛ This is a dictionary:
```
local humInfo = {
  Health = 2500,             -- Don't forget your commas here either
  Weapon = "USP_Silenced",
  ["Hidden Weapon"] = false, -- Quotes required - this key has a space in it.
  ["5Head"] = false          -- Quotes required - this key starts with a number.
}
```
#### ⬛ Dictionaries refer to values using *keys* instead of indices. These can be numeric but are usually strings (if they're both all numeric and in order, then it's an array).
```
print(humInfo.Health)    -- Dot syntax will return a value
print(humInfo["Weapon"]) -- Brackets also works, as usual
print(humInfo[Health])   -- This won't work; it thinks you're looking for the value of variable Health.
```
Output:
```
2500
USP_Silenced
nil
```
## 1.3 GUI integration
GUIs simplify the execution of code. You don't want to see something like
`local TheFunctionIWillExecuteAndItsArguments = {"teleport", "WhoIsJoe123"}`
at the top of your code.

If you've never used a GUI before, these videos discuss Wally's UI Library.
> [wyn#0001 Exploiting #3 - GUI Library basics: toggles and buttons](https://www.youtube.com/watch?v=EPixzMICZh8)
> [wyn#0001 Exploiting #4 - GUI Library basics: dropdowns with tables](https://www.youtube.com/watch?v=Ceuv4CzPnCo)

If you have made GUIs before, I recommend this library.
> [Kavo UI](https://github.com/bloodball/UI-Librarys/blob/main/Kavo)
> [Usage documentation](https://xheptcofficial.gitbook.io/kavo-library/)
> Though there are many alternatives such as Wally's UI and Solaris UI among others, this library supports:
> - Easily accessible close button, so you can test a GUI multiple times
> - Buttons, toggles, keybinds, textboxes (that don't only support numbers), basic dropdowns, color pickers
> - Refresh functionality for dropdowns and others
> - A manually inserted keybind that hides and unhides the window, without destroying the GUI. `:ToggleUI()`
> - **Labels** and **information boxes** that support user-friendliness.

# 2. Function Manipulation

We will now learn how to abuse scripts we find in DarkDex to take penetrate actual vulnerabilities in-game. Try not to skip too much here.

## 2.1 LocalScript and ModuleScript environments

LocalScripts and ModuleScripts replicated to the client-side are open to numerous attack vectors. We'll start simple.

>[wyn#0001 Exploiting #5 - ModuleScripts](https://www.youtube.com/watch?v=KdwDt8iRMbk)

### Review
The video covered:
- Finding LocalScripts and ModuleScripts using their symbols on DarkDex. Modules are often in ReplicatedStorage.
- `require()` and dot syntax to access and replace items and functions within ModuleScripts.

In the video, there's a ModuleScript to process gamepasses. Gamepasses are pretty secure; the dev was just foolish to allow such a vulnerability.

### A) Script environments; colon syntax

A script environment is everything in a script. We can access these environments to change the inner workings of the script if anti-penetration defenses are weak and it is available to the client-side for manipulation.

|           | LocalScript | ModuleScript |
| --------- | ----------- | ------------ |
| Purpose | Modify client-side objects that the client needs to know earlier than the server, such as movement physics or GUI objects. | Return tables with preset functions and values in them to whatever "requires" the script, such as GUI creation actions or gun physics.|
| Return the script environment table | `getsenv(FullPath)` | `require(FullPath)`|

In ModuleScripts, actions such as
`RoninKatana.Blade:CalculateHitbox(arg1, arg2)`
are the same as
`RoninKatana.Blade.CalculateHitbox(RoninKatana.Blade, arg1, arg2)`.

This is for complicated object-oriented programming reasons that we don't need to know too precisely.
> Basically, `CalculateHitbox` is a function standardly distributed from somewhere else, and since we can't tailor a rewrite for every single use of `CalculateHitbox`, we need to figure out what the function is actually going to apply itself to using the first argument. Stuff like this is called **"sugar syntax"**, since it just makes the code easier to read and type.

This is also how `FireServer` works for remotes. FireServer is actually only "one" function, so it needs to figure out what remote event it needs to connect to from the first argument.
`game:GetService("ReplicatedStorage").SelfDamage:FireServer(game:GetService("Players").LocalPlayer.Character)`
The function `FireServer` processes the arguments `ReplicatedStorage.SelfDamage` and the local player, so it sends the remote call `SelfDamage` to the server.

### B) Decompiling scripts
Games "compile" human-friendly code into machine-ready code. Reversing this process isn't perfect.
#### ⬛ Decompilation loses many variable names, as well as concise readability. However, function names tend to persist, especially in ModuleScripts. Furthermore, it is still vital to try to understand the decompiled code, unreadable as it may be.
Before compiling (studio-side):
```
local plr = game:GetService("Players").LocalPlayer       -- This LocalScript sets your WalkSpeed to 16.
local char = plr.Character or plr.CharacterAdded:Wait() 
wait(.1)
spawn(function()
 while task.wait() do
   if not char or not char.Humanoid then break end
   if char.Humanoid.WalkSpeed > 16 then
     char.Humanoid.WalkSpeed = 16
   end
end
```
After decompiling (from client-side):
```
local v1 = game
local v2 = ("Players")
local v3 = v1.GetService
local v4 = v3(v1,v2)
local v5 = v4.LocalPlayer
local v7 = wait
local v40 = true
local v27 = 16
local v28 = 0.1
while v40 do
  v7()
  local v6 = v5.Character
  if v6 then
    v40 = false
  end
end
v7(v28)
local v8 = spawn
v8(function()
  while true do
    local v9 = v5.Character
    if v9 then
      local v10 = v9.Humanoid
      if v10 then
        local v11 = v9
        local v12 = v11.Humanoid
        if v12.WalkSpeed > v27 then
          v12.WalkSpeed = v27
        end
      else
        break
      end
    else
      break
    end
    v7()
  end
end)
```
It's horrid! Although readability is greatly reduced, you can still get somewhat of a sense of what the function does. Are there connections? Does math occur? Where does it mention our humanoid?
## 2.2 Basic upvalues and constants
How do we spoof the values that functions handle?
> [wyn#0001 Exploiting #6 - Upvalues and Constants](https://www.youtube.com/watch?v=sAjmwhmGgeU)

##### Modifying pet capacity like he did at the end of the video is untrustworthy. It may only work client-side and not be recognized by the server giving out pets, or even disappear upon rejoin causing pet losses. In the example game, the vulnerability was patched after the video.
### Review
The video covered:
- What upvalues and constants are
- Getting and setting constants and upvalues

##### Sometimes functions cannot be directly manipulated via accessing a script environment using its path. Circumventing this issue requires `getgc()`, which we will discuss later.

### A) Constants; upvalues
| Upvalues | Constants |
| -------- | --------- |
| Born outside the function | Born within the function |
| `getupvalues(funcPath)` | `getconstants(funcPath)` |
| `setupvalue(funcPath, index, value)` | `setconstant(funcPath, index, value)`|
Other than the change in name for their respective functions, you use them in the same manner for either kind of value.
[Read this as well](https://x.synapse.to/docs/development/debug_api.html). It is very concise.

## 2.3 Tables; metatables; hooks

A metatable sounds scary, but it's just instructions given to a table on what to do when something interacts with it.
> Try to understand basic metatables here, so you at least have a foundation to build upon before going to metamethod hooks:
> [DevForum - Metatables and Metamethods](https://devforum.roblox.com/t/all-you-need-to-know-about-metatables-and-metamethods/503259) (section 1, 2, 3, maybe 6)
> Or, live on the edge and continue.

So, metatables give instructions before failing something that normally should fail. Beyond the standard metamethods like `#tbl` to count elements in an array, you can do things like making similar words do the same command.
```
cmds = {
  remotespy = function() loadstring("print('I forgor the link to rspy')")() end
}
othercmds = {
  rspy = "remotespy",
  spy = "remotespy"
}
setmetatable(cmds, {
  __index = function(tbl, index)
    if othercmds[index] then return cmds[othercmds[index]] end
  end
})
cmds["rspy"]()
```

