# Callbacks

Barcode success is not the package envelope `status: "done"`. The barcode helper passes `BarcodeResult` JSON straight to `veryfiLensSuccess`.

| Key | Meaning |
|-----|---------|
| `barcode_uuid` | Local id. Also the document-ready package id |
| `barcodes` | Array of items |

Each item:

| Key | Meaning |
|-----|---------|
| `data` | Payload string |
| `score` | Float |
| `image` | Path `Documents/barcodeImage{index}.jpg` when a cropped image was saved. Empty string if none |
| `type` | Symbology string on the processed-image callback. The live update path does not set `type` |
| `bounding_region` | `[CGFloat]`. Empty on the live update path |

`veryfiLensUpdate` receives the same JSON shape during live detection, without `type`, `bounding_region`, or an image path.

`veryfiLensClose` follows the scheduled camera close. It is not the barcode payload.

`veryfiLensError` is configure and credential failure.

A document-ready call with engine `.inApp` and `documentType` `barcode` is a notification. It is not the barcode payload. The camera path calls `veryfiLensSuccess` from the barcode helper. Do not wait for a second package-uploader success. `PackageUploader.inAppAPIProcessing` only succeeds when metadata contains `credit_card`.

Whether `autoDeleteLocalResourcesAfterProcessing` deletes `Documents/barcodeImage{index}.jpg` is **Unconfirmed**. Copy the image path during success if the host needs the file.

`shareOCRFeedback()` shares the last **code**-scan logs. It is not the barcode result API.
