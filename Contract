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
