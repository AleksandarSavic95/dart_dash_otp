# dart_otp - Null Safety

This is a fork of the [dart_otp](https://github.com/factisresearch/dart_otp/blob/master/test/totp_test.dart) repository, thank you for this contribution. Currently the original repo is not compatible with null safety so we made an update to make it compatible with newer versions of dart, e.g. 2.12.0.
___



`dart_otp` is a dart package to generate and verify one-time passwords that were used to implement 2FA and MFA authentication method in web applications and other login-required systems.

The package was implement based on [RFC4226](https://tools.ietf.org/html/rfc4226) (HOTP: An HMAC-Based One-Time Password Algorithm) and [RFC6238](https://tools.ietf.org/html/rfc6238) (TOTP: Time-Based One-Time Password Algorithm).

## Feature

* Create and verify a HOTP object
* Create and verify a TOTP object
* Generate a `otpauth url` with the b32 encoded string
* Support for OTP tokens encrypted with SHA1, SHA256, SHA384 and SHA512

### Installation

#### Pubspec

Add `dart_otp` as a dependency in your `pubspec.yaml` file.

```yaml
dependencies:
  dart_dash_otp: ^1.3.2
```

### Example

#### Time-based OTPs

```dart
import 'package:dart_otp/dart_otp.dart';

void main() {

  /// default initialization for intervals of 30 seconds and 6 digit tokens
  TOTP totp = TOTP(secret: "J22U6B3WIWRRBTAV");

  /// initialization for custom interval and digit values
  TOTP totp = TOTP(secret: "J22U6B3WIWRRBTAV", interval: 60, digits: 8);

  totp.now(); /// => 432143

  /// verify for the current time
  totp.verify(otp: 432143); /// => true

  /// verify after 30s
  totp.verify(otp: 432143); /// => false
}
```

#### Counter-based OTPs

```dart
import 'package:dart_otp/dart_otp.dart';

void main() {

  /// default initialization 6 digit tokens
  HOTP hotp = HOTP(secret: "J22U6B3WIWRRBTAV");

  /// initialization for custom digit value
  HOTP hotp = HOTP(secret: "J22U6B3WIWRRBTAV", counter: 50, digits: 8);

  hotp.at(counter: 0); /// => 432143
  hotp.at(counter: 1); /// => 231434
  hotp.at(counter: 2132); /// => 242432

  /// verify with a counter
  hotp.verify(otp: 242432, counter: 2132); /// => true
  hotp.verify(otp: 242432, counter: 2133); /// => false
}
```

### Release notes

See [CHANGELOG.md](./CHANGELOG.md).
