# ♥️♣️ Guess The Suit 🎴

A fun and simple **Ethereum smart contract game** where players try to guess the suit of a virtual playing card — **Hearts, Diamonds, Clubs, or Spades**!  
This project is perfect for beginners exploring **Solidity**, **smart contracts**, and **blockchain randomness** concepts.

---

## 🧩 Project Description

**GuessTheSuit** is a Solidity smart contract that simulates a simple card-guessing game on the blockchain.  
Players make a guess for the card suit, and the contract randomly selects one using pseudo-random logic.  
It’s a great example to learn about enums, mappings, events, and basic randomness in Ethereum smart contracts.

> ⚠️ *Note: This contract uses pseudo-randomness based on block data and should not be used for real-money gambling or production environments.*

---

## ⚙️ What It Does

1. A player calls the `play()` function and provides a number between **0 and 3**:
   - `0 = Hearts`
   - `1 = Diamonds`
   - `2 = Clubs`
   - `3 = Spades`
2. The contract generates a pseudo-random suit.
3. If the player’s guess matches the random result — **they win!**
4. Each game is recorded on-chain and can be retrieved for transparency.

---

## 🌟 Features

- 🎮 **Interactive On-Chain Game** – Play directly from Remix or a dApp interface.  
- 🔢 **Enum Usage** – Demonstrates Solidity enums (`Suit { Hearts, Diamonds, Clubs, Spades }`).  
- 🔄 **Event Logging** – Emits `GamePlayed` event for every attempt.  
- 🧾 **Game History** – Stores each play in the `games` mapping for full traceability.  
- 🧠 **Educational Example** – Perfect for learning Solidity basics like randomness, structs, mappings, and events.  

---

## 📜 Deployed Smart Contract

**Network:** Celo (or any EVM-compatible testnet)  
**Contract Address:** `0x637Ecdac85902d1A601A49c7E7D7e0d9f6c04b48`  
*(Replace `XXX` with your deployed contract address once deployed.)*

---

## 💻 Solidity Code

Here’s the complete smart contract code used in this project:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/**
 * @title GuessTheSuit
 * @dev A simple game where users guess the suit of a card.
 * NOTE: This uses pseudo-randomness — not secure for real money!
 */
contract GuessTheSuit {
    enum Suit { Hearts, Diamonds, Clubs, Spades }

    address public owner;
    uint256 public gameCount;

    struct GameResult {
        address player;
        Suit guess;
        Suit actual;
        bool won;
    }

    mapping(uint256 => GameResult) public games;

    event GamePlayed(address indexed player, Suit guess, Suit actual, bool won);

    constructor() {
        owner = msg.sender;
    }

    /**
     * @dev Player guesses the suit (0=Hearts, 1=Diamonds, 2=Clubs, 3=Spades)
     */
    function play(uint8 _guess) external returns (bool) {
        require(_guess < 4, "Invalid suit");

        // Generate pseudo-random number (not secure)
        uint256 rand = uint256(
            keccak256(abi.encodePacked(block.timestamp, msg.sender, gameCount))
        ) % 4;

        Suit actual = Suit(rand);
        bool won = (uint8(actual) == _guess);

        games[gameCount] = GameResult(msg.sender, Suit(_guess), actual, won);
        emit GamePlayed(msg.sender, Suit(_guess), actual, won);

        gameCount++;
        return won;
    }

    /**
     * @dev Helper: Get string name of a suit
     */
    function getSuitName(uint8 _suit) public pure returns (string memory) {
        if (_suit == 0) return "Hearts";
        if (_suit == 1) return "Diamonds";
        if (_suit == 2) return "Clubs";
        if (_suit == 3) return "Spades";
        return "Unknown";
    }
}
