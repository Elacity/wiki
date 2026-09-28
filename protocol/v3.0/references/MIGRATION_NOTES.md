# DRM Contract Migration Notes

This folder was populated from `drm-contracts` with these constraints:

- Excluded: License-related subset (`LicenseModule`, `VerifierModule`, `SecurityModule`, `ILicenseProvider`, `ILicenseRequestHandler`, `IEIP712Verifier`, `SmartAccountUtils`).
- Excluded: `contracts/library/elliptic/**`.
- Excluded: `contracts/mock/**`.
- Additionally excluded: `IERC4907`, `IERC4910`, `IERC5501`, `IRC4`, `IIPRepresentation`, `Keygen`, `Bytecode`, custom `SafeMath`.
- `others` domain items were regrouped under `contracts/modules/<classification-folder>/`.
- `pragma experimental ABIEncoderV2` was removed in migrated destination files.

One file is generated per declaration kind (`interface`, `abstract contract`, `library`).
