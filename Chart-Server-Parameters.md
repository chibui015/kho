The web application provided by this project at http://zxing.org (and in the source code under `zxingorg/`) provides a re-implementation of the now-deprecated [Google Chart Server API](https://google-developers.appspot.com/chart/infographics/docs/qr_codes) for QR code encoding. The documentation for this API is reproduced here.

The endpoint is at `/w/chart`. It responds to `GET`, but also `POST` requests to specify the encoded data (see `chl` below). The response is, by default, a PNG image. `/w/chart.png` also works and returns PNG. `/w/chart.gif` returns a GIF image, and `/w/chart.jpeg` or `/w/chart.jpg` returns a JPEG.

| Parameter | Required? | Description |
| --------- | --------- | ----------- |
| `cht=qr` | Required | Specifies a QR code. |
| `chs=<width>x<height>` | Required | Image size. |
| `chl=<data>` | Required | The data to encode. Data can be digits (0-9), alphanumeric characters, binary bytes of data, or Kanji. You cannot mix data types within a QR code. The data must be UTF-8 URL-encoded. Note that URLs have a 2K maximum length, so if you want to encode more than 2K bytes (minus the other URL characters), you will have to send your data using POST. |
| `choe=<output_encoding>` | Optional | How to encode the data in the QR code. Here are the available values: <ul><li>`UTF-8` [Default]</li><li>`Shift_JIS`</li><li>`ISO-8859-1`</li></ul> |
| `chld=<error_correction_level>\|<margin>` | Optional	| <ul><li>`error_correction_level` - QR codes support four levels of error correction to enable recovery of missing, misread, or obscured data. Greater redundancy is achieved at the cost of being able to store less data. See the appendix for details. Here are the supported values: <ul><li>L - [Default] Allows recovery of up to 7% data loss</li><li>M - Allows recovery of up to 15% data loss</li><li>Q - Allows recovery of up to 25% data loss</li><li>H - Allows recovery of up to 30% data loss</li></ul></li><li>`margin` - The width of the white border around the data portion of the code. This is in rows, not in pixels. (See below to learn what rows are in a QR code.) The default value is 4.</li></ul> |

## Example

https://zxing.org/w/chart?cht=qr&chs=350x350&chld=H%7C6&choe=UTF-8&chl=Hello+World

![](https://zxing.org/w/chart?cht=qr&chs=350x350&chld=H%7C6&choe=UTF-8&chl=Hello+World)