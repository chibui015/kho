Much has been said about 2D barcodes, and the discussion has focused on the format of the 2D barcode itself -- QR Code, Data Matrix, and so on. But equally important is the format of what the barcode itself encodes.

2D barcodes encode text, generally, but that text can represent many things. Commonly, 2D barcodes encode text that represents a URL, like `https://google.com/m`. This is a special string of text since it is recognizable as a URL by readers, and therefore can be acted upon: the reader can open the URL in a browser.

2D barcodes can encode many types of actionable text. Text representing contact information, when recognized, could trigger a prompt to add the contact to an address book. But this only works when readers understand that text encodes contact information. For this, we need standards too.

There are some standards -- de facto and otherwise -- already in use. This wiki attempts to catalog some possible standards for encoding various types of information, and suggest a standard action associated to them. It is not necessarily complete and contributions are welcome.

The ZXing reader library supports all of the formats mentioned in this wiki and a bit more.

## URL

The most common application of barcodes is to encode the text of URL such as `https://google.com/m`. To do so, simply encode exactly the text of the URL in the barcode: `https://google.com/m`. Include the protocol (`https://` here) to ensure it is recognized as a URL.

Readers should open the URL in the device's web browser when decoding a URL. It is probably desirable for a reader to display the URL and ask the user whether to proceed, so that the user may see the URL before accessing it.

URLs prefixed with `URLTO:` have been observed "in the wild" (e.g. `URLTO:google.com/m`). 

It's interesting to note that, actually, QR Codes can encode data more efficiently in some cases if only uppercase letters are used. That's because it has a special encoding mode for text consistent of only (uppercase) letters, numbers, and common symbols. It may be advantageous to encode a URL like `HTTP://MYSITE.COM/FOO...` rather than `http://mysite.com/foo...` for this reason. However, this depends upon the web server responding to requests correctly when the URI is uppercased. It's not necessarily true that URIs and paths are treated as case-insensitive by a web server, since URIs are technically case sensitive. Don't try this unless you test it to know it works.

