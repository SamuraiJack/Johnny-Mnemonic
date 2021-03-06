Name
====

Johnny.Mnemonic - Yet another "ajax back button" implementation, written on Joose3, with the test suite and no required page markup at this time


SYNOPSIS
========


        var mnemonic = new Johnny.Mnemonic({
            defaultToken    : '/',
            hashFrom        : 'window'
        })
        
        mnemonic.on('statechange', function (mnemonic, token) {
            
            this.syncState(token)
        })
        
        mnemonic.setup()
        
        -or-

        var mnemonic = new Johnny.Mnemonic({
            defaultToken : '/'
        })
        
        mnemonic.setup(function () {
        
            ...
        
            mnemonic.on('statechange', function (mnemonic, token) {
                this.syncState(token)
            })
        })
        
        -later-
        
        mnemonic.remember(stateToken)
        
        mnemonic.back()
        mnemonic.forward()


DESCRIPTION
===========

Johnny.Mnemonic is a yet another "ajax back button" implementation, with the focus on maintainability. 

Its source code is relatively clean, and, (the most important) it has an automated test suite - this makes it different from others solutions in this area.

Another difference is that Mnemonic do not requires any additional page markup or assets - its a pure javascript solution. 

Currently works on everything except Opera (patches welcome).


IMPLEMENTATION DETAILS
======================

Mnemonic can remember arbitrary state tokens (strings), inserting them in the browser's history along the way.
Tokens are inserted in the hash part of current URL (after the `#` sign).

When user will press 'back/forward' button in the browser, mnemonic will "recall" in what state the application was at that step, and notify your application 
via its `statechange` event, on which you should listen. You then can synchronize the application state in the event handler.

About events: Johnny.Mnemonic is a subclass of Ext.util.Observable, please refer to its documentation (see below) for details.   


PROPERTIES
==========

- `defaultToken`

As a special case, the empty hash value is always treated as "default token" and the very initial state of the application. 

Note: You don't need to call the `remember` method with the initial state - instead, supply it with this property.

- `hashFrom`

The string, referencing the target window, which `location` object mnemonic should examine. Can be either `'window'` or `'top'`. Default value is `'top'`, what should enable
the usage from iframes.

- `exclamation`

Default to `true`. Boolean value, indicating, whether mnemonic should prepend the hash value withe the exclamation like: '#!/' to conform with google recommendations.

 
METHODS
=======

- `setup([readyFunc], [readyScope])`

Should be called once to initialize the mnemonic instance. Accept the optional callback and its scope, which will be called after the initialization completion.

This callback is the last place to subscribe on the `statechange` event - immediately after it, will be fired the 1st event, with the initial state of the application.


- `remember(token)`

Saves the passed `token` in the browser's history, making its available for 'back/forward' buttons. Also displays the token in the hash part of URL.

Note: To avoid creation of extra history step, the hash will not be modified for the initial state, when application was loaded with empty hash.


- `back()`

Equivalent of pressing "back" button in browser. Will switch the history on one step back and fire the `statechange` event with the according token.
You can also call the `back` method of `history` object directly.

- `forward()`

Equivalent of pressing "forward" button in browser. Will switch the history on one step forward and fire the `statechange` event with the according token.
You can also call the `forward` method of `history` object directly.


EVENTS
======

- `statechange(mnemonic, token)`

Will be fired with 2 parameters above. Application should listen on this event and synchronize its state according to received state token.

Note: This event will be also fired for the initial state of the application. So, generally, you shouldn't treat the initial state specially, just
define a correct synchronization handler. 

The last place to subscribe on this event is the callback passed to `setup`. Immediately after that callback execution, will be fired the event with the initial state.  


- `ready(mnemonic)`

Will be fired when 'setup' method will initialize the mnemonic.


GETTING HELP
============

This extension is supported via github issues tracker: <http://github.com/SamuraiJack/Johnny-Mnemonic/issues>

For general Joose questions you can also visit #joose on irc.freenode.org. 


SEE ALSO
========

[Documentation for Joose](http://joose.github.com/Joose/doc/html/Joose.html)

[Documentation for JooseX.Observable extension](http://samuraijack.github.com/JooseX-Observable/doc/html/JooseX/Observable.html)

[Dedicated to William Gibson](http://project.cyberpunk.ru/lib/johnny_mnemonic/)



BUGS
====

Currently, test suite can't be ran under Chrome - seems it doesn't allow manipulations with window's history from another window.
While pressing the 'back/forward' buttons manually, it works fine though.

All complex software has bugs lurking in it, and this module is no exception.

Please report any bugs through the web interface at <http://github.com/SamuraiJack/Johnny-Mnemonic/issues>


AUTHORS
=======

Nickolay Platonov [nplatonov@cpan.org](mailto:nplatonov@cpan.org)



COPYRIGHT AND LICENSE
=====================

Copyright (c) 2010, Nickolay Platonov

All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
* Neither the name of Nickolay Platonov nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission. 

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE. 
