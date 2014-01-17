## Android

On Android, you can invoke Barcode Scanner from a web page and have the result returned to your site via a callback URL. For example, when `01234` is scanned, to have the user return to `http://foo.com/products/01234/description`, simply link to a URL like this, where `{CODE}` is a placeholder for the value of the returned code:

`http://zxing.appspot.com/scan?ret=http%3A%2F%2Ffoo.com%2Fproducts%2F%7BCODE%7D%2Fdescription&SCAN_FORMATS=UPC_A,EAN_13`

Note that the URL in the `ret=` parameter is [URL-escaped](http://www.blooberry.com/indexdot/html/topics/urlencoding.htm#whatwhy), and that `{CODE}` is used as a placeholder for the scanned value (above, it appears as `%7BCODE%7D`). `SCAN_FORMATS`, and other parameters, can be set here as well to control the scan behavior. For example is can be used to supply a comma-separated list of [format names](http://code.google.com/p/zxing/source/browse/trunk/core/src/com/google/zxing/BarcodeFormat.java).

Additional placeholders are available in the URL pattern:

  * `{CODE}`: the formatted content of the barcode, or the raw content (unparsed) if `raw=true`
  * `{RAWCODE}`: the raw content (unparsed)
  * `{FORMAT}`: the name of the barcode format scanned like `UPC_A`
  * `{TYPE}`: the name of the type of parsed content, like `PRODUCT`
  * `{META}`: a string representation of the scan metadata, like `{POSSIBLE_COUNTRY=US/CA}`

_These are not available in other platform implementations, like the iOS app._

The following URL also works, and some have found it works where the HTTP URL above doesn't. This also shows usage of `SCAN_FORMATS`:

`zxing://scan/?ret=http%3A%2F%2Ffoo.com%2Fproducts%2F%7BCODE%7D%2Fdescription&SCAN_FORMATS=UPC_A,EAN_13`

## iPhone

Note that this functionality will be available on the iPhone app `Barcodes`. iOS works differently, and so the URL pattern must instead use the `zxing://scan/` URL:

`zxing://scan/?ret=http%3A%2F%2Ffoo.com%2Fproducts%2F%7BCODE%7D%2Fdescription&SCAN_FORMATS=UPC_A,EAN_13`

Only the `{CODE}` placeholder is available at the moment.

## Custom search URL

Separately, users can specify a custom search URL to invoke when a barcode is scanned with the Android app Barcode Scanner. When set under Settings, a Custom Search button will appear. The search URL can contain two placeholders: `%`s for the barcode content, `%t` the type, and `%f` for the format. For example, `http://example.org/?q=%s&format=%f&type=%t` might invoke, on a scan, a URL like `http://example.org/?q=10359050900&format=EAN_13&type=PRODUCT`