Note that NTT DoCoMo uses a [MEBKM bookmark format](https://web.archive.org/web/20160213153725/https://www.nttdocomo.co.jp/english/service/developer/make/content/barcode/function/application/bookmark/) to express not only a URL but title. This could be treated like a URL, or, prompt the user to add a browser bookmark.

## E-mail address

To encode an e-mail address like `sean@example.com`, one could simply encode `sean@example.com`. However to ensure it is recognized as an e-mail address, it is advisable to create a proper `mailto:` URL from the address: `mailto:sean@example.com`.

All of the standard `mailto:` tricks/links should work here as well. Readers should open a blank e-mail message to the given address.

```plain
# Address
mailto:someone@yoursite.com

# Address, subject
mailto:someone@yoursite.com?subject=Mail%20from%20Our%20Site

# Address, CC, BCC, subject
mailto:someone@yoursite.com?cc=someoneelse@theirsite.com,another@thatsite.com,me@mysite.com&bcc=lastperson@theirsite.com&subject=Big%20News

# Address, CC, BCC, subject, body
mailto:someone@yoursite.com?cc=someoneelse@theirsite.com,another@thatsite.com,me@mysite.com&bcc=lastperson@theirsite.com&subject=Big%20News&body=Body%20goes%20here.
```

See also…

* [CSS-Tricks: Mailto Links](https://css-tricks.com/snippets/html/mailto-links/)
* [Apple URL Scheme Reference: Mail Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MailLinks/MailLinks.html#//apple_ref/doc/uid/TP40007899-CH4-SW1)
* [Android: Common Intents](https://developer.android.com/guide/components/intents-common#Email)
* [RFC 6068](https://tools.ietf.org/html/rfc6068)

Note that NTT DoCoMo has standardized a more expressive [MATMSG format](https://web.archive.org/web/20130330190304/http://www.nttdocomo.co.jp:80/english/service/developer/make/content/barcode/function/application/mail/) for encoding an e-mail address, subject, and message.

## Telephone numbers

A `tel` URI should be used to encode a telephone number, to ensure that the digits are understood as a telephone number. Further, you should generally use the most complete version of a telephone number possible (i.e., country code + area code + number). For example, to encode the US phone number 212-555-1212, one should encode `tel:+1-212-555-1212`. This tel URI includes a "+1" prefix that will make it usable outside the United States.

Readers should invoke the device's dialer, if applicable, and pre-fill it with the given number, but not automatically initiate a call.

```plain
# NYC Directory assistance
tel:+12125551212
```

See Also…

* [CSS-Tricks: The Current State of Telephone Links](https://css-tricks.com/the-current-state-of-telephone-links/)
* [Apple URL Scheme Reference: Phone Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/PhoneLinks/PhoneLinks.html)
* [Android: Common Intents](https://developer.android.com/guide/components/intents-common#Phone)

## Contact information

We have, for example, the [vCard](https://en.wikipedia.org/wiki/VCard) format for encoding contact information as text. This format proves a bit verbose for use in 2D barcodes, whose information capacity is limited. It is not clear whether vCard is or should be used to encode contact information.

NTT DoCoMo has popularized a compact [MECARD format](https://web.archive.org/web/20130712164414/http://www.nttdocomo.co.jp:80/english/service/developer/make/content/barcode/function/application/addressbook/) for encoding contact information. For example, to encode the name Sean Owen, address "76 9th Avenue, 4th Floor, New York, NY 10011", phone number "212 555 1212", e-mail `srowen@example.com`, one would encode this in a barcode:

```
MECARD:N:Owen,Sean;ADR:76 9th Avenue, 4th Floor, New York, NY 10011;TEL:12125551212;EMAIL:srowen@example.com;;
```

See the link above for complete information. Note that this format has generally been used with QR Codes only in the past, but, may well be parsed and understood when encoded in a Data Matrix symbol too.

Readers should open a new address book entry, populated with the given data, and prompt the user to add a new contact.

### AU format

Note that KDDI AU also proposes a [slightly different format](http://www.au.kddi.com/ezfactory/tec/two_dimensions/address.html) for this information.

### BIZCARD

We have also observed a BIZCARD format in use but have yet to locate a definitive reference on the format. One can guess its structure from a few examples; it resembles the MECARD format:

```
BIZCARD:N:Sean;X:Owen;T:Software Engineer;C:Google;A:76 9th Avenue, New York, NY 10011;B:+12125551212;E:srowen@google.com;;
```

### vCard

Many apps understand the [vCard specification](https://en.wikipedia.org/wiki/VCard). The wiki for [mangstadt/ez-vcard](https://github.com/mangstadt/ez-vcard) provides documentation for [Version differences](https://github.com/mangstadt/ez-vcard/wiki/Version-differences) and [Property lists](https://github.com/mangstadt/ez-vcard/wiki/Property-List) between the various versions of the vCard specification.

```vcard
BEGIN:VCARD
VERSION:3.0
N:Owen;Sean;;;
FN:Sean Owen
TITLE:Software Engineer
EMAIL;TYPE=INTERNET;TYPE=WORK;TYPE=PREF:srowen@google.com
URL;TYPE=Homepage:https://example.com
END:VCARD
```

## SMS/MMS/FaceTime

Much like an e-mail address, one can encode an SMS shortcode or number by creating an `sms` URI. For example to create a link to the number 12345 one would encode `sms:12345`. See [RFC 5724](https://tools.ietf.org/html/rfc5724) for details.

Likewise `SMSTO:` URLs have been observed in the wild, though I am still looking for an official specification for it. Follow this with the number to SMS. We have heard of URIs of the form `sms:[number]:[subject]`, and likewise for other prefixes like `smsto:`.

There appear to be "mms:" and "MMSTO:" URIs used like "sms:" URIs in practice. We assume the format is the same, and that the reader should react similarly to such a URI.

Readers should open a new SMS message, ready for the user to compose and send it.

```plain
# Send an SMS/MMS to a number
sms:+18005551212

# Send an SMS/MMS to a number with pre-filled message.
sms:+18005551212:This%20is%20my%20text%20message.

# FaceTime Video
facetime:+18005551212
facetime:me@icloud.com

# FaceTime Audio
facetime-audio:+18005551212
facetime-audio:me@icloud.com
```

See Also…

* [CSS-Tricks: iPhone Calling and Texting Links](https://css-tricks.com/snippets/html/iphone-calling-and-texting-links/)
* [Apple URL Scheme Reference: SMS Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/SMSLinks/SMSLinks.html#//apple_ref/doc/uid/TP40007899-CH7-SW1)
* [Apple URL Scheme Reference: FaceTime Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/FacetimeLinks/FacetimeLinks.html#//apple_ref/doc/uid/TP40007899-CH2-SW1)
* [Android: Common Intents](https://developer.android.com/guide/components/intents-common#Messaging)

## Maps, Geographic information

A `geo` URI may be used to encode a point on the earth, including altitude. For example, to encode the Google's New York office, which is at 40.71872 deg N latitude, 73.98905 deg W longitude, at a point 100 meters above the office, one would encode "geo:40.71872,-73.98905,100".

A reader might open a local mapping application like Google Maps to this location and zoom accordingly, or could open a link to this location on a mapping web site like Google Maps in the device's web browser.

Services like [Apple Maps](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html#//apple_ref/doc/uid/TP40007899-CH5-SW1) and [Google Maps](https://developers.google.com/maps/documentation/maps-static/intro) have more thorough implemenations with more options. These are presented as normal HTTPS-based URLs, but are generally handled by the built-in mapping applications.

* [Apple Maps: Space Needle](https://maps.apple.com/?address=400%20Broad%20St,%20Seattle,%20WA%20%2098109,%20United%20States&auid=17457489312301189071&ll=47.620521,-122.349293&lsp=9902&q=Space%20Needle)
* [Google Maps: Space Needle](https://maps.google.com/?address=400%20Broad%20St,%20Seattle,%20WA%20%2098109,%20United%20States&auid=17457489312301189071&ll=47.620521,-122.349293&lsp=9902&q=Space%20Needle)

## Calendar Events

The newer [iCalendar](https://en.wikipedia.org/wiki/ICalendar) (`.ics`) format, as well as the older [vCalendar](https://en.wikipedia.org/wiki/ICalendar#vCalendar_1.0) (`.vcs`) format both define a sub-set of their formats for _events_. These are [vEvents](https://icalendar.org/iCalendar-RFC-5545/3-6-1-event-component.html).

```vevent
BEGIN:VEVENT
SUMMARY:Summer+Vacation!
DTSTART:20180601T070000Z
DTEND:20180831T070000Z
END:VEVENT
```

## Wi-Fi Network config (Android, iOS 11+)

We propose a syntax like "MECARD" for specifying wi-fi configuration. Scanning such a code would, after prompting the user, configure the device's Wi-Fi accordingly. Example:

```
WIFI:T:WPA;S:mynetwork;P:mypass;;
```

| Parameter | Example | Description
| --------- | ------- | -----------
| T | WPA | Authentication type; can be WEP or WPA, or 'nopass' for no password. Or, omit for no password.
| S | mynetwork | Network SSID. Required. Enclose in double quotes if it is an ASCII name, but could be interpreted as hex (i.e. "ABCD")
| P | mypass | Password, ignored if T is "nopass" (in which case it may be omitted). Enclose in double quotes if it is an ASCII name, but could be interpreted as hex (i.e. "ABCD")
| H | true | Optional. True if the network SSID is hidden.

Order of fields does not matter. Special characters "\", ";", "," and ":" should be escaped with a backslash ("\") as in MECARD encoding. For example, if an SSID was literally `"foo;bar\baz"` (with double quotes part of the SSID name itself) then it would be encoded like: `WIFI:S:\"foo\;bar\\baz\";;`

## Platform-specific

### Google Play Store

You can construct URIs that (on Android devices) link directly into Google Play Store. For example to encode a link to an app whose package is `org.example.foo`, use:

```plain
market://details?id=org.example.foo
```

### iTunes and App Store

```plain
# Apple Music, iTunes Music Store
https://geo.itunes.apple.com/us/album/concrete-and-gold/1249068417?mt=1&app=music
https://geo.itunes.apple.com/us/album/concrete-and-gold/1249068417?mt=1&app=itunes

# iBooks Store
https://geo.itunes.apple.com/us/book/crazy-rich-asians/id588213164?mt=11

# App Store
https://itunes.apple.com/us/app/fortnite/id1261357853?mt=8
```

See also…

* [Apple URL Scheme Reference: iTunes Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/iTunesLinks/iTunesLinks.html#//apple_ref/doc/uid/TP40007899-CH3-SW1)
* [iTunes Link Maker](https://linkmaker.itunes.apple.com)

### YouTube

* [Apple URL Scheme Reference: YouTube Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/YouTubeLinks/YouTubeLinks.html#//apple_ref/doc/uid/TP40007899-CH8-SW1)
