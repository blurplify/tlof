# MODULE SYNTAX:
-------------------
Core Game Functions:
-------------------

**core:exists(file),**
`file` being the file name. 
> This function returns whether or not a file exists.
>
> If it doesn't exist, it will also return an error code.

**core:getOS()**
> Makes use of `core:exists()` and returns the current operating system.
>
> Otherwise, it will return nil.

**core:getNthLine(filename,n),** `filename` being the name, `n` being the line position.
> Will return the text written on the desired line of file `filename`.
>
> If there are too little lines (if the function doesn't `return` anything), an error will be thrown.

**core:dostring(s),** `s` being the code to be ran.
> Equivalent to `return assert(loadstring())()`.
>
> Runs code with an accurate error message, rather than "attempt to *verb* a nil value".

**core:ReturnLettersAsTable(s),** `s` being a string.
> Splits the string via the use of `string:sub()` with an increasing, number turned negative.
>
> The substring will be put into a table that will be returned once the number is capped.

**core:IsInList(item,list),** `item` being any value, `list` being a table.
> Iterates through `list` and checks if the `value == item`. If so, it will return true.

**core:GetPositionOfItemFromList(item,list),** `item` being any value, `list` being a table.
> Grabs an item via the same method of `core:IsInList()` and returns its key.

**core:AddToList(item,list),** `item` being any value, `list` being a table.
> Equivalent to `list[#list+1] = item`. 
>
> An alternative to `table.insert()` that doesn't require a position parameter.

**core:RemoveFromList(item,list),** `item` being any value, `list` being a table.
> Grabs the item and sets it to nil, then moves every proceeding item back.
>
> An alternative to `table.remove()` that doesn't require a position parameter.

**core:StringToList(s),** `s` being a string.
> A self explanatory function that splits the string by spaces.
>
> The each item in the returned table will contain the respective words.

**core:ListToString(list),** `list` being a table.
> The equivalent of `table.concat()` with a space as the seperator.

**core:wrap(s,maxchars,maxlines,cut).**
- `s` being a string, `maxchars` being the length of a line, `maxlines` being the total lines.
- If `cut` is not omitted, the string will cut off if the line's next word exceeds `maxchars` instead.

> Inserts a `\n` every `maxchars` for `maxlines`. If `cut` is omitted, words will be split early.

**core:promptdialogue(speaker,tone,speech,animated,autoskip).**
- Strings `speaker` being the "author", `tone` denoting the sprite, `speech` being the text.
- `animated` and `autoskip` being booleans. Both can be omitted.
    - If `animated` is true, the text will display another character every frame.
    - If `autoskip` is true, the dialogue will advance after the text finishes.
        - Will skip immediately if `animated` isn't true.

> Displays a textbox with the character on the left and the speech next to it.
>
> The path for the character is `game\assets\sprites\textbox\speaker\speaker_tone`.

**core:sleep(s),** `s` being a float.
> Yields the current block of code for `s` ms.

**core:wait(s),** `s` being an integer or number.
> A streamlined variant of `core:sleep()`. Yields the current block of code for `s` seconds.

**core:foldRGB(rgb)**, `rgb` being a table.
> Less lenient of a function. Runs if `rgb` is a table with 3 values.
>
> If so, each value is divided by 255 and the table is returned.
>
> No checks are made to see if the values are numbers.

**core:gameover()**
> Triggers the gameover screen and plays the music and a random jingle.

**core:loadgame(file),** `file` being the desired/target name. Can be omitted.
> Loads and returns the requested game file. If no file is found, a new one will be made.
>
> If `file` is omitted, a file by the name of "NewGame" will be made.
>
> Due to its handling of omitted parameters, it doubles as a file creation function.

**core:savegame(file),** `file` being the file name.
> Finds and updates the data in `"game\files\" .. file .. "\data.txt"`.
>
> If no file is found, or the data fails to save, the error will be printed to the console.

Event System (ES):
------------------

**es.events:** A table with all established events.

**es.events.queue:** A table with all events queued for firing.

**es:create(name),** `name` being a string.
> Creates an event by the name of `name`.

**es:setconditions(name,conditions,auto),**
- `name` being the name of the event.
- `conditions` being a function, that must return true
- `auto` being a boolean. Determines if the event fires as soon as the conditions are met.
    - Can be omitted.

> The function connected will be called every time the event is queued.
>
> If auto is on, the event fires if the function returns true.
>
> Otherwise, the function will simply gatekeep the firing rather than initiating it.

**es:connect(name,func),** `name` being the name of the event, `func` being a function.
> Function `func` will be called after the event **fires**, as opposed to before with conditions.

*An alternative to this function is `es:on(name,func)`*

**es:fire(name),** `name` being the name of the event.
> Runs conditions, if any, and if said conditions are met, calls connected functions, if any.

**es:disconnect(name,func),** `name` being the name of the event, `func` being a function.
> If `func` is not omitted, it will no longer be called after the event fires.
>
> Otherwise, the event will be deleted entirely.

*An alternative to this function is `es:remove(name,func)`*

**es:queue(name,returns),** `name` being the name of the event, `returns` being a table.
> Parameter `returns` only exists if you need to pass things from within your custom events.
>
> The `returns` should be omitted, and is only there for any specific use cases that need it.
>
> Other than that, this function simply stores the events for handling.

**es:handle()**
> Should be ran constantly under a loop. Events will not fire if they aren't handled.
>
> Checks the queue, fires the queued events, and empties said queue.

Map Data:
---------

**maps:format(mapstr),** `mapstr` being a string.
> Maps are encoded as strings to be made easily editable and readable.
>
> This function simply replaces the new lines and spaces for easy iteration.
>
> Said string is placed into a complex table with rows and columns for easy reference.

**maps:draw(world,area),** `world` and `area` being strings.
> The function searches for the `mapstr` that is `maps[world][area]`.
>
> If it exists, the resulting table from `maps:format()` is iterated through and drawn.
>
> The `drawpos` of each individual tile increases in a set pattern for seamless placement.