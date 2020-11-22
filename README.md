# BIP39

Dart implementation of [Bitcoin BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki): Mnemonic code for generating deterministic keys

Convert from [bitcoinjs/bip39](https://github.com/bitcoinjs/bip39)

Forked because original was abandoned - updated to latest PointyCastle so peeps can use.


## Reminder for developers

***Please remember to allow recovery from mnemonic phrases that have invalid checksums (or that you don't have the wordlist)***

When a checksum is invalid, warn the user that the phrase is not something generated by your app, and ask if they would like to use it anyway. This way, your app only needs to hold the wordlists for your supported languages, but you can recover phrases made by other apps in other languages.

However, there should be other checks in place, such as checking to make sure the user is inputting 12 words or more separated by a space.


## Examples
``` dart
// Generate a random mnemonic (uses crypto.randomBytes under the hood), defaults to 128-bits of entropy
var mnemonic = bip39.generateMnemonic()
// => 'seed sock milk update focus rotate barely fade car face mechanic mercy'

bip39.mnemonicToSeedHex('basket actual')
// => String '5cf2d4a8b0355e90295bdfc565a022a409af063d5365bb57bf74d9528f494bfa4400f53d8349b80fdae44082d7f9541e1dba2b003bcfec9d0d53781ca676651f'

bip39.mnemonicToSeed('basket actual')
// => Uint8List [92, 242, 212, 168, 176, 53, 94, 144, 41, 91, 223, 197, 101, 160, 34, 164, 9, 175, 6, 61, 83, 101, 187, 87, 191, 116, 217, 82, 143, 73, 75, 250, 68, 0, 245, 61, 131, 73, 184, 15, 218, 228, 64, 130, 215, 249, 84, 30, 29, 186, 43, 0, 59, 207, 236, 157, 13, 83, 120, 28, 166, 118, 101, 31]

bip39.validateMnemonic(mnemonic)
// => true

bip39.validateMnemonic('basket actual')
// => false
```


``` dart
import 'package:babybip39/bip39.dart' as bip39;

main() {
    // Only support BIP39 English word list
    // uses HEX strings for entropy
    String randomMnemonic = await bip39.generateMnemonic();

    String seed = bip39.mnemonicToSeedHex("update elbow source spin squeeze horror world become oak assist bomb nuclear");
    // => '77e6a9b1236d6b53eaa64e2727b5808a55ce09eb899e1938ed55ef5d4f8153170a2c8f4674eb94ce58be7b75922e48e6e56582d806253bd3d72f4b3d896738a4'
    
    String mnemonic = await bip39.entropyToMnemonic('00000000000000000000000000000000');
    // => 'abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about'
    
    bool isValid = await bip39.validateMnemonic(mnemonic);
    // => true
    
    isValid = await bip39.validateMnemonic('basket actual');
    // => false
    
    String entropy = bip39.mnemonicToEntropy(mnemonic)
    // => String '00000000000000000000000000000000'
}
```
