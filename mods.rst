Modules
==================

.. highlight:: lua

.. _emu:
    
emu
------------------

The ``emu`` module contains general emulator functions.

* :ref:`frameadvance <frameadvance>`
* :ref:`gamecode <gamecode>`
* :ref:`smallgamecode <smallgamecode>`
* :ref:`speedmode <speedmode>`
* :ref:`wait <wait>`
* :ref:`pause <pause>`
* :ref:`unpause <pause>`
* :ref:`emulateframe <emulateframe>`
* :ref:`emulateframefastnoskipping <emulateframefastnoskipping>`
* :ref:`emulateframefast <emulateframefast>`
* :ref:`emulateframeinvisible <emulateframeinvisible>`
* :ref:`redraw <redraw>`
* :ref:`getframecount <getframecount>`
* :ref:`getlagcount <getlagcount>`
* :ref:`lagged <lagged>`
* :ref:`emulating <emulating>`
* :ref:`atframeboundary <atframeboundary>`
* :ref:`registerbefore <registerbefore>`
* :ref:`registerafter <registerafter>`
* :ref:`registerstart <registerstart>`
* :ref:`registerexit <registerexit>`
* :ref:`persistglobalvariables <persistglobalvariables>`
* :ref:`message <message>`
* :ref:`print <print>`
* :ref:`openscript <openscript>`
* :ref:`reset <reset>`
* :ref:`addmenu <addmenu>`
* :ref:`setmenuiteminfo <setmenuiteminfo>`
* :ref:`registermenustart <registermenustart>`
* :ref:`register3devent <register3devent>`
* :ref:`set3dtransform <set3dtransform>`

.. _frameadvance:

``emu.frameadvance()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    
Steps emulation one frame. Can be used when overriding the main emulation loop.

.. _gamecode:

``emu.gamecode()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    
Retrieves the full game code of the currently loaded ROM as a string.

.. _smallgamecode:

``emu.smallgamecode()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    
Retrieves the game code excluding the region code of the currently loaded ROM as a string.

.. _speedmode:

``emu.speedmode(mode)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``mode`` is a string with a value of any of {``normal``, ``nothrottle``, ``turbo``, ``maximum``}.

Sets the emulation speed when using ``emu.frameadvance``.

.. _wait:

``emu.wait()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Tells the emulator to wait while the script does calculations. Note that hotkeys are not disabled, so e.g. a savestate could still be loaded while the emulator is waiting.

.. _pause:

``emu.pause()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pauses emulation.

.. _unpause:

``emu.unpause()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Unpauses emulation.

.. _emulateframe:

``emu.emulateframe()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Steps one frame. Equivalent to::

    emu.setspeed("normal")
    emu.frameadvance()

.. _emulateframefastnoskipping:

``emu.emulateframefastnoskipping()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Fast-forwards emulation once but render every frame. Equivalent to::

    emu.setspeed("nothrottle")
    emu.frameadvance()

.. _emulateframefast:

``emu.emulateframefast()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Fast-forwards emulation once. Equivalent to::

    emu.setspeed("turbo")
    emu.frameadvance()

.. _emulateframeinvisible:

``emu.emulateframeinvisible()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Extremely-fast-forwards emulation once. Equivalent to::

    emu.setspeed("maximum")
    emu.frameadvance()

.. _redraw:

``emu.redraw()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Redraws the current frame.

.. _getframecount:

``emu.getframecount()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gets the current frame count as an integer.

.. _getlagcount:

``emu.getlagcount()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gets the current total lag frames as an integer.

.. _lagged:

``emu.lagged()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns true if the current frame is a lag frame.

.. _emulating:

``emu.emulating()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns true if the emulator is currently running.

.. _atframeboundary:

``emu.atframeboundary()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns true if the emulator is at a frame boundary.

.. _registerbefore:

``emu.registerbefore(func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``func`` is a function taking no arguments.

Sets up a callback function to run before every frame starts. Can be used to e.g. set up memory or controller inputs for the next frame.

Example::

    function myCallback()
        print("Frame starting")
    end
    
    emu.registerbefore(myCallback)

.. _registerafter:

``emu.registerafter(func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``func`` is a function taking no arguments.

Sets up a callback function to run after every frame ends. Can be used to e.g. read out memory or controller dataa for the previous frame.

Example::

    function myCallback()
        print("Frame ending")
    end
    
    emu.registerafter(myCallback)

.. _registerstart:

``emu.registerstart(func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``func`` is a function taking no arguments.

Sets up a callback function to run on script start or reset. Does not work correctly with script start.

Example::

    function myCallback()
        print(emu.gamecode())
        file = io.open("log.txt", "w+")
    end
    
    emu.registerstart(myCallback)

.. _registerexit:

``emu.registerexit(func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``func`` is a function taking no arguments.

Sets up a callback function to run on script end.

Example::

    function myCallback()
        file:close()
    end
    
    emu.registerexit(myCallback)

.. _persistglobalvariables:

``emu.persistglobalvariables(variabletable)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``variabletable`` is a table comprising keys and values.

Defines a global variable for each key in ``variabletable``. The variables will be set to the values they had the last time the script exited, or the value provided in the table if that is not available.
To set a default value of ``nil`` for a variable, pass the variable name as a string with no associated value.

Example::

    emu.persistglobalvariables({
        variable1 = defaultvalue1,
        variable2 = defaultvalue2,
        -- ...
    })

.. _message:

``emu.message(str)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``str`` is a string with the message to be displayed.

Displays an info message to the user.

.. _print:

``emu.print(...)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Replacement for ``luaB_print()`` that outputs to the appropriate textbox instead of stdout.

.. _openscript:

``emu.openscript(filename)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``filename`` is a string containing the filepath of the script to open.

Opens a new Lua script. Only available in the Windows frontend.

.. _reset:

``emu.reset()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Resets the currently loaded ROM.

.. _addmenu:

``emu.addmenu(menuName, menuEntries)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``menuName`` is a string containing the name the new menu should have. ``menuEntries`` is a table containing the entries that the menu will have.

TODO: add an example for this.

.. _setmenuiteminfo:

``emu.setmenuiteminfo(menuItem, infoTable)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``menuItem`` is a... actually, I really have no idea.

TODO: work out how the hell this function works

.. _registermenustart:

``emu.registermenustart(func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``func`` is a function taking ?? arguments.

TODO: literally all of the menu stuff

.. _register3devent:

``emu.register3devent(func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``func`` is a function taking ?? arguments.

TODO: figure out 3D stuff

.. _set3dtransform:

``emu.set3dtransform(mode, matrix)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``mode`` is a number with a value of either 2 or 3. ``matrix`` is a camera matrix.

TODO: 3D

.. _gui:
    
gui
------------------

.. _stylus:
    
emu
------------------

.. _savestate:
    
savestate
------------------


.. _memory:
    
memory
------------------

The ``memory`` module contains functions related to game memory.

* :ref:`readbyte <readbyte>`
* :ref:`readbytesigned <readbytesigned>`
* :ref:`readword <readword>`
* :ref:`readwordsigned <readwordsigned>`
* :ref:`readdword <readdword>`
* :ref:`readdwordsigned <readdwordsigned>`
* :ref:`readbyterange <readbyterange>`
* :ref:`writebyte <writebyte>`
* :ref:`writeword <writeword>`
* :ref:`writedword <writedword>`
* :ref:`isvalid <isvalid>`
* :ref:`getregister <getregister>`
* :ref:`setregister <setregister>`
* :ref:`vram_readword <vram_readword>`
* :ref:`vram_writeword <vram_writeword>`
* :ref:`registerwrite <registerwrite>`
* :ref:`registerread <registerread>`
* :ref:`registerexec <registerexec>`

.. _readbyte:

``memory.readbyte(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a byte in the ARM9 CPU's address space.

Reads one byte from address ``address``.

.. _readbytesigned:

``memory.readbytesigned(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a signed byte in the ARM9 CPU's address space.

Reads one signed byte from address ``address``.

.. _readword:

``memory.readword(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a word in the ARM9 CPU's address space.

Reads one word (16 bits) from address ``address``.

.. _readwordsigned:

``memory.readwordsigned(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a signed word in the ARM9 CPU's address space.

Reads one signed word (16 bits) from address ``address``.

.. _readdword:

``memory.readdword(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a dword in the ARM9 CPU's address space.

Reads one dword (32 bits) from address ``address``.

.. _readdwordsigned:

``memory.readdwordsigned(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a signed dword in the ARM9 CPU's address space.

Reads one signed dword (32 bits) from address ``address``.

.. _readbyterange:

``memory.readbyterange(address, length)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the start of the block to read. ``length`` is an integer representing the length of the block to read.

Reads ``length`` bytes from address ``address``. Returns an array.

.. _writebyte:

``memory.writebyte(address, value)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a byte in the ARM9 CPU's address space. ``value`` is the byte to write there as an integer.

Writes the byte ``value`` to address ``address``.

.. _writeword:

``memory.writeword(address, value)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a word in the ARM9 CPU's address space. ``value`` is the word to write there as an integer.

Writes the word ``value`` to address ``address``.

.. _writedword:

``memory.writedword(address, value)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of a dword in the ARM9 CPU's address space. ``value`` is the dword to write there as an integer.

Writes the dword ``value`` to address ``address``.

.. _isvalid:

``memory.isvalid(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing an address in the ARM9 CPU's address space.

Returns ``true`` if ``address`` is a valid hardware address, else ``false``.

Example::

    function getString(address)
        if memory.isvalid(address) then
            local index = 1
            local str = ""
            local c = memory.readbyte(address)
            while c ~= 0 do
                str = str .. string.char(c)
                c = memory.readbyte(address + index)
                index = index + 1
            end
            return str
        end
        return nil
    end

.. _getregister:

``memory.getregister(cpu_dot_registername_string)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``cpu_dot_registername_string`` is a string representing the register to read. The format is ``<CPU>.<register>``, where ``CPU`` is "arm9" or "arm7" (or "main" or "sub", respectively) and ``register`` is ``r0``-``r15``, ``cpsr`` or ``spsr``.

Returns the contents of the register referenced by ``cpu_dot_registername_string``.

Example::

    function debugPrintHook()
        local strAddr = memory.getregister("arm9.r0")
        if strAddr ~= 0 then
            local str = getString(strAddr)
            if str ~= nil then
                print(str)
            end
        end
    end

.. _setregister:

``memory.setregister(cpu_dot_registername_string, value)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``cpu_dot_registername_string`` is a string representing the register to write. The format is ``<CPU>.<register>``, where ``CPU`` is "arm9" or "arm7" (or "main" or "sub", respectively) and ``register`` is ``r0``-``r15``, ``cpsr`` or ``spsr``. ``value`` is the data to write to it, as an integer.

Writes ``value`` to the register referenced by ``cpu_dot_registername_string``.

.. _vram_readword:

``memory.vram_readword(address)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is the address of a word in the console's VRAM address space.

Reads one word (16 bits) from VRAM address ``address``.

.. _vram_writeword:

``memory.vram_writeword(address, value)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is the address of a word in the console's VRAM address space. ``value`` is the word to write there.

Writes one word (16 bits) to VRAM address ``address``.

.. _registerwrite:

``memory.registerwrite(address[, size = 1][, cpuname = "main"], func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing an address in the chosen CPU's address space. ``size`` is an integer representing the length of the memory region that should be registered. ``cpuname`` is a string containing the name of the CPU to register to, with a value of either "main" or "sub" (or "arm9" or "arm7", respectively). ``func`` is a function taking no arguments.

Registers a memory write watchpoint with size ``size`` at address ``cpuname``:``address``. When ``cpuname``:``address`` is written, ``func`` will be called.

TODO: verify whether ``size`` and ``cpuname`` are used at all (they don't seem to be).

.. _registerread:

``memory.registerread(address[, size = 1][, cpuname = "main"], func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing an address in the chosen CPU's address space. ``size`` is an integer representing the length of the memory region that should be registered. ``cpuname`` is a string containing the name of the CPU to register to, with a value of either "main" or "sub" (or "arm9" or "arm7", respectively). ``func`` is a function taking no arguments.

Registers a memory read watchpoint with size ``size`` at address ``cpuname``:``address``. When ``cpuname``:``address`` is read, ``func`` will be called.

TODO: verify whether ``size`` and ``cpuname`` are used at all (they don't seem to be).

.. _registerexec:

``memory.registerexec(address[, size = 2][, cpuname = "main"], func)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``address`` is an integer representing the address of an instruction in the chosen CPU's address space. ``size`` is an integer representing the length of the memory region that should be registered. ``cpuname`` is a string containing the name of the CPU to register to, with a value of either "main" or "sub" (or "arm9" or "arm7", respectively). ``func`` is a function taking no arguments.

Registers a memory exec watchpoint with size ``size`` at address ``cpuname``:``address``. When ``cpuname``:``address`` is executed, ``func`` will be called.

TODO: verify whether ``size`` and ``cpuname`` are used at all (they don't seem to be).

Example::

    memory.registerexec(0x02103ea8, debugPrintHook)

.. _joypad:
    
joypad
------------------


.. _input:
    
input
------------------

.. _movie:
    
movie
------------------


.. _sound:
    
sound
------------------

.. _bit:
    
bit
------------------


.. _agg:
    
agg
------------------

.. _controller:
    
controller
------------------

