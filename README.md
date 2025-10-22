# yEnc-FPE Encryption Standard

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Experimental](https://img.shields.io/badge/Status-Experimental-red.svg)]()

This repository contains the specification for the **yEnc-FPE (yEnc Format-Preserving Encryption)** standard, a reversible encryption scheme designed specifically for yEnc-encoded binary blocks used in Usenet transfers.

## Overview

yEnc-FPE provides a method to encrypt yEnc message blocks while maintaining:

- **Format compatibility**: Output remains valid yEnc-encoded text
- **Length preservation**: Ciphertext length equals plaintext length (±1 sentinel byte)
- **Usenet safety**: Maintains printable character range required for Usenet transmission
- **Deterministic reconstruction**: Perfect recovery of original yEnc structure
- **No additional encoding required**: Encrypted output is directly usable without further yEnc encoding

The standard uses **AES-FF1** (NIST SP 800-38G compliant Format-Preserving Encryption) with a 253-character alphabet covering all yEnc-compatible bytes.

## Key Features

- **Reversible encryption** of complete yEnc blocks (header, body, footer)
- **Format-preserving** output suitable for direct Usenet transmission
- **No re-encoding needed**: Encrypted output is already yEnc-compatible and ready for transmission
- **Deterministic reconstruction** of yEnc structure including line breaks and escape sequences
- **Segment-aware** encryption with unique tweaks per upload segment
- **Password-based** key derivation using Argon2id
- **Length-preserving** with configurable line wrapping

## Use Cases

- **Multipart binary uploads**: Encrypt individual yEnc segments while maintaining format
- **Content protection**: Add confidentiality layer to Usenet binaries without breaking compatibility
- **Segment encryption**: Encrypt specific parts of multipart uploads
- **Format-preserving obfuscation**: Maintain yEnc structure while encrypting content

## Specification

The complete specification is available in [`yEnc-FPE Encryption Standard.txt`](./yEnc-FPE%20Encryption%20Standard.txt).

### Quick Overview

1. **Normalization**: Remove line breaks, insert sentinel byte between header and body
2. **Encryption**: Apply AES-FF1 over 253-character alphabet with segment-specific tweaks
3. **Formatting**: Add line breaks for display while preserving cryptographic integrity
4. **Decryption**: Reverse process with perfect reconstruction of original yEnc structure

### Algorithm Summary

```
Alphabet: 253 bytes (0x01-0xFF excluding CR/LF)
Cipher: AES-FF1 (AES-256 with Format-Preserving Encryption)
Key Derivation: Argon2id(password, salt)
Tweak: HMAC-SHA256(master, "ff1/tweak" || segmentIndex)[0:8]
Salt: 16 random bytes from yEnc-compatible range
```

## Security Properties

- **Confidentiality**: AES-256 equivalent security under unique key/tweak pairs
- **Deterministic**: Identical plaintext produces identical ciphertext (with same key/tweak)
- **Format-preserving**: No information leakage through format changes
- **Segment isolation**: Each segment encrypted with unique tweak prevents cross-segment attacks

**Note**: This scheme provides confidentiality only, not integrity or authenticity verification.

## Implementation Considerations

- Requires FF1-compatible library supporting custom alphabets
- Must handle yEnc escape sequences correctly during reconstruction
- Segment indexing must be globally unique across entire upload session
- Line break insertion must respect yEnc escape sequence boundaries

## Status

**Current Version**: 0.1 (Experimental)  
**Last Updated**: October 22, 2025

This specification is currently in **experimental** status. The standard is being developed collaboratively and may undergo changes based on community feedback and security review.

## Contributing

We welcome contributions to improve and refine the yEnc-FPE standard:

### Discussions

For general questions, implementation discussions, or conceptual feedback, please use the [**Discussions**](../../discussions) section.

### Issues and Change Requests

- **Bug reports**: Found an error in the specification? Please [open an issue](../../issues/new).
- **Enhancement proposals**: Suggestions for improvements should be submitted as [issues](../../issues/new) with detailed rationale.
- **Specification changes**: Proposed modifications to the standard should be submitted as [pull requests](../../pulls) with clear justification and impact analysis.

### Pull Requests

When submitting PRs for specification changes:

1. Clearly describe the motivation and impact
2. Update relevant sections consistently
3. Consider backward compatibility implications
4. Include example scenarios if applicable

## Related Work

- [NIST SP 800-38G](https://csrc.nist.gov/publications/detail/sp/800-38g/final): FF1 and FF3 Format-Preserving Encryption specification
- [yEnc specification](http://www.yenc.org/yenc-draft.1.3.txt): Original yEnc encoding format
- [RFC 7539](https://tools.ietf.org/html/rfc7539): ChaCha20 and Poly1305 (alternative cipher considerations)

## License

This specification is released under the MIT License. See [LICENSE](LICENSE) for details.

---

**Maintainer**: [@Tensai75](https://github.com/Tensai75)  
**Repository**: [github.com/Tensai75/yenc-fpe-standard](https://github.com/Tensai75/yenc-fpe-standard)
