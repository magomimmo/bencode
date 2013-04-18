# bencode

Clojure implementation of [Bencode](http://bittorrent.org/beps/bep_0003.html#bencoding),
the encoding used by BitTorrent for storing and transmitting loosely structured data.


## Installation

Add the following dependency to your _project.clj_ file:

````clojure

[bencode "0.0.1"]
````


## Usage

First, import the `bencode.core` namespace:

````clojure

(use '[bencode.core])
````

At this point, you should be able to use `bencode` and `bdecode` functions for
encoding and decoding, respectively:

````clojure

(bencode {:cow "moo" :spam ["info" 32]})
;; -> "d3:cow3:moo4:spaml4:infoi32eee"

(bdecode "d3:cow3:moo4:spaml4:infoi32eee")
;; -> {:cow "moo", :spam ["info" 32]}
```


### Encoding Options

The `bencode` function also accepts an optional map:

````clojure

(bencode "moo" {:raw-str? true})
;; -> #<byte[] [B@53c059f6>
````

At this point, these are the exposed options:

* `:to` - Instance of `OutputStream` where the encoding result should be
  written to. Default: `nil`
* `:raw-str?` - Whether the string being encoded should be returned as a
  byte array. This option can only be used if the option `:to` is absent.
  Default: `false`


### Decoding Options

The `bdecode` function also accepts an optional map:

````clojure

(bdecode "d3:cow3:moo4:spaml4:infoi32eee", {:str-keys? true :raw-keys ["spam"]})
;; -> {"cow" "moo", "spam" [#<byte[] [B@74184b3b> 32]}
````

At this point, these are the exposed options:


* `:str-keys?` - Whether strings should be used as dictionary keys instead of
  keywords. Default: `false`
* `:raw-keys` - List containing all dictionary keys whose values should be
  decoded as raw strings instead of UTF-8-encoded strings. Default: `nil`


### Supported Data Types

According to the Bencoding spec, only _strings_, _integers_, _lists_ and
_dictionaries_ are supported. Furthermore, only strings can be used as
keys in a dictionary, and the keys must appear in sorted order (sorted as raw
strings, not alphanumerics).

On the Clojure side, _keywords_ are encoded as _strings_, _sets_ and _vectors_
are encoded as _lists_, and all integers - _byte_, _short_, _int_, _long_,
_big integers_ - are encoded as _integers_ with arbitrary size, and decoded to
the smallest type which can hold the number without losing data.


## License

Copyright (C) Daniel Fernandes Martins

Distributed under the New BSD License. See COPYING for further details.