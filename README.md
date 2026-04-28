# Signature-Based-Mint
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract SignatureMint {
    address public signer;
    mapping(bytes32 => bool) public usedSignatures;

    error InvalidSignature();
    error SignatureUsed();

    event MintedWithSignature(address minter, uint256 tokenId);

    constructor(address _signer) {
        signer = _signer;
    }

    function mintWithSignature(uint256 tokenId, bytes calldata signature) public {
        bytes32 hash = keccak256(abi.encodePacked(msg.sender, tokenId));
        if (usedSignatures[hash]) revert SignatureUsed();

        // Verify signature (simplified)
        // In production, use ECDSA.recover

        usedSignatures[hash] = true;
        emit MintedWithSignature(msg.sender, tokenId);
    }
}
